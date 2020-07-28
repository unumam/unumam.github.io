# Unum.DB

Unum.DB is a high-performance persistent [ACID database](https://en.wikipedia.org/wiki/ACID) that supports a wide variety of workloads:

* Graphs, like [Neo4J](https://neo4j.com).
* Text Search, like [ElasticSearch](https://elastic.co).
* Tables, like [PostgreSQL](https://postgresql.org), [MySQL](https://mysql.com) and [SQLite](https://sqlite.org).
* Documents, like [MongoDB](https://mongodb.com).
* Time Series, like [InfluxDB](https://influxdata.com).
* High Dimensional Embeddings, unlike anything else.
  
## Why Unum?

1. [**Fastest on the Market!**](#fastest-on-the-market) 10x-100x performance improvements! Also more compact, than most DBs!
2. [**No Vendor Lock!**](#no-vendor-lock) Easy to try & go back if you don't like it! No new languages to learn!
3. [**Extreme Flexibility!**](#extreme-flexibility) Runs on servers, phones or even IoT! No JVM required!
4. [**Broad Functionality**](#broad-functionality) out of the box and easy integration with most common AI tools!

### Fastest on the Market!

All benchmarks are pubulicly available. They are repeatable and include thousands of data-points. **UnumDB outperforms all competitors in all workloads!**

* [Graph Benchmarks](https://github.com/unumxyz/PyWrappedDBs/tree/master/BenchGraphs/MacbookPro).
* [Text Search Benchmarks](https://github.com/unumxyz/PyWrappedDBs/tree/master/BenchDocs/MacbookPro).

We usually beat the 2nd best result by 10x! That means **90% savings** on your cloud infrastructure or 10x faster service for your customers. [Here is how we reach those numbers](#how-can-it-be-so-fast).

### No Vendor Lock!

Most DBs aren't compatible with each other, and we don't like that. <br/>

For every kind of application (graphs, text, tables), we provide generic Python interfaces. If you are not satisfied with our product, you can replace the backend implementation to something more familiar (like MongoDB, ElasticSearch or PostgreSQL). [Check out all the options here](https://github.com/unumxyz/PyWrappedDBs).

### Extreme Flexibility!

Every aspect of UnumDB is adjustable:

* Each workload specialization can be 
  * used separately (for simple graphs or plain texts) or 
  * as part of a bigger general-purpose database.
* The data can be persisted 
  * on-disk (like PostgreSQL) or 
  * in-memory (like MemSQL or Redis).
* The software can be ditributed as 
  * embedded library (like SQLite or RocksDB) or 
  * a standalone server app (like MongoDB or PostgreSQL).
* It can be installed
  * on-premise (like MongoDB) or 
  * in the public cloud (like CosmosDB).
* It will run
  * on any desktop, mobile or server CPUs 
  * but will work faster on modern high-end hardware.

Sounds complex? Don't worry - the installation process is single-click, the rest is auto-tuning!

### Broad Functionality

* Lossy compression & built-in quantization for big-data applications.
* Semantic Content Search combining the TextDB and VectorDB.
* High-performance pattern-matching in TextDB using our new RegEx algorithm.
* Embedded server-side scripting in Python.
* High-Bandwidth Streaming into 3-rd party AI & big-data pipelines. **Providing up to 4 GB/s of data from a single commodity SSD!**

The last point is particularly important! It guarantees that the functionality of UnumDB won't limit you. You can always implement a custom pipeline using third party products and swiftly stitch it with UnumDB output.

## How can it be so FAST?

Below are some of the bottlenecks we have identified in most modern DBs. <br/>
If you decide to write your own, those are the points to consider. 

|                   |            Common Solutions             |          UnumDB Approach          |             **Consequences**             |
| :---------------- | :-------------------------------------: | :-------------------------------: | :--------------------------------------: |
| Data layout       |          Row-wise or columnar           |     Optimal for each datatype     |       Less random jumps on **SSD**       |
| Compression       |    Generic, but slow (Snappy, zlib)     |     Newly invented algorithms     | Writes/reads less bytes to/from **SSD**  |
| Analytics         |     Integrating 3rd party libraries     |      Co-designed algorithms       | Optimal use of search indexes by **CPU** |
| Computations      |               Sequential                |         SIMD-Accelerated          | Processing more bytes per **CPU** cycle  |
| Query language    |   SQL-like with big parsing overhead    |      Simple Python-interface      |   Lower latency for simple operations    |
| Memory management |      Garbage collecting languages       |  Modern C++ with smart pointers   |   Reusing **RAM** & avoiding GC stalls   |
| In-Memory copies  | >1 per read/write + DB cache + OS cache | 1 per write + DB cache + OS cache |   Fitting more data-points in **RAM**    |
| Parallelism       |            Multi-processing             |   Asynchronous multi-threading    |   Faster sharing between **CPU** cores   |
| Communications    |                 TCP/IP                  |      DMA or Infiniband RDMA       |      Faster sharing between servers      |
| Serialization     |           Plain text or JSON            |              Binary               |   No serialization overhead on **CPU**   |

**Interested? [Get in touch for a demo!](mailto:info@unum.xyz)**