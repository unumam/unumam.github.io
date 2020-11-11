# Unum.DB

Unum.DB is a high-performance persistent [ACID database](https://en.wikipedia.org/wiki/ACID) that supports a wide variety of workloads:

* Graphs, like [Neo4J](https://neo4j.com).
* Text Search, like [ElasticSearch](https://elastic.co).
* Tables, like [PostgreSQL](https://postgresql.org), [MySQL](https://mysql.com) and [SQLite](https://sqlite.org).
* Documents, like [MongoDB](https://mongodb.com).
* Time Series, like [InfluxDB](https://influxdata.com).
* High Dimensional Embeddings, unlike anything else.
  
## Why Unum?

1. [Fastest on the Market!](#fastest-on-the-market) 10x-100x performance improvements! Also more compact, than most DBs!
2. [No Vendor Lock!](#no-vendor-lock) Easy to try & go back if you don't like it! No new languages to learn!
3. [Extreme Flexibility!](#extreme-flexibility) Runs on servers, phones or even IoT! No JVM required!
4. [Broad Functionality](#broad-functionality) out of the box and easy integration with most common AI tools!

### Fastest on the Market!

All benchmarks are pubulicly available. They are repeatable and include thousands of data-points. **UnumDB outperforms all competitors in all workloads!**

* [Graph Benchmarks](https://unum.xyz/db/graph).
* [Text Search Benchmarks](https://unum.xyz/db/text).

We usually beat the 2nd best result by 10x! That means **90% savings** on your cloud infrastructure or 10x faster service for your customers. [Here is how we reach those numbers](#how-can-it-be-so-fast).

### No Vendor Lock!

Most DBs aren't compatible with each other, and we don't like that. <br/>
For every kind of workload (graphs, text, tables), we provide generic Python interfaces. <br/>
If you are not satisfied with our product, you can replace the backend implementation to something more familiar (like MongoDB, ElasticSearch or PostgreSQL). <br/>
[Check out all the options here](https://github.com/unumxyz/PyWrappedDBs).

### Extreme Flexibility!

Every aspect of UnumDB is adjustable:

* The data can be persisted
  * in-memory (like MemSQL or Redis) for real-time analytics or
  * on-disk (like PostgreSQL) for big-data processing.
* The software can be ditributed as
  * embedded library (like SQLite or RocksDB) for datasets under 10 TB or
  * a standalone server app (like MongoDB or PostgreSQL) for larger collections.
* It will run
  * on any desktop, mobile or IoT device,
  * on local cluster of servers or
  * in the public clouds like AWS and Azure.

### Broad Functionality

* Lossy compression & built-in quantization for big-data applications.
* Semantic Content Search for text collections.
* High-performance pattern-matching using our new RegEx algorithms.
* Embedded server-side scripting in Python.
* High-Bandwidth Streaming into 3-rd party AI & big-data pipelines. **Providing up to 4 GB/s of data from a single commodity SSD!**

The last point is particularly important! It guarantees that the functionality of UnumDB won't limit you. You can always implement a custom pipeline using third party products and swiftly stitch it with UnumDB output.

## How can it be so FAST?

Below are some of the bottlenecks we have identified in most modern DBs.<br/>
If you decide to write your own, those are the points to consider.

### SSD

* Data layout
  * Others: Row-wise or columnar
  * Unum: Optimal for each datatype
  * Consequences: Less random jumps and more sequential ops

* Compression
  * Others: Generic, but slow ([Snappy](https://google.github.io/snappy/), [zlib](https://zlib.net))
  * Unum: Newly invented algorithms
  * Consequences: Writes/reads less data

### CPU

* Search algorithms
  * Others: Text-book solutions from 1980s
  * Unum: Co-designed new algorithms
  * Consequences: Effective use of buffered probablistic data-structures
  
* Algorithm implementations
  * Others: Sequential
  * Unum: [SIMD](https://en.wikipedia.org/wiki/SIMD)-accelerated
  * Consequences: Processing more bytes per cycle

* Parallelism
  * Other: Multi-processing
  * Unum: Asynchronous multi-threading
  * Consequences: Faster sharing between cores

* Serialization
  * Others: Plain text or JSON
  * Unum: Binary
  * Consequences: No serialization overhead

* Query language
  * Others: SQL-like with big parsing overhead
  * Unum: Simple Python-bindings
  * Consequences: Lower latency for point-wise lookups

### RAM

* Memory management
  * Others: Garbage collecting languages and runtimes
  * Unum: Modern C++ with smart pointers
  * Consequences: Avoids GC stalls
  
* In-memory copies
  * Other: >1 per read/write + DB cache + OS cache
  * Unum: 1 per write + DB cache
  * Consequences: Fitting more data-points in cache
  
### WEB

* In-cluster communications
  * Others: TCP/IP
  * Unum: DMA or Infiniband RDMA
  * Consequences: Faster sharing between servers

**Interested? [Get in touch for a demo!](mailto:info@unum.xyz)**