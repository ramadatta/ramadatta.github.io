---
layout: post
title: "Scanpy `standard_scale`: var vs group (with worked calculations)"
date: 2026-02-03
description: "Understanding the difference between standard_scale='var' and 'group' in Scanpy heatmaps with worked calculations and examples"
tags: [single-cell, scanpy, scanpy-plotting, visualization]
categories: [bioinformatics]
giscus_comments: true
---

<figure style="text-align:center; margin-bottom: 1.5rem;">
  <a href="https://scanpy.readthedocs.io/en/stable/" target="_blank" rel="noopener noreferrer">
    <img src="{{ '/assets/images/scanpy-logo.svg' | relative_url }}"
         alt="Scanpy – Single-Cell Analysis in Python"
         style="max-width: 260px; height: auto;">
  </a>
  <figcaption style="margin-top: 0.5rem; font-size: 0.9rem;">
    Powered by <a href="https://scanpy.readthedocs.io/en/stable/" target="_blank" rel="noopener noreferrer">Scanpy</a>
  </figcaption>
</figure>

When you make a heatmap (or dotplot/matrixplot) in Scanpy, `standard_scale` can be a helpful option for visualization. It applies the same min–max scaling formula, but along different axes. The two settings answer different questions, so it’s worth being explicit about which one you want.

This post walks through a small example with **full calculations** so you can see exactly what changes.

---

## The two scaling modes

Both modes use min–max scaling:

\[
\text{scaled} = \frac{x - \min(x)}{\max(x) - \min(x)}
\]

The difference is where the min and max are computed:

- `standard_scale="var"`: compute min/max **per gene** across groups (column-wise)
- `standard_scale="group"`: compute min/max **per group** across genes (row-wise)

Both produce values in \([0, 1]\), but the interpretation differs depending on which axis you scaled within.

---

## The raw data

We’ll use three groups and four genes:

- **CD68, CD3D, CD19**: marker genes in this toy example
- **ACTB**: a housekeeping-style gene often used as a control

Raw values (think: average expression per group, simplified):

| Group        | CD68 | CD3D | CD19 | ACTB |
|-------------|-----:|-----:|-----:|-----:|
| Macrophages |  200 |   20 |   15 |  150 |
| T cells     |   10 |  180 |   25 |  140 |
| B cells     |   25 |   30 |  190 |  145 |

---

## `standard_scale="var"` (scale each gene across groups)

Here, each gene is scaled independently using its min and max across the groups.

### CD68 (values: 200, 10, 25)
- min = 10
- max = 200
- range = 200 − 10 = 190

Calculations:
- Macrophages: \((200 - 10) / 190 = 190/190 = 1.00\)
- T cells: \((10 - 10) / 190 = 0/190 = 0.00\)
- B cells: \((25 - 10) / 190 = 15/190 \approx 0.0789 \rightarrow 0.08\)

### CD3D (values: 20, 180, 30)
- min = 20
- max = 180
- range = 180 − 20 = 160

Calculations:
- Macrophages: \((20 - 20) / 160 = 0.00\)
- T cells: \((180 - 20) / 160 = 160/160 = 1.00\)
- B cells: \((30 - 20) / 160 = 10/160 = 0.0625 \rightarrow 0.06\)

### CD19 (values: 15, 25, 190)
- min = 15
- max = 190
- range = 190 − 15 = 175

Calculations:
- Macrophages: \((15 - 15) / 175 = 0.00\)
- T cells: \((25 - 15) / 175 = 10/175 \approx 0.0571 \rightarrow 0.06\)
- B cells: \((190 - 15) / 175 = 175/175 = 1.00\)

### ACTB (values: 150, 140, 145)
- min = 140
- max = 150
- range = 150 − 140 = 10

Calculations:
- Macrophages: \((150 - 140) / 10 = 10/10 = 1.00\)
- T cells: \((140 - 140) / 10 = 0/10 = 0.00\)
- B cells: \((145 - 140) / 10 = 5/10 = 0.50\)

### Result (`standard_scale="var"`)

| Group        | CD68 | CD3D | CD19 | ACTB |
|-------------|-----:|-----:|-----:|-----:|
| Macrophages | 1.00 | 0.00 | 0.00 | 1.00 |
| T cells     | 0.00 | 1.00 | 0.06 | 0.00 |
| B cells     | 0.08 | 0.06 | 1.00 | 0.50 |

