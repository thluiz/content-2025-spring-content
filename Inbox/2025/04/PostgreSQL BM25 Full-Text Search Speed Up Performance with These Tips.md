---
created: 2025-04-09T18:33:03 (UTC -03:00)
tags: []
source: https://blog.vectorchord.ai/postgresql-full-text-search-fast-when-done-right-debunking-the-slow-myth?utm_source=tldrnewsletter
author: Jinjing Zhou
---

# PostgreSQL BM25 Full-Text Search: Speed Up Performance with These Tips

> ## Excerpt
> Boost PostgreSQL full-text search speed by 50x with simple optimizations. Use VectorChord-BM25 to accelerate and better BM25 ranking in postgres.

---
You might have come across discussions or blog posts suggesting that PostgreSQL's built-in full-text search (FTS) struggles with performance compared to dedicated search engines or specialized extensions. A notable recent example comes from **Neon's blog post, "Performance Benchmark: pg\_search on Neon"** ([link](https://neon.tech/blog/pgsearch-on-neon)).

In their benchmark, Neon compared query performance on their database platform _with_ their `pg_search` extension (based on Rust's Tantivy library via `pgrx`) against the Postgres built-in fulltext search setting with tsvector and GIN index. They commendably stated they optimized this standard setup by adding GIN indexes where appropriate (benchmark code available [here](https://github.com/paradedb/paradedb/tree/dev/benchmarks)).

However, while adding GIN indexes is a necessary first step, their results showing significantly slower performance for the "standard" setup suggest crucial _additional_ optimization steps for PostgreSQL FTS were likely missed. The conclusion that standard FTS is inherently much slower than `pg_search` might be based on an unintentionally handicapped baseline.

Let's dive into how to _correctly_ set up and use standard PostgreSQL FTS for optimal performance, addressing the specific configuration flaws likely present in the baseline used in the Neon benchmark, and demonstrating the true speed of built-in FTS. We'll show concrete numbers demonstrating a **~50x performance increase** just by applying these standard optimizations to the baseline configuration.

## **The Benchmark Setup (Recap from Neon's Analysis)**

The analysis used a table structure similar to this with 10 million log entries:

```
<span>CREATE</span> <span>TABLE</span> benchmark_logs (
    <span>id</span> <span>SERIAL</span> PRIMARY <span>KEY</span>,
    message <span>TEXT</span>,
    country <span>VARCHAR</span>(<span>255</span>),
    severity <span>INTEGER</span>,
    <span>timestamp</span> <span>TIMESTAMP</span>,
    metadata JSONB
);
```

They tested various query types. Many involved searching the `message` text field, using queries structured like this in their "standard Postgres" examples:

```
<span>-- Example problematic query structure (likely used in Neon's baseline)</span>
<span>SELECT</span> country, <span>COUNT</span>(*)
<span>FROM</span> benchmark_logs
<span>WHERE</span> to_tsvector(<span>'english'</span>, message) @@ to_tsquery(<span>'english'</span>, <span>'research'</span>)
<span>GROUP</span> <span>BY</span> country
<span>ORDER</span> <span>BY</span> country;
```

Even with a GIN index present on `message`, this query structure and the likely default GIN index settings are where the performance issues for the _standard FTS baseline_ begin.

#### **Our Test Environment Details (Replicating the Baseline Scenario)**

To demonstrate the impact of proper optimization on standard FTS, we used the following setup:

-   **Instance:** AWS EC2 `i7ie.xlarge` with **local NVMe SSD** (minimizing I/O impact).
    
-   **CPU:** **4 vCPUs**.
    
-   **PostgreSQL Configuration:** PostgreSQL 16 via Docker, configured for the hardware:
    
    ```
      docker run -d \
        --name my-postgres \
        --network=host \
        -v /mnt/localssd/pgdata:/var/lib/postgresql/data \
        -e POSTGRES_PASSWORD=mysecretpassword \
        postgres:latest \
        -c shared_buffers=8GB \
        -c maintenance_work_mem=8GB \
        -c max_parallel_workers=4 \
        -c max_worker_processes=4
    ```
    
-   **Parallelism Note:** This setup provides `max_parallel_workers_per_gather = 2`. The Neon post mentioned their test environment used 8 parallel workers due to larger instances, implying potentially higher parallelism (`max_parallel_workers_per_gather` up to 8) than our setup. Our optimized results for _standard_ FTS were achieved with _less_ query parallelism.
    

**Mistake #1:** Calculating `tsvector` On-the-Fly (Major issue)

The sample queries shown in the Neon blog (and common in basic FTS examples) calculate the `tsvector` within the `WHERE` clause:

```
WHERE to_tsvector('english', message) @@ to_tsquery('english', 'research')
```

This forces PostgreSQL to:

1.  **Perform Expensive Computation:** Run `to_tsvector()` (parsing, stemming, etc.) repeatedly for many rows during query execution.
    
2.  **Limit Index Efficiency:** Prevent the most direct and efficient use of the GIN index, even if one exists on the base `message` column.
    

#### **The Fix (For a Proper Standard FTS Baseline):** Pre-calculate and store the `tsvector`.

1.  **Add a** `tsvector` column:
    
    ```
     <span>ALTER</span> <span>TABLE</span> benchmark_logs <span>ADD</span> <span>COLUMN</span> message_tsvector tsvector;
    ```
    
2.  **Populate the column:**
    
    ```
     <span>UPDATE</span> benchmark_logs <span>SET</span> message_tsvector = to_tsvector(<span>'english'</span>, message);
    ```
    
3.  **Index the** `tsvector` column (with `fastupdate=off`): (As shown in Fix #1)
    
4.  **Rewrite queries:**
    
    ```
     <span>-- Optimized standard FTS query</span>
     <span>SELECT</span> country, <span>COUNT</span>(*)
     <span>FROM</span> benchmark_logs
     <span>WHERE</span> message_tsvector @@ to_tsquery(<span>'english'</span>, <span>'research'</span>) <span>-- Use the indexed column!</span>
     <span>GROUP</span> <span>BY</span> country
     <span>ORDER</span> <span>BY</span> country;
    ```
    

**Mistake #2 :** Ignoring GIN Index `fastupdate` (Minor issue)

While Neon's benchmark correctly identified GIN as the index type for standard FTS, it likely used the default setting: `fastupdate=on`.

-   `fastupdate=on` (Default): Prioritizes index _update_ speed by using pending lists. This significantly slows down _searches_ over time, especially on large, static datasets used for benchmarks, as both the main index and growing pending lists must be scanned. It also contributes to index bloat and less efficient page structure.
    
-   `fastupdate=off`: Prioritizes _search_ speed. Index updates are slower, but the resulting index is more compact and significantly faster to query as there are no pending lists to scan. Essential for read-heavy workloads or benchmarking search performance.
    

For a fair comparison focused on _search_ speed, the standard FTS baseline should have used `fastupdate=off`.

**The Fix (For a Proper Standard FTS Baseline):**

```
<span>-- Create the GIN index correctly for search speed</span>
<span>CREATE</span> <span>INDEX</span> idx_gin_logs_message_tsvector
<span>ON</span> benchmark_logs <span>USING</span> GIN (message_tsvector) <span>-- Index the dedicated tsvector column</span>
<span>WITH</span> (fastupdate = <span>off</span>);
```

#### **The Performance Impact: A ~50x Speedup for Standard FTS**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1743581394771/adecbc81-f0d7-4bf2-9696-1206b197386b.png?auto=compress,format&format=webp)

What happens when we apply these _standard_ optimizations to the _standard_ PostgreSQL FTS setup? We tested a query in Neon's examples on the 10-million-row dataset:

-   **Unoptimized Standard FTS (Neon's Baseline):** ~41301 ms (41.3 seconds)
    
-   **Optimized Standard FTS (Our Fixes):** ~877 ms (0.88 seconds)
    

This demonstrates a **~50x speed improvement** for _standard PostgreSQL FTS_ achieved simply by applying well-established best practices. This transforms the baseline performance dramatically, suggesting the comparison point used in the Neon benchmark may not have represented the true potential of built-in FTS. This speedup was achieved even with potentially less query parallelism available than in Neon's tests.

## **The Grain of Truth: Ranking (**`ts_rank`**) Performance**

Neon's analysis _did_ likely highlight a valid point regarding ranking speed. Functions like `ts_rank` or `ts_rank_cd` _can_ be relatively slow in standard PostgreSQL compared to just finding matches. This is because ranking requires fetching and processing data for all matching rows _before_ limiting, which can be I/O and CPU intensive.

## **Beyond Standard FTS: Advanced Ranking with VectorChord-BM25**

While standard FTS excels at _finding_ matches quickly, if sophisticated, high-performance _ranking_ is critical, specialized tools are often required. Instead of relying solely on built-in functions, consider [**VectorChord-BM25**](https://github.com/tensorchord/VectorChord-bm25).

[**VectorChord-BM25**](https://github.com/tensorchord/VectorChord-bm25) is a PostgreSQL extension specifically designed for fast, relevance-based ranking using the well-regarded **BM25 algorithm**. BM25 goes beyond simple term matching, estimating relevance based on term frequency within a document and inverse document frequency across the entire collection.

![Benchmark against ElasticSearch](https://cdn.hashnode.com/res/hashnode/image/upload/v1740189817247/5a81c020-1aa2-452d-8e8d-b9653c5b3489.png?auto=compress,format&format=webp&auto=compress,format&format=webp)

Key advantages reported for VectorChord-BM25 include:

-   **High Performance:** Designed for speed, reportedly outperforming other solutions like Elasticsearch (up to 3x faster) and potentially pg\_search for ranking tasks.
    
-   **BM25 Algorithm:** Implements a standard, effective relevance ranking model.
    
-   **Specialized Indexing:** Uses a dedicated bm25 index type (leveraging optimizations like Block WeakAnd) which is crucial for both performance and calculating the global statistics needed for BM25 scoring.
    
-   **Dedicated Data Type:** Introduces a bm25vector type to store tokenized representations suitable for BM25.
    

If your application heavily relies on the quality and speed of relevance ranking, and standard ts\_rank proves insufficient, [**VectorChord-BM25**](https://github.com/tensorchord/VectorChord-bm25) presents a powerful, integrated alternative within PostgreSQL.

## **Conclusion: Standard FTS is Faster Than You Might Think**

Standard PostgreSQL FTS is a highly capable and performant feature for _finding_ documents when correctly optimized using tsvector columns and properly configured GIN indexes (with fastupdate=off for reads). Benchmarks suggesting otherwise may be comparing against unoptimized baselines.

For advanced _relevance ranking_ where standard `ts_rank` falls short, specialized extensions like **VectorChord-BM25** offer significant performance improvements by implementing algorithms like BM25 with dedicated data types and optimized indexing strategies.

Choose the right tool for the job: optimize standard FTS first, and if high-performance relevance ranking is paramount, explore dedicated extensions built for that purpose.
