---
layout: default
title: Texts
nav_order: 2
parent: Storage Solutions
permalink: /storage/texts/
---

# Text Processing Performance

Most of the data that flows into our analytical pipelines originally has textual form.
Afterwards it transforms into a more structured representation, but we often re-evaluate the same context once more related data arrives.
At such times it's crucial to fetch all the occurences of a specific string or RegEx pattern, so we need a fast text index!

[Reproduce Our Results](https://github.com/unumam/PyStorage){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 } [Our Performance Ingredients](/lectures/storage-recipe){: .btn .fs-5 .mb-4 .mb-md-0 }

## Setup

### Datasets

- [Covid 19](https://www.kaggle.com/allen-institute-for-ai/CORD-19-research-challenge) Scientific Papers.
  - Documents: 45,941.
  - Sections: 1,604,649.
  - Size: 1,7 GB.
- [Political Tweets India](https://www.kaggle.com/iamyoginth/facthub) Posts.
  - Documents: 12,488,144.
  - Sections: 12,488,144.
  - Size: 2,3 GB.
- [English Wikipedia](https://www.kaggle.com/jkkphys/english-wikipedia-articles-20170820-sqlite) Dump from 2017.
  - Documents: 4,902,648.
  - Sections: 23,046,187.
  - Size: 18,2 GB.

### Device

- CPU:
  - Model: `Intel(R) Core(TM) i9-9880H CPU @ 2.30GHz`.
  - Cores: 8 (16 threads @ 2.3 Ghz).
- RAM Space: 16.0 GB.
- Disk Space: 931.5 GB.
- OS Family: Darwin.
- Python Version: 3.7.7.

Databases were configured to use 512 MB of RAM for cache and 4 cores for query execution.

## Sequential Writes: Import CSV (docs/sec)

Every datascience project starts by importing the data.
Let's see how long it will take to load every dataset into each DB.

|               |  Covid19   | PoliticalTweetsIndia |    Gains    |
| :------------ | :--------: | :------------------: | :---------: |
| MongoDB       |   398.40   |       3,301.61       |     1x      |
| ElasticSearch |  5,915.53  |      10,262.29       |    8.98x    |
| Unum.TextDB   | 142,575.07 |      930,285.22      | **319.82x** |

|               |     Covid19     | PoliticalTweetsIndia |
| :------------ | :-------------: | :------------------: |
| MongoDB       | 1 hours, 7 mins |   50 mins, 29 secs   |
| ElasticSearch | 4 mins, 31 secs |   16 mins, 14 secs   |
| Unum.TextDB   | 0 mins, 11 secs |   0 mins, 13 secs    |

Those benchmarks only tell half of the story.
We should not only consider performance, but also the used disk space and the affect on the hardware lifetime, as SSDs don't last too long.
Unum has not only the highest performance, but also the most compact representation.
MongoDB generally performs well across different benchmarks, but it failed to import the English Wikipedia in 10 hours.
I suspect a bug in the implementation of the text index, as some batch import operations took over 10 mins for a modest batch size of 10,000 docs.

|               | Covid19 | PoliticalTweetsIndia | EnglishWikipedia |
| :------------ | :-----: | :------------------: | :--------------: |
| MongoDB       | 1,9 GB  |        3,2 GB        | Expected 60,7 GB |
| ElasticSearch | 2,5 GB  |        2,9 GB        |     33,5 GB      |
| Unum.TextDB   |    1    |          1           |        1         |

![Import Overhead - Total Bytes Written](/assets/storage-texts/Import_Overhead_-_Total_Bytes_Written.svg)

## Read Queries

### Random Reads: Lookup Doc by ID

### Random Reads: Find up to 10,000 Docs containing a Short Word

### Random Reads: Find up to 20 Docs containing a Short Word

### Random Reads: Find up to 20 Docs with Short Phrases

### Random Reads: Find up to 20 Docs containing a Long Word

### Random Reads: Find up to 20 Docs with Long Phrases

## Write Operations

### Random Writes: Upsert Doc

### Random Writes: Upsert Docs Batch

### Random Writes: Remove Doc

### Random Writes: Remove Docs Batch

---

> This page will be updated with new results soon.
