Concurrency is one of Go's greatest strengths, but it can also be a source of many problems if you're not careful or don't fully understand what you're doing.

Is writing concurrent code difficult? I'd say it's one of the challenging parts in programming because so many things can go wrong. Go makes it easier with Goroutines compared to other languages, but that doesn't mean it's foolproof.

One way to avoid going the wrong way with concurrency is to use tha patterns that has been already tested overtime instead of trying to invent your own.

In this blog we will learn how you can utilize confinement pattern to improve your Go concurrency performance.

### [](https://dev.to/bahaanoah/golang-concurrency-how-confinement-improves-performance-without-locks-1ld5?context=digest#whats-confinement)[What's confinement?](https://dev.to/bahaanoah/golang-concurrency-how-confinement-improves-performance-without-locks-1ld5?context=digest#what-is-confinement)

Confinement is a simple yet powerful pattern that ensures data is only accessible from a single concurrent process. When done correctly, it makes a concurrent program completely safe, eliminating the need for synchronization.

In Go, the confinement pattern keeps data access and modifications restricted to a single Goroutine. This approach helps avoid race conditions and ensures safe operations without relying on synchronization tools like mutexes.

Imagine a writer keeping notes in a private journal. Since only they write and read from it, there's no risk of conflicting edits or needing coordination with others.

Similarly, in Go, if a single goroutine owns and modifies a piece of data, there's no need for synchronization mechanisms like mutexes, since no other goroutine can interfere with it.

### [](https://dev.to/bahaanoah/golang-concurrency-how-confinement-improves-performance-without-locks-1ld5?context=digest#why-use-confinement)[Why Use Confinement?](https://dev.to/bahaanoah/golang-concurrency-how-confinement-improves-performance-without-locks-1ld5?context=digest#why-use-confinement)

-   Avoid race conditions without using mutexes.
-   Improve performance by eliminating locking overhead.
-   Simplify code by keeping state management within a single goroutine.

**That being said nothing explains it better than some code examples.**

### [](https://dev.to/bahaanoah/golang-concurrency-how-confinement-improves-performance-without-locks-1ld5?context=digest#code-examples)[Code Examples](https://dev.to/bahaanoah/golang-concurrency-how-confinement-improves-performance-without-locks-1ld5?context=digest#code-examples)

#### [](https://dev.to/bahaanoah/golang-concurrency-how-confinement-improves-performance-without-locks-1ld5?context=digest#example-1-race-condition-and-no-confinement)[Example 1: Race condition and no confinement](https://dev.to/bahaanoah/golang-concurrency-how-confinement-improves-performance-without-locks-1ld5?context=digest#example-1)

The code simulates processing multiple orders concurrently by spawning a goroutine for each order, which appends the processed result to a shared slice. However, since all goroutines access and modify the slice simultaneously without synchronization, a race condition occurs, leading to unpredictable results.  

```
package main

import (
    "fmt"
    "strings"
    "sync"
)

func processOrder(order string) string {
    return fmt.Sprintf("Processed %s", order)
}

func addOrder(order string, result *[]string, wg *sync.WaitGroup) {
    processedOrder := processOrder(order)
    *result = append(*result, processedOrder) // Shared state modified by multiple goroutines (critical section)
    wg.Done()
}

func main() {
    var wg sync.WaitGroup

    orders := []string{"Burger", "Pizza", "Pasta"}
    processedOrders := make([]string, 0, len(orders))

    for _, order := range orders {
        wg.Add(1)

        go addOrder(order, &amp;processedOrders, &amp;wg)
    }

    wg.Wait()

    fmt.Println("Processed Orders:", strings.Join(processedOrders, ", "))
}

```

##### [](https://dev.to/bahaanoah/golang-concurrency-how-confinement-improves-performance-without-locks-1ld5?context=digest#issues)🔴 Issues:

-   Unpredictable orders value due to race conditions.
-   Different results on each run.

When trying to run the example above with `--race` flag, we can see the data race in the output.

[![Race condition output](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fres.cloudinary.com%2Fdt5qg3orv%2Fimage%2Fupload%2Ff_auto%2Cq_auto%2Fjxxgpkocvkn7jwlj09p6)](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fres.cloudinary.com%2Fdt5qg3orv%2Fimage%2Fupload%2Ff_auto%2Cq_auto%2Fjxxgpkocvkn7jwlj09p6)

