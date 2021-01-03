---
layout: default
title: Vectors
nav_order: 3
parent: Storage Solutions
permalink: /storage/vectors/
---

# Vectors Processing Performance

Almost every modern AI solution uses "embeddings" or "vectors" as intermediate representation. DL can't work directly with words or discrete entities like "building" or "Coliseum". You first encode them into some high-dimensional numeric represenation (like `{0.4, 0.2, 0.3, 0.9, 0.8}`) and then pass such vectors into your NLP pipeline.

Needless to say, you need to store the registry of all encodings. Typically, embeddings have under 1000 dimensions, each encoded with `float32_t`. So each vector can take upto 4 MB and a million of such vectors will fit into RAM. If you have a billion, things get much more complicated and you need UnumDB.

[Reproduce Our Results](github.com/unumam/PyStorageBenchmarks){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 } [Our Performance Ingridients](/storage/recipe){: .btn .fs-5 .mb-4 .mb-md-0 }

## Sequential Writes: Import Vectors

## Read Queries

### Random Reads: Top 1 Nearest Neighbors

### Random Reads: Top 10 Nearest Neighbors

### Random Reads: Lookup Vector

## Write Operations

### Random Writes: Insert Vector

### Random Writes: Insert Vectors Batch

### Random Writes: Remove Vector

### Random Writes: Remove Vectors Batch
