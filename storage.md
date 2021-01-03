---
layout: default
title: Storage Solutions
nav_order: 2
has_children: true
permalink: /storage/
---

# Storage Solutions

Unum comes with a database engine, with astonishing performance. Let's see why it's important to have fast data storage.

## Energy Costs

Contrary to popular opinion, performing arithmetical operations in a computer is orders of magnitude cheaper, that fetching the data for those computations.

|              Operation               | Energy Costs <sup>1</sup> |
| :----------------------------------: | :-----------------------: |
|           Integer Addition           |         **1 pJ**          |
|            Load from SRAM            |           3 pJ            |
|          Move 10 mm on-chip          |           30 pJ           |
|            Send off-chip             |          500 pJ           |
|             Send to DRAM             |           1 nJ            |
| Read from NAND Flash SSD<sup>2</sup> |   **2 μJ** <sup>3</sup>   |
|            Send over LTE             |           10 μJ           |

Means, simply accessing an integer from DRAM is 1'000x more expensive, than operating on it.
Furthermore, DRAM is very expensive and volatile (energy-dependant).
So if your business needs to process a lot of data — you can't store it in RAM and will have to fetch it from a storage devices.

## Time Costs

Energy consumption aside, it takes a different amount of time to exchange data between different parts of the computer.

|                                    Channel                                    |  Bandwidth  |
| :---------------------------------------------------------------------------: | :---------: |
|          CPU ⇌ [DDR4 RAM](https://en.wikipedia.org/wiki/DDR4_SDRAM)           |  ~100 GB/s  |
|    GPU ⇌ [HBM2 VRAM](https://en.wikipedia.org/wiki/High_Bandwidth_Memory)     | ~1,000 GB/s |
|         GPU ⇌ GPU [via NVLink](https://en.wikipedia.org/wiki/NVLink)          |  ~300 GB/s  |
|   CPU ⇌ GPU [via PCI-E Gen3 x16](https://en.wikipedia.org/wiki/PCI_Express)   |  ~15 GB/s   |
| CPU ⇌ SSD [via NVMe PCI-E Gen3 x4](https://en.wikipedia.org/wiki/NVM_Express) |  << 4 GB/s  |

CPU ⇌ SSD communication channel is by far the slowest link in the chain.
The theoretical throughput of that channel is limited to 4 GB/s, but in the real world most reads happen at 200 MB/s.
This means, that even if you buy the most expensive CPUs and GPUs, they may spend 90% idling - simply waiting for the data to come.
Modern Machine Learning approaches almost universally improve when trained on bigger datasets.
So if you want to have competitve analytics, you need a fast database.

## What are the options?

The databases that we compare across a broad range of workloads include:

|                         |          Unum          | MongoDB | Postgres | Neo4J | ElasticSearch |
| :---------------------: | :--------------------: | :-----: | :------: | :---: | :-----------: |
|         Purpose         | Nets, Text, Vecs, Docs |  Docs   |  Tables  | Nets  |     Text      |
| Implementation Language |          C++           |   C++   |    C     | Java  |     Java      |
|      Lines of Code      |         100 K          | 3900 K  |  1300 K  | 800 K |     300 K     |

How big can be the difference?

* 50x faster dataset imports.
* 5x smaller compressed representations.
* 100x faster random lookups.
* 10x faster batched insertions.

Again, if you want to have competitve analytics, you need a fast database.<br/>
If you want to build the best AI in your industry - you need Unum.

[Check our Benchmarks](/storage/graphs){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 } [Our Performance Ingridients](/storage/recipe){: .btn .fs-5 .mb-4 .mb-md-0 }

---

1. Following are approximate numbers accurate only in relative to each other terms. 
2. SSDs don't address single bytes. Generally the 2-4 KB pages are grouped into 128 KB blocks.
3. According to "A comprehensive study of energy efficiency and performance of flash-based SSD". [Paper](http://www.cse.psu.edu/~buu1/papers/ps/park-jsa11.pdf).
4. [SQLite](https://www.sqlite.org) is the most minimalistic SQL database.
5. [MySQL](https://www.mysql.com) is the most widely used Open-Source DB in the world.
6. [Postgres](https://www.postgresql.org) is the 2nd most popular Open-Source DB.
7. [MongoDB](https://www.mongodb.com) is the most popular NoSQL database.
8. [Neo4J](https://neo4j.com) is the most popular graph database.

[The Most Popular Open Source Databases 2020](https://www.percona.com/blog/2020/04/22/the-state-of-the-open-source-database-industry-in-2020-part-three/).