---
layout: post
title: "Scanpy `standard_scale`: var vs group — finally clear (with manual calculations)"
date: 2026-02-03
description: "Deep-dive into standard_scale='var' vs 'group' in Scanpy heatmaps, dotplots & matrixplots — complete with step-by-step math and when to choose each."
tags: [single-cell, scanpy, heatmap, visualization, bioinformatics]
categories: [single-cell-analysis]
giscus_comments: true
toc: true
---

<div style="text-align:center; margin: 2.5rem 0;">
  <a href="https://scanpy.readthedocs.io/en/stable/" target="_blank" rel="noopener noreferrer">
    <img src="{{ '/assets/images/scanpy-logo.svg' | relative_url }}"
         alt="Scanpy – Single-Cell Analysis in Python"
         style="max-width: 280px; height: auto; filter: drop-shadow(0 4px 12px rgba(0,0,0,0.12));">
  </a>
  <p style="margin-top: 1rem; font-size: 1.1rem; color: #555; font-style: italic;">
    source: <a href="https://scanpy.readthedocs.io/" target="_blank">Scanpy</a>
  </p>
</div>

When creating heatmaps, dotplots or matrixplots in Scanpy, the `standard_scale` parameter dramatically changes what the reader perceives — even though both options use the **same min-max formula**:

$$
\text{scaled} = \frac{x - \min}{\max - \min} \quad \in [0,1]
$$

The **only difference** is **along which axis** the min & max are calculated.

- `standard_scale="var"`   → **column-wise** (per gene across groups)  
- `standard_scale="group"` → **row-wise**   (per group across genes)

Let’s compute both versions manually on a tiny realistic toy dataset so the difference becomes intuitive.

---

## Toy dataset — mean expression per cluster

| Cluster       | CD68  | CD3D  | CD19  | ACTB  |
|---------------|-------|-------|-------|-------|
| Macrophages   | 200   | 20    | 15    | 150   |
| T cells       | 10    | 180   | 25    | 140   |
| B cells       | 25    | 30    | 190   | 145   |

Classic markers + one housekeeping gene.

---

## Option 1: `standard_scale="var"` — scale **each gene** across clusters

**Goal:** answer “Which cluster expresses this gene the most / least?”

For every **column**, compute its own min & max.

**CD68** (10, 200, 25) → min=10, max=200, range=190  
Macrophages: (200-10)/190 = **1.00**  
T cells:     (10-10)/190  = **0.00**  
B cells:     (25-10)/190  ≈ **0.08**

**CD3D** (20, 180, 30) → min=20, max=180, range=160  
→ 0.00 | **1.00** | 0.06

**CD19** (15, 25, 190) → min=15, max=190, range=175  
→ 0.00 | 0.06 | **1.00**

**ACTB** (150, 140, 145) → min=140, max=150, range=10  
→ **1.00** | 0.00 | 0.50

**Scaled matrix (`"var"`):**

| Cluster       | CD68  | CD3D  | CD19  | ACTB  |
|---------------|-------|-------|-------|-------|
| Macrophages   | **1.00** | 0.00  | 0.00  | **1.00** |
| T cells       | 0.00  | **1.00** | 0.06  | 0.00  |
| B cells       | 0.08  | 0.06  | **1.00** | 0.50  |

> **Key insight**  
> After `"var"` scaling you **cannot compare intensity across genes** — every gene is stretched to use the full [0–1] range independently.  
> Perfect when you want to spot **marker specificity**.

---

## Option 2: `standard_scale="group"` — scale **each cluster** across genes

**Goal:** answer “Which genes stand out the most **inside this cluster**?”

For every **row**, compute its own min & max.

**Macrophages** (200, 20, 15, 150) → min=15, max=200, range=185  
CD68: **1.00** | CD3D: 0.03 | CD19: 0.00 | ACTB: 0.73

**T cells** (10, 180, 25, 140) → min=10, max=180, range=170  
→ 0.00 | **1.00** | 0.09 | 0.76

**B cells** (25, 30, 190, 145) → min=25, max=190, range=165  
→ 0.00 | 0.03 | **1.00** | 0.73

**Scaled matrix (`"group"`):**

| Cluster       | CD68  | CD3D  | CD19  | ACTB  |
|---------------|-------|-------|-------|-------|
| Macrophages   | **1.00** | 0.03  | 0.00  | 0.73  |
| T cells       | 0.00  | **1.00** | 0.09  | 0.76  |
| B cells       | 0.00  | 0.03  | **1.00** | 0.73  |

> **Key insight**  
> Now gene values **cannot be compared across clusters** — each cluster has its own internal [0–1] stretch.  
> Excellent when showing **intra-cluster gene ranking** or signature composition.

---

## Quick decision guide

| Question you want the figure to answer                                 | Recommended     | Typical cmap suggestion     |
|-----------------------------------------------------------------------|------------------|------------------------------|
| Which cluster has the highest expression of **gene X**?               | `"var"`          | sequential (e.g. Blues, viridis, Reds) |
| Within this cluster, which genes are relatively strongest?            | `"group"`        | sequential (YlOrRd, Purples…) |
| I want absolute comparison across everything                          | `None`           | diverging or raw scale       |

---

## Ready-to-copy Scanpy snippets

```python
import scanpy as sc

marker_genes = ["CD68", "CD3D", "CD19", "ACTB"]
groupby     = "celltype"          # or "leiden", "anno"...

# Most common: per-gene scaling (marker view)
sc.pl.matrixplot(adata, marker_genes, groupby,
                 standard_scale="var",
                 cmap="Blues", dendrogram=True,
                 title="Per-gene scaling — marker specificity")

# Per-group scaling (signature composition view)
sc.pl.matrixplot(adata, marker_genes, groupby,
                 standard_scale="group",
                 cmap="YlOrRd", dendrogram=True,
                 title="Per-group scaling — intra-cluster ranking")

# Raw / absolute reference (often surprisingly useful)
sc.pl.matrixplot(adata, marker_genes, groupby,
                 standard_scale=None,
                 cmap="viridis", layer="log1p",
                 title="No scaling — absolute means")
```

Bonus: try `sc.pl.dotplot(…)` with the same `standard_scale` logic — same interpretation applies.

---

## TL;DR takeaway

- `"var"`   = compare **clusters within one gene** → great for marker panels  
- `"group"` = compare **genes within one cluster** → great for cluster signatures  
- Always ask: *“What comparison do I actually want to preserve?”*

Happy plotting!