___

#### [](https://dev.to/bahaanoah/golang-concurrency-how-confinement-improves-performance-without-locks-1ld5?context=digest#example-2-using-mutex)[Example 2: Using Mutex](https://dev.to/bahaanoah/golang-concurrency-how-confinement-improves-performance-without-locks-1ld5?context=digest#example-2)

To solve the problem above, one way is to guard the critical section with a [**Mutex lock**](https://pkg.go.dev/sync) (mutual exclusion lock). Mutex is a lock that we set before using a shared resource and release after using it. but it comes with a cost, it can slow down the program because it makes the program sequential.  

```
func processOrder(order string) string {
    return fmt.Sprintf("Processed %s", order)
}

func addOrder(order string, result *[]string, wg *sync.WaitGroup, lock *sync.Mutex) {
    lock.Lock()
    processedOrder := processOrder(order)
    *result = append(*result, processedOrder) // Shared state modified by multiple goroutines (critical section)
    lock.Unlock()
    wg.Done()
}

func main() {
    var wg sync.WaitGroup
    var lock = &amp;sync.Mutex{}

    orders := []string{"Burger", "Pizza", "Pasta"}
    processedOrders := make([]string, 0, len(orders))

    for _, order := range orders {
        wg.Add(1)

        go addOrder(order, &amp;processedOrders, &amp;wg, lock)
    }

    wg.Wait()

    fmt.Println("Processed Orders:", strings.Join(processedOrders, ", "))

    // Output &gt; Processed Orders: Processed Pasta, Processed Burger, Processed Pizza
}

```

Let's also simulate some processing time in the `processOrder` func and measure the processing time to see the impact of locking.  

```
func processOrder(order string) string {
    time.Sleep(2 * time.Second)
    return fmt.Sprintf("Processed %s", order)
}

func addOrder(order string, result *[]string, wg *sync.WaitGroup, lock *sync.Mutex) {
    lock.Lock()
    processedOrder := processOrder(order)
    *result = append(*result, processedOrder) // Shared state modified by multiple goroutines (critical section)
    lock.Unlock()
    wg.Done()
}

func main() {
    start := time.Now()

    var wg sync.WaitGroup
    var lock = &amp;sync.Mutex{}

    orders := []string{"Burger", "Pizza", "Pasta"}
    processedOrders := make([]string, 0, len(orders))

    for _, order := range orders {
        wg.Add(1)

        go addOrder(order, &amp;processedOrders, &amp;wg, lock)
    }

    wg.Wait()

    fmt.Println("Processed Orders:", strings.Join(processedOrders, ", "))
    fmt.Println("Processing Time:", time.Since(start))

    // Output &gt; Processed Orders: Processed Burger, Processed Pizza, Processed Pasta
    // &gt; Processing Time: 6.00230125s
}

```

##### [](https://dev.to/bahaanoah/golang-concurrency-how-confinement-improves-performance-without-locks-1ld5?context=digest#improvements)🟡 Improvements:

-   ✅ No race conditions (mutex ensures only one goroutine modifies counter at a time).
-   ❌ Performance overhead due to frequent locking/unlocking.

As you can see in the output above the processing time is 2 seconds per order because it's processing them sequentially.

> We can improve the code above and keep the lock by moving the locking to be only on the critical section only, but will rather use confinement.

___

#### [](https://dev.to/bahaanoah/golang-concurrency-how-confinement-improves-performance-without-locks-1ld5?context=digest#example-3-using-confinement)[Example 3: Using Confinement](https://dev.to/bahaanoah/golang-concurrency-how-confinement-improves-performance-without-locks-1ld5?context=digest#example-3)

In this example with using confinement to solve the race condition, avoiding the need for Mutex locks.  

```
func processOrder(order string) string {
    time.Sleep(2 * time.Second)
    return fmt.Sprintf("Processed %s", order)
}

func addOrder(order string, resultDest *string, wg *sync.WaitGroup) {
    processedOrder := processOrder(order)
    *resultDest = processedOrder // Shared state modified by multiple goroutines (critical section)
    wg.Done()
}

func main() {
    start := time.Now()

    var wg sync.WaitGroup

    orders := []string{"Burger", "Pizza", "Pasta"}
    processedOrders := make([]string, len(orders))

    for idx, order := range orders {
        wg.Add(1)

        go addOrder(order, &amp;processedOrders[idx], &amp;wg)
    }

    wg.Wait()

    fmt.Println("Processed Orders:", strings.Join(processedOrders, ", "))
    fmt.Println("Processing Time:", time.Since(start))

    // Output &gt; Processed Orders: Processed Burger, Processed Pizza, Processed Pasta
    // &gt; Processing Time: 2.002053625s
}

```

##### [](https://dev.to/bahaanoah/golang-concurrency-how-confinement-improves-performance-without-locks-1ld5?context=digest#advantages)🟢 Advantages:

-   ✅ No race conditions (single goroutine modifies orders).
-   ✅ No mutex locking overhead.
-   ✅ More efficient in scenarios with heavy contention.

##### [](https://dev.to/bahaanoah/golang-concurrency-how-confinement-improves-performance-without-locks-1ld5?context=digest#how-confinement-works-in-this-example)💡 How confinement works in this example

-   By passing the result directly instead of modifying a shared resource (like a slice), each goroutine is given its own dedicated element in the `processedOrders` slice.
-   Each goroutine is responsible for modifying only its own element in the slice `(processedOrders[idx])`, which prevents contention over shared memory.
-   Each `addOrder` goroutine updates its own location in the slice rather than appending to a shared slice.

🔹 **Another approach to achieve confinement with the same example can be done with channels**  

```
func processOrder(order string) string {
    time.Sleep(2 * time.Second)
    return fmt.Sprintf("Processed %s", order)
}

func addOrder(order string, resultChan chan&lt;- string, wg *sync.WaitGroup) {
    defer wg.Done()
    processedOrder := processOrder(order)
    resultChan &lt;- processedOrder
}

func main() {
    start := time.Now()

    orders := []string{"Burger", "Pizza", "Pasta"}
    resultChan := make(chan string, len(orders))

    var wg sync.WaitGroup

    for _, order := range orders {
        wg.Add(1)
        go addOrder(order, resultChan, &amp;wg)
    }

    go func() {
        wg.Wait()
        close(resultChan)
    }()

    var processedOrders []string
    for order := range resultChan {
        processedOrders = append(processedOrders, order)
    }

    fmt.Println("Processed Orders:", strings.Join(processedOrders, ", "))
    fmt.Println("Processing Time:", time.Since(start))

    // Output:
    // Processed Orders: Processed Burger, Processed Pizza, Processed Pasta
    // Processing Time: 2.002053625s
}

```

##### [](https://dev.to/bahaanoah/golang-concurrency-how-confinement-improves-performance-without-locks-1ld5?context=digest#how-confinement-works-in-this-example)💡 How confinement works in this example

-   Each goroutine sends its result to a dedicated channel (`resultChan`).
-   The channel safely handles communication between goroutines without needing locks.
-   The main goroutine collects results from the channel, ensuring thread-safe data collection.
-   A buffered channel is used to prevent blocking when sending results.
-   Channel communication enforces data transfer through a single point, naturally preventing race conditions.

> Unlike the mutex-based solution, both of these approaches keeps the program concurrent because no goroutines need to wait for others to release a lock as we can see the difference in the processing time.

### [](https://dev.to/bahaanoah/golang-concurrency-how-confinement-improves-performance-without-locks-1ld5?context=digest#conclusion)[Conclusion](https://dev.to/bahaanoah/golang-concurrency-how-confinement-improves-performance-without-locks-1ld5?context=digest#conclusion)

The Go confinement pattern is great when you need safe, sequential access to a shared resource without locks. However, if multiple goroutines require parallel access, other synchronization methods (mutexes, atomics) may be better.

-   ✅ Use confinement when one goroutine can own the data (best for queues, worker pools).
-   ✅ Use mutex when multiple goroutines need simultaneous access (best for shared maps, counters).
-   ❌ Don't use confinement When multiple goroutines must access and modify the same data.
-   ❌ Avoid shared state without synchronization (leads to race conditions).

#### [](https://dev.to/bahaanoah/golang-concurrency-how-confinement-improves-performance-without-locks-1ld5?context=digest#resources)Resources

-   [Book - Chapter 4. Concurrency Patterns in Go](https://learning.oreilly.com/library/view/concurrency-in-go/9781491941294/ch04.html#confinement)
-   [Video - Improve Go Concurrency Performance With This Pattern](https://www.youtube.com/watch?v=Bk1c30avsuU&t=588s)