---
created: 2024-09-27T07:34:14 (UTC -03:00)
tags: []
source: https://duckdb.org/why_duckdb.html
author: GitHub User
---

# Why DuckDB – DuckDB

> ## Excerpt
> There are many database management systems (DBMS) out there. But there is no one-size-fits all database system. All take different trade-offs to better adjust to specific use cases. DuckDB is no different. Here, we try to explain what goals DuckDB has and why and how we try to achieve those goals through technical means. To start with, DuckDB is a relational (table-oriented) DBMS that supports the Structured Query Language (SQL). Simple SQLite is the world's most widely deployed DBMS. Simplicity in installation, and embedded in-process operation are central to its success. DuckDB adopts these ideas of simplicity and embedded operation.…

---
There are many database management systems (DBMS) out there. But there is [no one-size-fits all database system](https://blobs.duckdb.org/papers/stonebraker-centintemel-one-size-fits-all-icde-2015.pdf). All take different trade-offs to better adjust to specific use cases. DuckDB is no different. Here, we try to explain what goals DuckDB has and why and how we try to achieve those goals through technical means. To start with, DuckDB is a [relational (table-oriented) DBMS](https://en.wikipedia.org/wiki/Relational_database) that supports the [Structured Query Language (SQL)](https://en.wikipedia.org/wiki/SQL).

## [Simple](https://duckdb.org/why_duckdb.html#simple)

SQLite is the [world's most widely deployed DBMS](https://www.sqlite.org/mostdeployed.html). Simplicity in installation, and embedded in-process operation are central to its success. DuckDB adopts these ideas of simplicity and embedded operation.

DuckDB has **no external dependencies**, neither for compilation nor during run-time. For releases, the entire source tree of DuckDB is compiled into two files, a header and an implementation file, a so-called "amalgamation". This greatly simplifies deployment and integration in other build processes. For building, all that is required to build DuckDB is a working C++11 compiler.

For DuckDB, there is no DBMS server software to install, update and maintain. DuckDB does not run as a separate process, but completely **embedded within a host process**. For the analytical use cases that DuckDB targets, this has the additional advantage of **high-speed data transfer** to and from the database. In some cases, DuckDB can process foreign data without copying. For example, the DuckDB Python package can run queries directly on Pandas data without ever importing or copying any data.

## [Portable](https://duckdb.org/why_duckdb.html#portable)

Thanks to having no dependencies, DuckDB is extremely portable. It can be compiled for all major operating systems (Linux, macOS, Windows) and CPU architectures (x86, ARM). It can be deployed from small, resource-constrained edge devices to large multi-terabyte memory servers with 100+ CPU cores. Using [DuckDB-Wasm](https://duckdb.org/docs/api/wasm/overview.html), DuckDB can also run in web browsers and even on mobile phones.

DuckDB provides [APIs for Java, C, C++, Go, Node.js and other languages](https://duckdb.org/docs/api/overview.html).

## [Feature-Rich](https://duckdb.org/why_duckdb.html#feature-rich)

DuckDB provides serious data management features. There is extensive support for **complex queries** in SQL with a large function library, window functions, etc. DuckDB provides **transactional guarantees** (ACID properties) through our custom, bulk-optimized [Multi-Version Concurrency Control (MVCC)](https://en.wikipedia.org/wiki/Multiversion_concurrency_control). Data can be stored in persistent, **single-file databases**. DuckDB supports secondary indexes to speed up queries trying to find a single table entry.

DuckDB is deeply integrated into Python and R for efficient interactive data analysis.

## [Fast](https://duckdb.org/why_duckdb.html#fast)

DuckDB is designed to support **analytical query workloads**, also known as [online analytical processing (OLAP)](https://en.wikipedia.org/wiki/Online_analytical_processing). These workloads are characterized by complex, relatively long-running queries that process significant portions of the stored dataset, for example aggregations over entire tables or joins between several large tables. Changes to the data are expected to be rather large-scale as well, with several rows being appended, or large portions of tables being changed or added at the same time.

To efficiently support this workload, it is critical to reduce the amount of CPU cycles that are expended per individual value. The state of the art in data management to achieve this are either [vectorized or just-in-time query execution engines](https://www.vldb.org/pvldb/vol11/p2209-kersten.pdf). DuckDB contains a **columnar-vectorized query execution engine**, where queries are still interpreted, but a large batch of values (a "vector") are processed in one operation. This greatly reduces overhead present in traditional systems such as PostgreSQL, MySQL or SQLite which process each row sequentially. Vectorized query execution leads to far better performance in OLAP queries.

## [Extensible](https://duckdb.org/why_duckdb.html#extensible)

DuckDB offers a [flexible extension mechanism](https://duckdb.org/docs/extensions/overview.html) that allows defining new data types, functions, file formats and new SQL syntax. In fact, many of DuckDB's key features, such as support for the [Parquet file format](https://duckdb.org/docs/data/parquet/overview.html), [JSON](https://duckdb.org/docs/extensions/json.html), [time zones](https://duckdb.org/docs/extensions/icu.html), and supports for the [HTTP(S) and S3 protocols](https://duckdb.org/docs/extensions/httpfs/overview.html) are implemented as extensions. Extensions also [work in DuckDB Wasm](https://duckdb.org/2021/10/29/duckdb-wasm.html).

## [Free](https://duckdb.org/why_duckdb.html#free)

DuckDB's development started while the main developers were public servants in the Netherlands. We see it as our responsibility and duty to society to make the results of our work freely available to anyone in the Netherlands or elsewhere. This is why DuckDB is released under the very permissive [MIT License](https://en.wikipedia.org/wiki/MIT_License). DuckDB is open-source, the entire source code is freely available on GitHub. We invite contributions from anyone provided they adhere to our [Code of Conduct](https://duckdb.org/code_of_conduct.html).

## [Thorough Testing](https://duckdb.org/why_duckdb.html#thorough-testing)

While DuckDB was originally created by a research group, it was never intended to be a research prototype. Instead, it was intended to become a stable and mature database system. To facilitate this stability, DuckDB is intensively tested using [Continuous Integration](https://github.com/duckdb/duckdb/actions). DuckDB's test suite currently contains millions of queries, and includes queries adapted from the test suites of SQLite, PostgreSQL, and MonetDB. Tests are repeated on a wide variety of platforms and compilers. Every pull request is checked against the full test setup and only merged if it passes.

In addition to this test suite, we run various tests that stress DuckDB under heavy loads. We run the TPC-H and TPC-DS benchmarks, and run various tests where DuckDB is used by many clients in parallel.

## [Peer-Reviewed Papers and Thesis Works](https://duckdb.org/why_duckdb.html#peer-reviewed-papers-and-thesis-works)

-   [These Rows Are Made for Sorting and That's Just What We'll Do](https://duckdb.org/pdf/ICDE2023-kuiper-muehleisen-sorting.pdf) (ICDE 2023)
-   [Join Order Optimization with (Almost) No Statistics](https://blobs.duckdb.org/papers/tom-ebergen-msc-thesis-join-order-optimization-with-almost-no-statistics.pdf) (Master thesis, 2022)
-   [DuckDB-Wasm: Fast Analytical Processing for the Web](https://duckdb.org/pdf/VLDB2022-kohn-duckdb-wasm.pdf) (VLDB 2022 Demo)
-   [Data Management for Data Science - Towards Embedded Analytics](https://duckdb.org/pdf/CIDR2020-raasveldt-muehleisen-duckdb.pdf) (CIDR 2020)
-   [DuckDB: an Embeddable Analytical Database](https://duckdb.org/pdf/SIGMOD2019-demo-duckdb.pdf) (SIGMOD 2019 Demo)

## [Other Projects](https://duckdb.org/why_duckdb.html#other-projects)

To learn about projects using DuckDB, visit the [Awesome DuckDB repository](https://github.com/davidgasquez/awesome-duckdb).

## [Standing on the Shoulders of Giants](https://duckdb.org/why_duckdb.html#standing-on-the-shoulders-of-giants)

DuckDB uses some components from various Open-Source projects and draws inspiration from scientific publications. We are very grateful for this. Here is an overview:

-   **Execution engine:** The vectorized execution engine is inspired by the paper [MonetDB/X100: Hyper-Pipelining Query Execution](http://cidrdb.org/cidr2005/papers/P19.pdf) by Peter Boncz, Marcin Zukowski and Niels Nes.
-   **Optimizer:** DuckDB's optimizer draws inspiration from the papers [Dynamic programming strikes back](https://15721.courses.cs.cmu.edu/spring2020/papers/20-optimizer2/p539-moerkotte.pdf) by Guido Moerkotte and Thomas Neumann as well as [Unnesting Arbitrary Queries](http://www.btw-2015.de/res/proceedings/Hauptband/Wiss/Neumann-Unnesting_Arbitrary_Querie.pdf) by Thomas Neumann and Alfons Kemper.
-   **Concurrency control:** Our MVCC implementation is inspired by the paper [Fast Serializable Multi-Version Concurrency Control for Main-Memory Database Systems](https://db.in.tum.de/~muehlbau/papers/mvcc.pdf) by Thomas Neumann, Tobias Mühlbauer and Alfons Kemper.
-   **Secondary indexes:** DuckDB has support for secondary indexes based on the paper [The Adaptive Radix Tree: ARTful Indexing for Main-Memory Databases](https://db.in.tum.de/~leis/papers/ART.pdf) by Viktor Leis, Alfons Kemper and Thomas Neumann.
-   **SQL window functions:** DuckDB's window functions implementation uses Segment Tree Aggregation as described in the paper [Efficient Processing of Window Functions in Analytical SQL Queries](https://www.vldb.org/pvldb/vol8/p1058-leis.pdf) by Viktor Leis, Kan Kundhikanjana, Alfons Kemper and Thomas Neumann.
-   **SQL inequality joins:** DuckDB's inequality join implementation uses the IEJoin algorithm as described in the paper [Lightning Fast and Space Efficient Inequality Joins](https://vldb.org/pvldb/vol8/p2074-khayyat.pdf) Zuhair Khayyat, William Lucia, Meghna Singh, Mourad Ouzzani, Paolo Papotti, Jorge-Arnulfo Quiané-Ruiz, Nan Tang and Panos Kalnis.
-   **Compression of floating-point values:** DuckDB supports the multiple algorithms for compressing floating-point values:
    -   [Chimp](https://vldb.org/pvldb/vol15/p3058-liakos.pdf) by Panagiotis Liakos, Katia Papakonstantinopoulou and Yannis Kotidi
    -   [Patas](https://github.com/duckdb/duckdb/pull/5044), an in-house development, and
    -   [ALP (adaptive lossless floating-point compression)](https://dl.acm.org/doi/pdf/10.1145/3626717) by Azim Afroozeh, Leonard Kuffo and Peter Boncz, who also [contributed their implementation](https://github.com/duckdb/duckdb/pull/9635).
-   **SQL Parser:** We use the PostgreSQL parser that was [repackaged as a stand-alone library](https://github.com/lfittl/libpg_query). The translation to our own parse tree is inspired by [Peloton](https://pelotondb.io/).
-   **Shell:** We use the [SQLite shell](https://sqlite.org/cli.html) to work with DuckDB.
-   **Regular expressions:** DuckDB uses Google's [RE2](https://github.com/google/re2) regular expression engine.
-   **String formatting:** DuckDB uses the [fmt](https://github.com/fmtlib/fmt) string formatting library.
-   **UTF wrangling:** DuckDB uses the [utf8proc](https://juliastrings.github.io/utf8proc/) library to check and normalize UTF8.
-   **Collation and time:** DuckDB uses the [ICU](https://unicode-org.github.io/icu/) library for collation, time zone, and calendar support.
-   **Test framework:** DuckDB uses the [Catch2](https://github.com/catchorg/Catch2) unit test framework.
-   **Test cases:** We use the [SQL Logic Tests from SQLite](https://www.sqlite.org/sqllogictest/doc/trunk/about.wiki) to test DuckDB.
-   **Result validation:** [Manuel Rigger](https://www.manuelrigger.at/) used his excellent [SQLancer](https://github.com/sqlancer/sqlancer) tool to verify DuckDB result correctness.
-   **Query fuzzing:** We use [SQLsmith](https://github.com/anse1/sqlsmith) to generate random queries for additional testing.
-   **JSON parser:** We use [yyjson](https://github.com/ibireme/yyjson), a high performance JSON library written in ANSI C, to parse JSON in DuckDB's [JSON Extension](https://duckdb.org/docs/data/json/overview.html).