**How to read this:** for each gene, the values show which group is highest vs lowest *for that gene*.  
A useful reminder: after `var` scaling, color/intensity is **not comparable across different genes**, because each gene has been stretched to fill \([0,1]\) on its own scale.

---

## `standard_scale="group"` (scale each group across genes)

Here, each group (row) is scaled independently using its min and max across the genes shown.

### Macrophages row (values: 200, 20, 15, 150)
- min = 15
- max = 200
- range = 200 − 15 = 185

Calculations:
- CD68: \((200 - 15) / 185 = 185/185 = 1.00\)
- CD3D: \((20 - 15) / 185 = 5/185 \approx 0.0270 \rightarrow 0.03\)
- CD19: \((15 - 15) / 185 = 0.00\)
- ACTB: \((150 - 15) / 185 = 135/185 \approx 0.7297 \rightarrow 0.73\)

### T cells row (values: 10, 180, 25, 140)
- min = 10
- max = 180
- range = 180 − 10 = 170

Calculations:
- CD68: \((10 - 10) / 170 = 0.00\)
- CD3D: \((180 - 10) / 170 = 170/170 = 1.00\)
- CD19: \((25 - 10) / 170 = 15/170 \approx 0.0882 \rightarrow 0.09\)
- ACTB: \((140 - 10) / 170 = 130/170 \approx 0.7647 \rightarrow 0.76\)

### B cells row (values: 25, 30, 190, 145)
- min = 25
- max = 190
- range = 190 − 25 = 165

Calculations:
- CD68: \((25 - 25) / 165 = 0.00\)
- CD3D: \((30 - 25) / 165 = 5/165 \approx 0.0303 \rightarrow 0.03\)
- CD19: \((190 - 25) / 165 = 165/165 = 1.00\)
- ACTB: \((145 - 25) / 165 = 120/165 \approx 0.7273 \rightarrow 0.73\)

### Result (`standard_scale="group"`)

| Group        | CD68 | CD3D | CD19 | ACTB |
|-------------|-----:|-----:|-----:|-----:|
| Macrophages | 1.00 | 0.03 | 0.00 | 0.73 |
| T cells     | 0.00 | 1.00 | 0.09 | 0.76 |
| B cells     | 0.00 | 0.03 | 1.00 | 0.73 |

**How to read this:** within each group, the values show how genes rank relative to each other *within that group*.  
A useful reminder: after `group` scaling, the same gene’s scaled values are **not directly comparable across different groups**, because each row uses its own min/max.

---

## Choosing between them

A simple way to decide is to match the setting to the question you want the figure to emphasize:

### Use `standard_scale="var"` when the plot is gene-centric
You want to see, for each gene, which group is higher vs lower (e.g., marker-style plots).

### Use `standard_scale="group"` when the plot is group-centric
You want to see, within each group, which genes stand out relative to the other genes plotted (e.g., “signature composition” within a cluster).

---

## A practical tip: keep an unscaled reference around

Scaled views can be great for patterns, but it’s often helpful to also look at an unscaled (or differently scaled) view alongside, especially when interpreting small differences or control genes.

---

## Copy/paste Scanpy snippets

These are commonly used with `matrixplot`/`dotplot`:

```python
import scanpy as sc

genes = ["CD68", "CD3D", "CD19", "ACTB"]

# Scale per gene across groups
sc.pl.matrixplot(
    adata,
    var_names=genes,
    groupby="celltype",
    standard_scale="var",
)

# Scale per group across genes
sc.pl.matrixplot(
    adata,
    var_names=genes,
    groupby="celltype",
    standard_scale="group",
)

# Unscaled reference
sc.pl.matrixplot(
    adata,
    var_names=genes,
    groupby="celltype",
    standard_scale=None,
)
```

---

## Summary

* `standard_scale="var"`: min–max per gene across groups (good for “which group is highest for gene X?”)
* `standard_scale="group"`: min–max per group across genes (good for “which genes stand out in this group?”)

Both are useful as long as you keep in mind which comparisons the scaling preserves, and which ones it changes.

