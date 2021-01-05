---
layout: default
title: Designing Fast Storage
nav_order: 1
parent: Lectures & Materials
permalink: /lectures/storage-recipe/
---

# Why our Storage is so Fast?

## Summary

A modern DBMS can be viewed as a composition of following layers:

1. A key-value binary storage layer such as [RocksDB](https://rocksdb.org) or [WiredTiger](https://docs.mongodb.com/manual/core/wiredtiger/).
2. A single-instance logic layer such as [Postgres](https://www.postgresql.org).
3. A distributed load-balancer and replication manager like [GreenPlum](https://greenplum.org).

Depending on your performance goals you may have to optimize one or more layers in this equation. We replaced all of them with custom solutions. Assuming a DBMS implementation size can reach millions of lines of code, there are many design decisions to make. Let's group them by affected computer component.

---

- [Why our Storage is so Fast?](#why-our-storage-is-so-fast)
  - [Summary](#summary)
  - [SSD](#ssd)
    - [Data Layout](#data-layout)
    - [Compression](#compression)
  - [CPU](#cpu)
    - [Search Algorithms](#search-algorithms)
    - [Optimized Implementations](#optimized-implementations)
    - [Parallelism, Concurrency and Serialization](#parallelismconcurrency-and-serialization)
  - [RAM](#ram)
    - [Memory Management](#memory-management)
    - [Memory Accesses](#memory-accesses)
  - [WEB](#web)
    - [Communication Protocols](#communication-protocols)

---

## SSD

### Data Layout

|   Common Ingridients    |      Our Ingridients       |
| :---------------------: | :------------------------: |
| Row-wise or Column-wise | Hybrid and Domain-specific |

Imagine having a table with 1'000'000 rows and 3 columns. Every row represents your companies customers with columns: `first-name`, `last-name`, `age`. If you often delete or add customers it makes sense to store them in a row-major format. This means, that when you are deleting the customer #200'000, you won't have to shift the following 800'000 entries one row up. Row-major format means that the DB is optimized for row-level operations and can store different rows in different buckets on disk. The benefit is that the updates are fast.

The downside is that the lookups aren't very fast and aggregations are painfully slow. If you try computing the mean `age` of your customers - you would have to jump from one bucket to another only to extract a single number from every entry. This is where columnar databases shine. Some of them are immutable or append-only, but they still have a huge value, as they accelerate the increasingly important analytical queries.

At Unum, we use a hybrid approach to guarantee good performance across a wide variety of workloads. Our design is perfectly tuned to minimize the number of random jumps on SSD (can be as slow as 100 MB/s) and maximize the number of sequential operations (as fast as 6 GB/s). It means you can use UnumDB both as your primary database and analytical database at the same time.

Why it's important? If your data scientists have to sample & export the data every time they run experiments, your pipelines become bloated and slow. What's worse, the experiment results may be irrelevant as the data becomes outdated before experiments finish.

### Compression

| Common Ingridients | Our Ingridients |
| :----------------: | :-------------: |
|    Zlib, Snappy    |     Custom      |

If the SSDs are so slow compared to the rest of your server - the next logical step is to minimize the data that goes to disk. Designing a good compression algorithm is true science. Most of the research in this industry was done in 1980s and boils down to a few key topics: Shannons entropy and Huffman coding. Even today industry standard libraries such as [Snappy](http://google.github.io/snappy/) and [Zlib](https://zlib.net) use the same old ideas.

Luckily for our customers, we actively design and benchmark new specialized compression algorithms for different parts of our DB. Remember, we have a custom [Data Layout](#data-layout). This means we have a deeper understanding of how the bytes will be arranged on disk. ⇒ We can replace a general-purpose compression engine with a tailored algorithm for our system!

## CPU

### Search Algorithms

| Common Ingridients | Our Ingridients |
| :----------------: | :-------------: |
|   BNDM, DFA, LSH   |     Custom      |

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

|         Common Ingridients          |   Our Ingridients    |
| :---------------------------------: | :------------------: |
| High-level General-Purpose Language | SIMD Assembly, GPGPU |

Computer Science is great, but programming is very much an applied field. A new algorithm should not only have a better asymptotic complexity, but also a low constant overhead. This where our systems truly shine. The math and engineering come together in our products..

When we identify a hot data-flow path, we start optimizing. We can either process more bytes per-cycle on the CPU or send the data to a specialized accelerator card like GPU. If the data-points are small and low latency is important - we use `AVX2` and `AVX-512` SIMD instructions on x86 and `NEON` SIMd instructions ARM.

If the data batches are in 100 MB - 10 GB range, we often switch to GPUs and implement our kernels with CUDA, OpenCL, Halide, SyCL and all kinds of other heterogeneus computing technologies.

[Read About Our Software Stack](/jobs/#current-technology-stack){: .btn .fs-5 .mb-4 .mb-md-0 }

### Parallelism, Concurrency and Serialization

For the sake of completeness there are a few other technical things we should mention:

1. Multi-processing and multi-threading is not the same thing.
2. Not every concurrent data-structure is lock-free.
3. System calls are expensive and synchronous I/O is avoided at Unum.
4. We avoid JSON, XML and similar formats for internal use in favor of binary formats.
5. SQL comes with a hefty parsing overhead and is replaced with higher-level language bindings.

## RAM

### Memory Management

|       Common Ingridients       |       Our Ingridients        |
| :----------------------------: | :--------------------------: |
| Multi-copy, Garbage Collection | Bypass OS cache and 0 copies |

RAM chips are considered fast, but they are also very expensive. RAM is ~50x more expensive than NAND Flash (per byte). A good database must manage that tiny RAM pool very efficiently, not leaving any irrelevant data copies behind. Unfortunately, that's not always possible. Depending on the I/O protocols you use - hidden copies of your data can be stored in the OS cache pages, let alone the caches of the database itself.

Another problem, is that some databases (written in Java or other high-level languages) run in a Garbage-Collecting environment. This means that the developers of such databases rely on a slow and faulty procedure that automatically reclaims memory. It sometimes makes software development easier, but also comes with undeniable drawbacks and inefficiencies.

### Memory Accesses

|   Common Ingridients   |      Our Ingridients       |
| :--------------------: | :------------------------: |
| No Direct Optimization | Rare Page-Aligned Accesses |

Every modern computer has separate slots for the CPU and RAM on the motherboard. Every bit must travel through the PCB from RAM to CPU and vice-versa. Those round-trips are orders of magnitude cheaper, than reading from SSD or HDD, but if you have optimized everything else - this is the next place to look.

|              Operation               | Energy Costs <sup>1</sup> | CPU Cycles <sup>2</sup> |
| :----------------------------------: | :-----------------------: | :---------------------: |
|           Integer Addition           |           1 pJ            |            1            |
|            Load from SRAM            |           3 pJ            |           10            |
|          Move 10 mm on-chip          |           30 pJ           |                         |
|            Send off-chip             |          500 pJ           |                         |
|             Send to DRAM             |           1 nJ            |           200           |
| Read from NAND Flash SSD<sup>3</sup> |           2 μJ            |                         |

Means, simply accessing an integer from DRAM can be 1'000x more expensive, than operating on it. HPC software is optimized to reduce the number of memory accesses. A good example of that are the [fast matrix multiplcation algorithms](https://en.wikipedia.org/wiki/Matrix_multiplication_algorithm) which achieve sub-cubic complexity through such optimizations. We perform similar micro-optimizations throughout our system.

> <sup>1</sup> The numbers vary greatly depending on the litographic process generation, but the ratios generally hold.
> 
> <sup>2</sup> [Read](https://stackoverflow.com/questions/4087280/approximate-cost-to-access-various-caches-and-main-memory) "Approximate cost to access various caches and main memory".
> 
> <sup>3</sup> SSDs don't address single bytes. Generally the 2-4 KB pages are grouped into 128 KB blocks.

## WEB

### Communication Protocols

| Common Ingridients |    Our Ingridients    |
| :----------------: | :-------------------: |
|       TCP/IP       | UDP, Infiniband, RDMA |

As soon as you database grows beyond a single server instance, you need those servers to communicate with each other and communication is hard. Not only the synchronization logic must be bullet-proof to guarantee the same ACID transactions in a distributed environment, but also the implementation must be efficient. The growth of AI sector has accelerated the R&D in high-bandwidth networking and you can often find clusters ready for 100/200/400 or even 800 Gb/s networking.

The sad reality is that still most of the distributed systems still rely on the TCP/IP stack for communication. It doesn't support such speeds, it doesn't support global memory addressing and introduces a huge bottleneck when encoding/decoding packets. So if you want to reach our numbers, say goodbye to TCP/IP and hello to Infiniband RDMA!

---

The importance of each optimization above is extremely hard to quantify in a general case, because there is no "general case". Every part must be maticuluously benchmarked in conjuction with underlying hardware to achieve ideal performance! Happy benchmarking!
