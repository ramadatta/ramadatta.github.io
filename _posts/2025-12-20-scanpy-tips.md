---
layout: post
title: "5 Scanpy Tips I Wish I Knew Earlier"
date: 2025-12-20
description: "Practical tips for single-cell RNA-seq analysis with Scanpy that will save you hours"
tags: [python, scanpy, scRNA-seq, tutorial]
categories: [bioinformatics]
---

After years of working with single-cell data, I've accumulated some tricks that have significantly improved my workflow. Here are five Scanpy tips that I wish someone had told me when I started.

## 1. Use Backed Mode for Large Datasets

When working with datasets that don't fit in memory:

```python
adata = sc.read_h5ad('large_dataset.h5ad', backed='r')
```

This loads the data lazily, only reading what you need.

## 2. Cache Your Preprocessing

Save intermediate results to avoid recomputing:

```python
import os

cache_file = 'preprocessed.h5ad'
if os.path.exists(cache_file):
    adata = sc.read_h5ad(cache_file)
else:
    # preprocessing steps
    sc.pp.normalize_total(adata)
    sc.pp.log1p(adata)
    adata.write(cache_file)
```

## 3. Parallel Processing with scanpy

Use the `n_jobs` parameter when available:

```python
sc.pp.neighbors(adata, n_neighbors=30, n_jobs=8)
```

## 4. Better UMAP Reproducibility

Set random state everywhere:

```python
sc.tl.umap(adata, random_state=42)
sc.tl.leiden(adata, random_state=42)
```

## 5. Memory-Efficient Gene Selection

Instead of keeping all genes in memory:

```python
sc.pp.highly_variable_genes(adata, n_top_genes=2000)
adata = adata[:, adata.var.highly_variable]
```

---

*What are your favorite Scanpy tips? Share them in the comments below!*

