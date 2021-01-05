---
layout: default
title: Storage Solutions
nav_order: 2
has_children: true
permalink: /storage/
---

# Storage Solutions

A high-end server like [DGX A100](https://www.nvidia.com/en-us/data-center/dgx-a100/) comes with following components:

* 2x CPUs (with shared RAM),
* 8x GPUs (each with private VRAM) and
* 2x SSDs.

## How much data can potentially flow in each direction?

|                                    Channel                                    |  Bandwidth  |
| :---------------------------------------------------------------------------: | :---------: |
|          CPU ⇌ [DDR4 RAM](https://en.wikipedia.org/wiki/DDR4_SDRAM)           |  ~100 GB/s  |
|    GPU ⇌ [HBM2 VRAM](https://en.wikipedia.org/wiki/High_Bandwidth_Memory)     | ~1,000 GB/s |
|         GPU ⇌ GPU [via NVLink](https://en.wikipedia.org/wiki/NVLink)          |  ~300 GB/s  |
|   CPU ⇌ GPU [via PCI-E Gen3 x16](https://en.wikipedia.org/wiki/PCI_Express)   |  ~15 GB/s   |
| CPU ⇌ SSD [via NVMe PCI-E Gen3 x4](https://en.wikipedia.org/wiki/NVM_Express) |  << 4 GB/s  |

Logic tells, "a computer must spend it's time computing", in other words utilizing the (CPU ⇌ RAM) and (GPU ⇌ VRAM) channels. But the numbers tell a different story.

Unless you are doing very complex computations, your fast CPUs and GPUs may very well spend their clock cycles waiting for data to come from SSDs. The (CPU ⇌ SSD) link is the slowest in the chain. It's theoretical throughput (for Gen3 PCI-E) is 4 GB/s, but the real world reads often degrade to 200 MB/s.

## How to choose a DBMS for you workload?

Every business application has a custom data layout and unique access patterns. That's why we have made our DBMS more flexible. It works with more data types, than any other major company.

|          | Documents | [Texts](/storage/texts/) | [Networks](/storage/graphs/) | Vectors |
| :------: | :-------: | :----------------------: | :--------------------------: | :-----: |
| Postgres |     🐢     |            🐌             |              no              |   no    |
| MongoDB  |     👍     |            🐢             |              no              |   no    |
| Elastic  |     🐌     |            👍             |              no              |   no    |
|  Neo4J   |    no     |            no            |              🐢               |   no    |
|   Unum   |    🥇🥇🥇    |           🥇🥇🥇            |             🥇🥇🥇              |   🥇🥇🥇   |

It's worth noting that "no" means that there is no out-of-the-box support for such workloads. DBMSs can be retrofited with any functionality. [Here](https://opendistro.github.io/for-elasticsearch/blog/odfe-updates/2020/04/Building-k-Nearest-Neighbor-(k-NN)-Similarity-Search-Engine-with-Elasticsearch/) authors describe how one can do vector search on top of Elastic. The resulting performance of such configurations is generally quite low.

## How much faster UnumDB is?

We generally benchmark UnumDB against following competitors:

1. [SQLite](https://www.sqlite.org) is the most minimalistic SQL database.
2. [MySQL](https://www.mysql.com) is the most widely used Open-Source DB in the world.
3. [Postgres](https://www.postgresql.org) is the 2nd most popular Open-Source DB. Implemented in 1'300'000 lines of C++ code.
4. [MongoDB](https://www.mongodb.com) is the most popular NoSQL database. Implemented in 3'900'000 lines of C++ code.
5. [Neo4J](https://neo4j.com) is the most popular graph database. Implemented in 800'000 lines of Java code.
6. [ElasticSearch](https://www.elastic.co) is the most popular indexing software. Implemented in 300'000 lines of Java code on top the [Lucene](https://lucene.apache.org) C engine.

Abandoning legacy approaches allowed us to reach:

* 50x faster dataset imports.
* 5x smaller compressed representations.
* 100x faster random lookups.
* 10x faster batched insertions.
* compact implementation in 100'000 lines of C++ code.

[Reproduce Our Results](https://github.com/unumam/PyStorage){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 } [Our Performance Ingridients](/lectures/storage-recipe){: .btn .fs-5 .mb-4 .mb-md-0 }

To study our numbers in details, just explore the following links:
