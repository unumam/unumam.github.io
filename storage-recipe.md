---
layout: default
title: Performance Recipe
nav_order: 5
parent: Storage Solutions
permalink: /storage/recipe/
---

# Why our Storage is so Fast?

A database is complicated result of a long engineering process, but it can be seen as following:

1. A key-value binary storage layer such as [RocksDB](https://rocksdb.org) or [WiredTiger](https://docs.mongodb.com/manual/core/wiredtiger/).
2. A single-instance logic layer such as [Postgres](https://www.postgresql.org).
3. A distributed load-balancer and replication manager like [GreenPlum](https://greenplum.org).

Depending on your performance goals you may have to optimize one or more layers in this equation. Luckily, some databases are modular and you can mix and match the open-source parts you like. Unfortunately though, even a single component in this equation can have millions of lines of legacy C/C++ code that was growing like a snowball for past 30 years. Our budgets are tights and our ambitions are limitless so we ended up rebuilding the entire stack step-by-step.

If you were to repeat our journey, there is a number of things to pay attention to.

Let's group them by primary device affected.

## SSD

### Data Layout

|   Common Ingridients    |    Optimal Ingridients     |
| :---------------------: | :------------------------: |
| Row-wise or Column-wise | Hybrid and Domain-specific |

Imagine having a table with 1'000'000 rows and 3 columns. Every row represents your companies customers with columns: `first-name`, `last-name`, `age`. If you often delete or add customers it makes sense to store them in a row-major format. This means, that when you are deleting the customer #200'000, you won't have to shift the following 800'000 entries one row up. Row-major format means that the DB is optimized for row-level operations and can store different rows in different buckets on disk. The benefit is that the updates are fast.

The downside is that the lookups aren't very fast and aggregations are painfully slow. Imagine having to compute the mean `age` of your customers. You would have to jump from one bucket to another only to extract a single number from it. This is where columnar databases shine. They may even be immutable or append-only, but they still have a huge value, as they accelerate the increasingly important analytical queries.

At Unum, we use a hybrid approach to guarantee good performance across a wide variety of workloads. Our design is perfectly tuned to minimize the number of random jumps on SSD (can be as slow as 100 MB/s) and maximize the number of sequential operations (as fast as 6 GB/s). It means you can use UnumDB both as your primary database and analytical database at the same time.

Why it's important? If your data scientists have to sample & export the data every time they run experiments, your pipelines become bloated and slow. What's worse, the experiment results may be irrelevant as the data becomes outdated by the time experiments have finished.

### Compression

| Common Ingridients | Optimal Ingridients |
| :----------------: | :-----------------: |
|    Zlib, Snappy    |       Custom        |

If the SSDs are so slow compared to the rest of your server the next logical step is to minimize the volume of data that goes to disk. Designing a good compression algorithm is true science. Most of the research in this industry was done in 1980s and boils down to a few key topics: Shannons entropy and Huffman coding. Even today industry standard libraries such as [Snappy](http://google.github.io/snappy/) and [Zlib](https://zlib.net) use the same old ideas.

Luckily for our customers, we actively design and benchmark new specialized compression algorithms for different parts of our DB. Remember, we have a custom Data Layout. This means we have a deeper understanding of how the bytes will look like on disk. So we can have a better faster compression algorithm that is tailored specifically for every part of our system!

## CPU

### Search Algorithms

| Common Ingridients | Optimal Ingridients |
| :----------------: | :-----------------: |
|   BNDM, DFA, LSH   |       Custom        |

This is where things get repetitive. Let's take a primitive task of searching for a substring in a bigger string (such as count the inclusions of "the" in "the theme"). How hard can it be? Computer Scientists know the answer. If `N` is the length of the needle and `H` is the length of the haystack, then:

- Brute Force algorithm takes up to `~O(N*H)` steps,
- [Rabin–Karp](https://en.wikipedia.org/wiki/Rabin–Karp_algorithm) algorithm takes on average `~O(N+H)` steps,
- [Knuth-Morris-Pratt](https://en.wikipedia.org/wiki/Knuth–Morris–Pratt_algorithm) and [BNDM](https://www-igm.univ-mlv.fr/~lecroq/string/bndm.html) algorithms takes up to `~O(H)` steps, and so on.

This is what the textbook says, but if you take a few years off to run a few thousand benchmarks chances are you can find better approaches. Furthermore, search is not just about finding exact string matches. Here is what else we have:

- fuzzy search for partial matches,
- custom RegEx search for complex textual patterns,
- custom k-Nearest Neighbors search for high-dimensional vector representations,
- on the fly indexing of scalar fields.

### Optimized Implementations

|         Common Ingridients          | Optimal Ingridients  |
| :---------------------------------: | :------------------: |
| High-level General-Purpose Language | SIMD Assembly, GPGPU |

Computer Science is great, but programming is very much an applied field. A new algorithm should not only have a better asymptotic complexity, but also a low constant overhead. This where our systems truly shine. The math and engineering come together in our products. This is can be characterized by just 2 professional terms:

- SIMD Instructions
- GPGPU

which stand for "Single Instruction Multiple Data assembly Instructions" and "General Purporse programming on Graphics Processing Units". We have a long track record of utilizing both of those technologies and were invited to talk on those topics in professional communities all over the world:

- USA
- Russia
- Germany
- Armenia

### Parallelism, Concurrency and Serialization

For the sake of completeness there are a few other technical things we should mention:

1. Multi-processing and multi-threading is not the same thing.
2. Not every concurrent data-structure is lock-free.
3. System calls are expensive and synchronous I/O is avoided at Unum.
4. We avoid JSON, XML and similar formats for internal use in favor of binary formats.
5. SQL comes with a hefty parsing overhead and is replaced with higher-level language bindings.

## RAM

### Memory Management

|       Common Ingridients       |     Optimal Ingridients      |
| :----------------------------: | :--------------------------: |
| Multi-copy, Garbage Collection | Bypass OS cache and 0 copies |

Random Access Memory is considered fast <sup>1</sup>, but it's also very expensive. In 2020, a server can have over 10 TB of RAM, which costs well over 100'000$. That's a lot compared to even high end SSDs. A similar volume costs about 50x cheaper at about 2'000$. A good database must manage that tiny RAM pool very efficiently, not leaving copies around. Unfortunately, that's not always possible. Depending on the I/O protocols you use - hidden copies of your data can be stored in the OS cache pages, let alone the caches of the database itself.

Another problem, is that some databases written in Java or other high-level languages run in a Garbage-Collecting environment. This means that the developers of such databases rely on a slow and faulty procedure that automatically reclaims memory. It may simplify the development for them, but also comes with undeniable drawbacks and inefficiencies.

## WEB

### Communication Protocols

| Common Ingridients |  Optimal Ingridients  |
| :----------------: | :-------------------: |
|       TCP/IP       | UDP, Infiniband, RDMA |

As soon as you database grows beyond a single server instance, you need those servers to communicate with each other and communication is hard. Not only the synchronization logic must be bullet-proof to guarantee the same ACID transactions in a distributed environment, but also the implementation must be efficient. The growth of AI sector has accelerated the R&D in high-bandwidth networking and you can often find clusters ready for 100/200/400 or even 800 Gb/s networking. 

The sad reality is that still most of the distributed systems still rely on the TCP/IP stack for communication. It doesn't support such speeds, it doesn't support global memory addressing and introduces a huge bottleneck when encoding/decoding packets. So if you want to reach our numbers, say goodbye to TCP/IP and hello to Infiniband RDMA!

---

> <sup>1</sup> Everything is relative. DRAM is fast compared to NAND Flash, but extremely slow compared to SRAM on your CPU. It takes thousands of CPU cycles to retreive a single 32-bit integer from DRAM, but only one CPU cycle to perform an addition or subtraction.
