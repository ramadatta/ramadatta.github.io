---
layout: post
title: "Getting Started with Spatial Transcriptomics Analysis"
date: 2025-12-10
description: "An introduction to analyzing 10x Visium data with Squidpy and Scanpy"
tags: [spatial, tutorial, python, Visium]
categories: [bioinformatics]
---

Spatial transcriptomics adds a crucial dimension to single-cell analysis: location. Here's how to get started.

## What is Spatial Transcriptomics?

Unlike scRNA-seq, spatial methods preserve tissue architecture. You can see:

→ Where cells are located

→ How they interact with neighbors

→ Tissue-level organization

## The Basic Workflow

### 1. Load Your Data

```python
import scanpy as sc
import squidpy as sq

adata = sq.read.visium('path/to/data/')
```

### 2. Quality Control

```python
sc.pp.calculate_qc_metrics(adata, inplace=True)
sc.pl.spatial(adata, color='total_counts')
```

### 3. Preprocessing

Same as scRNA-seq:

```python
sc.pp.normalize_total(adata)
sc.pp.log1p(adata)
sc.pp.highly_variable_genes(adata)
```

### 4. Spatial Analysis

This is where it gets interesting:

```python
# Neighborhood enrichment
sq.gr.spatial_neighbors(adata)
sq.gr.nhood_enrichment(adata, cluster_key='cluster')

# Spatial autocorrelation
sq.gr.spatial_autocorr(adata, genes=['gene1', 'gene2'])
```

## Key Concepts

→ **Neighborhood enrichment** — Which cell types cluster together?

→ **Spatial autocorrelation** — Is gene expression spatially organized?

→ **Ligand-receptor analysis** — Cell-cell communication in space

---

*Spatial transcriptomics is the future. Have you tried it? Share your experience in the comments!*

