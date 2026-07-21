# Week 8 - Performance, Profiling & Debugging

## Task 1 - Profile and fix - Pending

---

## Task 2 - Find an N+1

For this task, I searched for multiple open-source Django projects for N+1 queries. Since not every project necessarily contains one, I investigated several different projects instead of stopping after the first attempt.

**Django Oscar**

I set up the project locally, enabled SQL query logging, and inspected several pages, including the storefront catalog and multiple dashboard pages (Products, Orders, Customers, and Reviews). I also created sample data where necessary to generate realistic SQL queries.

I found that most database relationships already used joins or batched queries, and I could not identify any N+1 queries.

**Healthchecks**

I explored the project structure and inspected several pages that could potentially trigger N+1 queries. However, I did not find any clear case that demonstrated the issue.

**django-cart**

Before writing any additional tests, I reviewed the project's existing test suite. I found dedicated performance and query-count tests that verify constant query counts during iteration and product loading. These regression tests already help prevent common N+1 scenarios, so I did not identify a new case worth reporting.

### Findings

Although I could not find an N+1 query suitable for opening an issue or submitting a fix, I gained valuable experience in analyzing SQL queries, reviewing query behavior, and evaluating how different open-source projects optimize database access.

To be continued.

---

## Task 3 - Read 3 real flame graphs

For this task, I selected the following flame graphs:

### [1. MySQL CPU Flame Graph](https://www.brendangregg.com/FlameGraphs/cpuflamegraphs.html)

**What's hot:**
`JOIN::exec` was the hottest function because it occupied the widest area in the flame graph.

**What's the bottleneck:**
Most of the CPU time was spent executing MySQL joins, making the query execution path the main bottleneck.

**What I learned:**
The flame graph made the real hotspot obvious, while the text-based profile was much harder to interpret.

### [2. Java Flame Graph](https://www.brendangregg.com/blog/2014-06-12/java-flame-graphs.html)

**What's hot:**
The Rhino JavaScript execution path was the hottest part of the profile because it consumed a large share of the CPU samples.

**What's the bottleneck:**
The Rhino execution was the main bottleneck. Other functions, such as writing data back to clients, were also hot but represented normal server work rather than a performance issue.

**What I learned:**
Flame graphs make it much easier to identify CPU-intensive Java code without getting lost in lengthy profiling outputs.

### [3. DOOM GPU Flame Graph](https://www.brendangregg.com/blog/2025-05-01/doom-gpu-flame-graphs.html)

**What's hot:**
Shader compilation and NIR preprocessing were the hottest CPU-side code paths because they consumed most of the sampled CPU time.

**What's the bottleneck:**
The CPU spent a significant amount of time compiling and preparing shaders before the GPU could execute them, making CPU-side shader preparation the main bottleneck.

**What I learned:**
Even in GPU-intensive applications, a flame graph can reveal CPU bottlenecks that delay GPU execution and affect overall performance.

---

## Task 4 - Use a real debugger - Pending
