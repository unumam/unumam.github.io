# Unum.DB

A collection of high-performance persistent ACID databases.

1. [**Fastest on the Market!**](#fastest-on-the-market) 10x-100x performance improvements! Also more compact, than most DBs!
2. [**No Vendor Lock!**](#no-vendor-lock) Easy to try & go back if you don't like it! No new languages to learn!
3. [**Extreme Flexibility!**](#extreme-flexibility) Runs on servers, phones or even IoT! No JVM required!
4. [**Broad Functionality**](#broad-functionality) out of the box and easy integration with most common AI tools!

## Components

* Graphs, like [Neo4J](https://neo4j.com).
* Text Search, like [ElasticSearch](https://elastic.co).
* Tables, like [PostgreSQL](https://postgresql.org), [MySQL](https://mysql.com) and [SQLite](https://sqlite.org).
* Documents, like [MongoDB](https://mongodb.com).
* Time Series, like [InfluxDB](https://influxdata.com).
* High Dimensional Embeddings, unlike anything else.
  
## Why Unum?

### Fastest on the Market!

All benchmarks are pubulicly available. They are repeatable and include thousands of data-points. **UnumDB outperforms all competitors in all workloads!**

* [Graph Benchmarks](https://github.com/unumxyz/PyWrappedDBs/tree/master/BenchGraphs/MacbookPro).
* [Text Search Benchmarks](https://github.com/unumxyz/PyWrappedDBs/tree/master/BenchDocs/MacbookPro).

We usually beat the 2nd best result by 10x! That means 90% savings on your cloud infrastructure or 10x faster service for your customers. [Here is how we reach those numbers](#how-can-it-be-so-fast).

### No Vendor Lock!

Most DBs aren't compatible with each other, and we don't like that. <br/>

For every kind of application (graphs, text, tables), we provide generic Python interfaces. If you are not satisfied with our product, you can replace the backend implementation to something more familiar (like MongoDB, ElasticSearch or PostgreSQL). [Check out all the options here](https://github.com/unumxyz/PyWrappedDBs) .

### Extreme Flexibility!

Every aspect of UnumDB is adjustable:

* Each component can be instantiated separately or as part of a bigger general-purpose database.
* They can be persisted on-disk (like PostgreSQL) or in-memory (like MemSQL or Redis).
* They can be embedded into the current application (like SQLite or RocksDB) or run as a standalone server (like MongoDB or PostgreSQL)
* They can be installed on-premise (like MongoDB) or in the public cloud (like CosmosDB).
* They can run on any CPUs but will take advantage of SIMD instructions of modern CPUs (like) or GPUs (like).

Sounds complex? Don't worry - the installation process is single-click, the rest is auto-tuning!

### Broad Functionality

* Lossy compression & built-in quantization for big-data applications.
* Semantic Content Search combining the TextDB and VectorDB.
* High-performance pattern-matching in TextDB using our new RegEx algorithm.
* Embedded server-side scripting in Python.
* High-Bandwidth Streaming into 3-rd party AI & big-data pipelines. Providing up to 4 GB/s of data from a single commodity SSD!

The last point is particularly important! It guarantees that the functionality of UnumDB won't limit you. You can always implement a custom pipeline using third party products and swiftly stitch it with UnumDB output.

## How can it be so FAST?

Below are some of the bottlenecks we have identified in most modern DBs. <br/>
If you decide to write your own, those are the points to consider. 

|                   |            Common Solutions             |            What we use in UnumDB            |                       **Result**                        |
| :---------------- | :-------------------------------------: | :-----------------------------------------: | :-----------------------------------------------------: |
| Data layout       |          Row-wise or columnar           |          Optimal for each datatype          |         Less random jumps on SSD<br/>Affects: 💾         |
| Compression       |    Generic, but slow (Snappy, zlib)     | Newly invented algorithms for each datatype |   Writes/reads less bytes to/from SSD<br/>Affects: 💾    |
| Analytics         |     Integrating 3rd party libraries     |           Co-designed algorithms            | Optimal use of search indexes & metadata<br/>Affects: 🧠 |
| Computations      |               Sequential                |              SIMD-Accelerated               |   Processing more bytes per CPU cycle<br/>Affects: 🧠    |
| Query language    |   SQL-like with big parsing overhead    |           Simple Python-interface           |   Lower latency for simple operations<br/>Affects: 🧠    |
| Memory management |      Garbage collecting languages       |       Modern C++ with smart pointers        |     Reusing RAM & avoiding GC stalls<br/>Affects: 🐏     |
| In-Memory copies  | 1+ per read/write + DB cache + OS cache |      1 per write + DB cache + OS cache      |     Fitting more data-points in RAM<br/>Affects: 🐏      |
| Parallelism       |            Multi-processing             |        Asynchronous multi-threading         |     Faster sharing between CPU cores<br/>Affects: 🧠     |
| Communications    |                 TCP/IP                  |    DMA or Infiniband RDMA (in a cluster)    |      Faster sharing between servers<br/>Affects: 📡      |
| Serialization     |           Plain text or JSON            |                   Binary                    |     No serialization overhead on CPU<br/>Affects: 🧠     |

Interested? [Get in touch for a demo!](mailto:info@unum.xyz).