---
layout: post
title: "DESeq2 vs edgeR: A Practical Guide for RNA-seq Differential Expression"
date: 2026-02-16
description: "A practical, under-the-hood guide for choosing between DESeq2 and edgeR for RNA-seq differential expression analysis, covering statistical foundations, design specification, and best practices."
tags: [RNA-seq, differential-expression, DESeq2, edgeR, bioinformatics, statistics]
categories: [bioinformatics]
giscus_comments: true
toc: true
---

## For the impatient: what should I choose

<div id="decision-flowchart" style="margin:2em 0;user-select:none;">
<style>
@import url('https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;500;600;700&family=JetBrains+Mono:wght@400;500;600;700&display=swap');

#decision-flowchart svg { width:100%; max-width:1200px; display:block; margin:0 auto; }

/* Animated flowing dashes on paths */
@keyframes flowDash {
  to { stroke-dashoffset: -24; }
}
@keyframes flowDashSlow {
  to { stroke-dashoffset: -32; }
}
@keyframes pulseGlow {
  0%, 100% { opacity: 0.5; }
  50% { opacity: 1; }
}
@keyframes fadeSlideIn {
  from { opacity:0; transform:translateY(8px); }
  to { opacity:1; transform:translateY(0); }
}
@keyframes subtlePulse {
  0%, 100% { filter: drop-shadow(0 0 3px var(--glow, transparent)); }
  50% { filter: drop-shadow(0 0 10px var(--glow, transparent)); }
}

.fc-path {
  stroke-dasharray: 6, 6;
  animation: flowDash 0.8s linear infinite;
}
.fc-path-long {
  stroke-dasharray: 8, 8;
  animation: flowDashSlow 1s linear infinite;
}

/* Node hover lift */
.fc-node { transition: filter 0.25s, transform 0.25s; cursor: default; }
.fc-node:hover { filter: brightness(1.15) drop-shadow(0 4px 12px rgba(0,0,0,0.15)); }

/* Terminal node glow pulse */
.fc-terminal { animation: subtlePulse 3s ease-in-out infinite; }
.fc-terminal-deseq2 { --glow: #42c0c9; }
.fc-terminal-edger  { --glow: #42c0c9; }
.fc-terminal-warn   { --glow: #f48c06; }
.fc-terminal-voom   { --glow: #42c0c9; }

/* Staggered entrance */
.fc-anim-1 { animation: fadeSlideIn 0.5s 0.05s both; }
.fc-anim-2 { animation: fadeSlideIn 0.5s 0.15s both; }
.fc-anim-3 { animation: fadeSlideIn 0.5s 0.25s both; }
.fc-anim-4 { animation: fadeSlideIn 0.5s 0.35s both; }
.fc-anim-5 { animation: fadeSlideIn 0.5s 0.45s both; }
.fc-anim-6 { animation: fadeSlideIn 0.5s 0.55s both; }
.fc-anim-7 { animation: fadeSlideIn 0.5s 0.65s both; }

/* Caption */
.fc-caption {
  text-align:center; margin-top:12px;
  font-family:'DM Sans',sans-serif; font-size:13px; color:#666666;
  line-height:1.5;
}
.fc-caption strong { color:#000000; }
</style>

<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 900 680" style="font-family:'DM Sans',sans-serif;">
  <defs>
    <!-- Subtle grid pattern -->
    <pattern id="fcGrid" x="0" y="0" width="30" height="30" patternUnits="userSpaceOnUse">
      <path d="M30 0V30H0" fill="none" stroke="#e0e0e0" stroke-width="0.5" opacity="0.3"/>
    </pattern>
    <!-- Noise texture -->
    <filter id="fcNoise">
      <feTurbulence type="fractalNoise" baseFrequency="0.9" numOctaves="4" stitchTiles="stitch" result="noise"/>
      <feColorMatrix type="saturate" values="0" in="noise" result="grey"/>
      <feBlend in="SourceGraphic" in2="grey" mode="multiply"/>
    </filter>
    <!-- Glow filters - updated to theme colors -->
    <filter id="glowAmber" x="-20%" y="-20%" width="140%" height="140%">
      <feGaussianBlur stdDeviation="6" result="b"/>
      <feFlood flood-color="#f48c06" flood-opacity="0.25" result="c"/>
      <feComposite in="c" in2="b" operator="in" result="g"/>
      <feMerge><feMergeNode in="g"/><feMergeNode in="SourceGraphic"/></feMerge>
    </filter>
    <filter id="glowTeal" x="-20%" y="-20%" width="140%" height="140%">
      <feGaussianBlur stdDeviation="6" result="b"/>
      <feFlood flood-color="#42c0c9" flood-opacity="0.25" result="c"/>
      <feComposite in="c" in2="b" operator="in" result="g"/>
      <feMerge><feMergeNode in="g"/><feMergeNode in="SourceGraphic"/></feMerge>
    </filter>
    <filter id="cardShadow" x="-5%" y="-5%" width="110%" height="120%">
      <feDropShadow dx="0" dy="3" stdDeviation="5" flood-color="#000" flood-opacity="0.1"/>
    </filter>
    <!-- Gradient backgrounds for cards - light theme -->
    <linearGradient id="bgStart" x1="0" y1="0" x2="1" y2="1">
      <stop offset="0%" stop-color="#f0f9fa"/><stop offset="100%" stop-color="#ffffff"/>
    </linearGradient>
    <linearGradient id="bgQuestion" x1="0" y1="0" x2="1" y2="1">
      <stop offset="0%" stop-color="#ffffff"/><stop offset="100%" stop-color="#f8f8f8"/>
    </linearGradient>
    <linearGradient id="bgChoice" x1="0" y1="0" x2="0" y2="1">
      <stop offset="0%" stop-color="#ffffff"/><stop offset="100%" stop-color="#f8f8f8"/>
    </linearGradient>
  </defs>

  <!-- Background -->
  <rect width="900" height="680" rx="16" fill="#ffffff" stroke="#e0e0e0" stroke-width="1"/>
  <rect width="900" height="680" rx="16" fill="url(#fcGrid)" opacity="0.4"/>

  <!-- ═══════ PATHS (drawn first, behind nodes) ═══════ -->

  <!-- Start → 3 data type options (curved) -->
  <!-- Start → Bulk (left) -->
  <path d="M340 58 C340 85, 195 85, 195 108" fill="none" stroke="#42c0c9" stroke-width="1.8" class="fc-path" opacity="0.6"/>
  <!-- Start → Pseudo-bulk (center) -->
  <path d="M450 58 L450 108" fill="none" stroke="#42c0c9" stroke-width="1.8" class="fc-path" opacity="0.6"/>
  <!-- Start → Single-cell (right) -->
  <path d="M560 58 C560 85, 720 85, 720 108" fill="none" stroke="#42c0c9" stroke-width="1.8" class="fc-path" opacity="0.6"/>

  <!-- Bulk → Sample size -->
  <path d="M195 152 C195 175, 330 175, 330 198" fill="none" stroke="#42c0c9" stroke-width="1.8" class="fc-path" opacity="0.6"/>
  <!-- Pseudo → Sample size -->
  <path d="M450 152 C450 175, 450 175, 450 198" fill="none" stroke="#42c0c9" stroke-width="1.8" class="fc-path" opacity="0.6"/>

  <!-- Sample size → n=1, n=2-3, n≥4 -->
  <path d="M330 242 C330 265, 150 262, 150 288" fill="none" stroke="#f48c06" stroke-width="1.8" class="fc-path" opacity="0.6"/>
  <path d="M420 242 L420 288" fill="none" stroke="#42c0c9" stroke-width="1.8" class="fc-path" opacity="0.6"/>
  <path d="M510 242 C510 265, 630 262, 630 288" fill="none" stroke="#42c0c9" stroke-width="1.8" class="fc-path" opacity="0.6"/>

  <!-- n=1 → STOP -->
  <path d="M150 332 L150 378" fill="none" stroke="#f48c06" stroke-width="2" class="fc-path-long" opacity="0.7"/>
  <!-- n=2-3 → edgeR small -->
  <path d="M420 332 L420 378" fill="none" stroke="#42c0c9" stroke-width="2" class="fc-path-long" opacity="0.7"/>
  <!-- n≥4 → Priority -->
  <path d="M630 332 L630 378" fill="none" stroke="#42c0c9" stroke-width="1.8" class="fc-path" opacity="0.6"/>

  <!-- Priority → 3 choices -->
  <path d="M560 422 C560 448, 420 445, 420 468" fill="none" stroke="#42c0c9" stroke-width="1.8" class="fc-path" opacity="0.6"/>
  <path d="M630 422 L630 468" fill="none" stroke="#42c0c9" stroke-width="1.8" class="fc-path" opacity="0.6"/>
  <path d="M700 422 C700 448, 810 445, 810 468" fill="none" stroke="#42c0c9" stroke-width="1.8" class="fc-path" opacity="0.6"/>

  <!-- Choice → Terminal -->
  <path d="M420 512 L420 558" fill="none" stroke="#42c0c9" stroke-width="2" class="fc-path-long" opacity="0.7"/>
  <path d="M630 512 L630 558" fill="none" stroke="#42c0c9" stroke-width="2" class="fc-path-long" opacity="0.7"/>
  <path d="M810 512 L810 558" fill="none" stroke="#42c0c9" stroke-width="2" class="fc-path-long" opacity="0.7"/>

  <!-- SC → SC Tools -->
  <path d="M720 152 L720 198" fill="none" stroke="#f48c06" stroke-width="1.8" class="fc-path" opacity="0.6"/>

  <!-- ═══════ NODES ═══════ -->

  <!-- Row 0: START -->
  <g class="fc-node fc-anim-1">
    <rect x="320" y="18" width="260" height="40" rx="20" fill="url(#bgStart)" stroke="#42c0c9" stroke-width="2" filter="url(#cardShadow)"/>
    <text x="450" y="43" text-anchor="middle" fill="#000000" font-size="14" font-weight="700">What type of RNA-seq data?</text>
  </g>

  <!-- Row 1: Data types -->
  <g class="fc-node fc-anim-2">
    <rect x="115" y="108" width="160" height="44" rx="8" fill="url(#bgChoice)" stroke="#42c0c9" stroke-width="1.5" filter="url(#cardShadow)"/>
    <text x="195" y="135" text-anchor="middle" fill="#000000" font-size="13" font-weight="600">Bulk RNA-seq</text>
  </g>
  <g class="fc-node fc-anim-2">
    <rect x="350" y="108" width="200" height="44" rx="8" fill="url(#bgChoice)" stroke="#42c0c9" stroke-width="1.5" filter="url(#cardShadow)"/>
    <text x="450" y="135" text-anchor="middle" fill="#000000" font-size="13" font-weight="600">Pseudo-bulk (scRNA-seq)</text>
  </g>
  <g class="fc-node fc-anim-2">
    <rect x="630" y="108" width="180" height="44" rx="8" fill="url(#bgChoice)" stroke="#42c0c9" stroke-width="1.5" filter="url(#cardShadow)"/>
    <text x="720" y="135" text-anchor="middle" fill="#000000" font-size="13" font-weight="600">Single-cell (cell-level)</text>
  </g>

  <!-- Row 2: Sample size question -->
  <g class="fc-node fc-anim-3">
    <rect x="280" y="198" width="280" height="44" rx="8" fill="url(#bgQuestion)" stroke="#42c0c9" stroke-width="2" filter="url(#cardShadow)"/>
    <text x="287" y="217" fill="#42c0c9" font-size="10" font-weight="700" font-family="'JetBrains Mono',monospace">?</text>
    <text x="420" y="226" text-anchor="middle" fill="#000000" font-size="13" font-weight="600">How many biological replicates?</text>
  </g>

  <!-- SC → Warning -->
  <g class="fc-node fc-anim-3 fc-terminal fc-terminal-warn" filter="url(#glowAmber)">
    <rect x="630" y="198" width="180" height="56" rx="8" fill="#fff5eb" stroke="#f48c06" stroke-width="1.5"/>
    <text x="720" y="221" text-anchor="middle" fill="#f48c06" font-size="11" font-weight="700">⚠ scRNA-seq tools</text>
    <text x="720" y="239" text-anchor="middle" fill="#e85d04" font-size="10.5">Aggregate to pseudo-bulk</text>
  </g>

  <!-- Row 3: Sample size options -->
  <g class="fc-node fc-anim-4">
    <rect x="80" y="288" width="140" height="44" rx="8" fill="url(#bgChoice)" stroke="#f48c06" stroke-width="1.5" filter="url(#cardShadow)"/>
    <text x="150" y="310" text-anchor="middle" fill="#000000" font-size="13" font-weight="700">n = 1</text>
    <text x="150" y="324" text-anchor="middle" fill="#666666" font-size="10">no replicates</text>
  </g>
  <g class="fc-node fc-anim-4">
    <rect x="340" y="288" width="160" height="44" rx="8" fill="url(#bgChoice)" stroke="#42c0c9" stroke-width="1.5" filter="url(#cardShadow)"/>
    <text x="420" y="310" text-anchor="middle" fill="#000000" font-size="13" font-weight="700">n = 2–3</text>
    <text x="420" y="324" text-anchor="middle" fill="#666666" font-size="10">small</text>
  </g>
  <g class="fc-node fc-anim-4">
    <rect x="560" y="288" width="140" height="44" rx="8" fill="url(#bgChoice)" stroke="#42c0c9" stroke-width="1.5" filter="url(#cardShadow)"/>
    <text x="630" y="310" text-anchor="middle" fill="#000000" font-size="13" font-weight="700">n ≥ 4</text>
    <text x="630" y="324" text-anchor="middle" fill="#666666" font-size="10">moderate+</text>
  </g>

  <!-- Row 4: STOP + edgeR small + Priority -->
  <g class="fc-node fc-anim-5 fc-terminal fc-terminal-warn" filter="url(#glowAmber)">
    <rect x="48" y="378" width="204" height="64" rx="8" fill="#fff5eb" stroke="#f48c06" stroke-width="2"/>
    <text x="150" y="402" text-anchor="middle" fill="#f48c06" font-size="13" font-weight="700">⛔ STOP</text>
    <text x="150" y="420" text-anchor="middle" fill="#e85d04" font-size="10.5">DE is not reliable.</text>
    <text x="150" y="434" text-anchor="middle" fill="#d00000" font-size="10">Cannot estimate biological variability.</text>
  </g>

  <g class="fc-node fc-anim-5 fc-terminal fc-terminal-edger" filter="url(#glowTeal)">
    <rect x="310" y="378" width="220" height="64" rx="8" fill="#f0f9fa" stroke="#42c0c9" stroke-width="2"/>
    <text x="420" y="402" text-anchor="middle" fill="#42c0c9" font-size="13" font-weight="700">→ edgeR QL pipeline</text>
    <text x="420" y="420" text-anchor="middle" fill="#2a9ba3" font-size="10.5">Better Type I error control</text>
    <text x="420" y="434" text-anchor="middle" fill="#2a9ba3" font-size="10">with small sample sizes</text>
  </g>

  <g class="fc-node fc-anim-5">
    <rect x="570" y="378" width="180" height="44" rx="8" fill="url(#bgQuestion)" stroke="#42c0c9" stroke-width="2" filter="url(#cardShadow)"/>
    <text x="577" y="397" fill="#42c0c9" font-size="10" font-weight="700" font-family="'JetBrains Mono',monospace">?</text>
    <text x="660" y="406" text-anchor="middle" fill="#000000" font-size="12.5" font-weight="600">What matters most?</text>
  </g>

  <!-- Row 5: Priority choices -->
  <g class="fc-node fc-anim-6">
    <rect x="330" y="468" width="180" height="44" rx="8" fill="#f0f9fa" stroke="#42c0c9" stroke-width="1.5" filter="url(#cardShadow)"/>
    <text x="420" y="488" text-anchor="middle" fill="#000000" font-size="11.5" font-weight="600">Safe defaults +</text>
    <text x="420" y="503" text-anchor="middle" fill="#000000" font-size="11.5" font-weight="600">LFC shrinkage</text>
  </g>
  <g class="fc-node fc-anim-6">
    <rect x="540" y="468" width="180" height="44" rx="8" fill="#f0f9fa" stroke="#42c0c9" stroke-width="1.5" filter="url(#cardShadow)"/>
    <text x="630" y="488" text-anchor="middle" fill="#000000" font-size="11.5" font-weight="600">Maximum control +</text>
    <text x="630" y="503" text-anchor="middle" fill="#000000" font-size="11.5" font-weight="600">QL calibration</text>
  </g>
  <g class="fc-node fc-anim-6">
    <rect x="740" y="468" width="140" height="44" rx="8" fill="#f0f9fa" stroke="#42c0c9" stroke-width="1.5" filter="url(#cardShadow)"/>
    <text x="810" y="488" text-anchor="middle" fill="#000000" font-size="11.5" font-weight="600">Already in limma</text>
    <text x="810" y="503" text-anchor="middle" fill="#000000" font-size="11.5" font-weight="600">ecosystem</text>
  </g>

  <!-- Row 6: Terminal results -->
  <g class="fc-node fc-anim-7 fc-terminal fc-terminal-deseq2" filter="url(#glowTeal)">
    <rect x="340" y="558" width="160" height="48" rx="24" fill="#ffffff" stroke="#42c0c9" stroke-width="2"/>
    <text x="420" y="587" text-anchor="middle" fill="#42c0c9" font-size="15" font-weight="700" font-family="'JetBrains Mono',monospace">DESeq2</text>
  </g>
  <g class="fc-node fc-anim-7 fc-terminal fc-terminal-edger" filter="url(#glowTeal)">
    <rect x="540" y="558" width="180" height="48" rx="24" fill="#ffffff" stroke="#42c0c9" stroke-width="2"/>
    <text x="630" y="587" text-anchor="middle" fill="#42c0c9" font-size="15" font-weight="700" font-family="'JetBrains Mono',monospace">edgeR QL</text>
  </g>
  <g class="fc-node fc-anim-7 fc-terminal fc-terminal-voom" filter="url(#glowTeal)">
    <rect x="740" y="558" width="140" height="48" rx="24" fill="#ffffff" stroke="#42c0c9" stroke-width="2"/>
    <text x="810" y="587" text-anchor="middle" fill="#42c0c9" font-size="15" font-weight="700" font-family="'JetBrains Mono',monospace">limma-voom</text>
  </g>

  <!-- Legend -->
  <g transform="translate(50, 630)" opacity="0.8">
    <circle cx="8" cy="8" r="5" fill="#f48c06" opacity="0.7"/>
    <text x="20" y="12" fill="#666666" font-size="10">Stop / Warning</text>
    <circle cx="128" cy="8" r="5" fill="#42c0c9" opacity="0.7"/>
    <text x="140" y="12" fill="#666666" font-size="10">DESeq2 / edgeR / limma-voom</text>
    <line x1="280" y1="8" x2="310" y2="8" stroke="#42c0c9" stroke-width="1.8" stroke-dasharray="6,6" opacity="0.6"/>
    <text x="318" y="12" fill="#666666" font-size="10">Decision flow</text>
  </g>
</svg>

<div class="fc-caption"><strong>Figure 2.</strong> Decision flowchart for choosing between DESeq2, edgeR, and limma-voom based on data type, sample size, and analysis priorities.</div>
      </div>

---

DESeq2 and edgeR are the two most widely used tools for differential expression analysis of RNA-seq count data. Both use **Negative Binomial generalized linear models**, and both are mature, well-validated, and actively maintained.

They often agree on strong signals, but they can diverge on borderline genes — especially with small sample sizes, strong composition effects, or outliers — because dispersion estimation and hypothesis testing differ. This guide covers both the statistical machinery and the **experimental design specification**, which is where most real-world errors originate.

<figure>
  <img src="{{ '/assets/images/deseq2_vs_edgeR.png' | relative_url }}" alt="DESeq2 vs edgeR comparison" />
  <figcaption><strong>Figure 1.</strong> Overview of DESeq2 and edgeR for RNA-seq differential expression analysis.</figcaption>
</figure>

---

## 1. The Shared Statistical Foundation


---


For each gene *g* in each sample *i*, DESeq2 and edgeR treat the read count as:

- a count value that varies across samples, and
- more variable than a Poisson model would allow (because biology adds extra noise).

So they both use a **negative binomial model**:

- **μ<sub>gi</sub>** (mu) = the expected/average count for that gene in that sample
- **α<sub>g</sub>** (alpha) = the extra variability for that gene (dispersion)

They then say the expected count is determined by two things:

1. **How deep that sample was sequenced** — captured by the size factor **s<sub>i</sub>** (normalization)
2. **What condition/batch/covariates the sample belongs to** — captured by the design matrix **x<sub>i</sub>** and the gene's coefficients **β<sub>g</sub>**

**β<sub>g</sub>** are basically the log fold changes for that gene under your model.

**Bottom line:** both packages use the same core idea:

$$\text{counts} \approx \text{(normalization for sample)} + \text{(effects of condition/batch/etc.)} + \text{(extra biological variability)}$$

And that's why the "DESeq2 vs edgeR" differences are mostly about implementation details, especially:

- how they estimate dispersion (α<sub>g</sub>),
- how they test for differences in β<sub>g</sub> (differential expression),
- and how they shrink/report fold changes and p-values.


---
## 2. Normalization

Both methods assume that most genes are not differentially expressed. Both can struggle under global transcriptome shifts (e.g., transcriptome-wide up-regulation) unless external controls are introduced.

| Feature | DESeq2 | edgeR |
|---------|--------|-------|
| **Normalization** | Median-of-ratios (size factors) | TMM (trimmed mean of M-values) |
| **Dispersion** | Trend + empirical Bayes shrinkage | Trend + shrinkage + quasi-likelihood layer |
| **Recommended test** | Wald (pairwise) / LRT (omnibus) | Quasi-likelihood F-test |
| **LFC shrinkage** | Central feature (apeglm / ashr) | Available, less central |
| **Filtering** | Automatic independent filtering | User-driven: filterByExpr() |
| **Outlier handling** | Cook's distance (needs adequate n) | Robust QL estimation |
| **UX philosophy** | Safe defaults, wrapped steps | Modular, explicit, flexible |

> **When assumptions break:** If you suspect global shifts, consider spike-in normalization, a set of empirically stable reference genes, or at minimum examine MA plots of normalized counts for asymmetry. These are escape hatches, not defaults.

In routine bulk RNA-seq, normalization is rarely the deciding factor between these packages. Design and dispersion estimation matter more.

---

## 3. Dispersion Estimation

Estimating per-gene dispersion from small sample sizes is inherently noisy. Both packages borrow information across genes (empirical Bayes shrinkage), but they do so differently.

**DESeq2** fits a mean–dispersion trend and shrinks each gene's estimate toward it. **edgeR's QL pipeline** does the same, but adds a **quasi-likelihood variance layer** on top — an additional gene-specific parameter that provides better control of false positive rates, particularly with small *n* or heterogeneous variance.

> **Key distinction:** edgeR's QL framework is often more conservative and better calibrated for Type I error control in challenging settings. DESeq2's dispersion estimation is stable and well-validated but does not include this additional QL layer.

---

## 4. Hypothesis Testing

Each package offers multiple testing procedures. Comparing results across packages without accounting for the test used is a common source of confusion.

### DESeq2

- **Wald test** (default): tests a single coefficient ≠ 0. Fast, standard for pairwise.
- **LRT**: full vs reduced model. For omnibus questions (any effect across ≥3 levels).

### edgeR

- **QL F-test** (recommended): robust error control, especially with small *n*.
- LRT options exist but QL is the standard recommendation.

> **Warning:** Comparing DESeq2 Wald test results to edgeR QL F-test results is comparing different inferential procedures — not "packages." Be explicit about which test you used in your methods.

---

## 5. Log Fold-Change Shrinkage

DESeq2 provides a polished workflow for shrinking noisy log2FC estimates toward zero, preventing low-count genes from producing extreme fold changes that dominate visualizations and rankings. edgeR can moderate estimates through its modeling framework, but LFC shrinkage is not as prominently integrated into the default workflow.

> **Common misconception:** Shrinkage primarily affects **effect size estimates, ranking, and visualization**. Your p-values come from the model fit; shrinkage is typically applied post hoc for interpretability, not for significance testing.

---

## 6. Filtering and Outliers

Genes with near-zero counts across samples cannot support reliable dispersion or fold-change estimation and inflate the multiple-testing burden. Filtering them is essential, not optional.

### DESeq2

**Independent filtering:** automatically selects a threshold to maximize discoveries at target FDR. **Cook's distance** flags outlier samples per gene — but only works reliably with adequate replicates. With very small *n*, treat outliers as a QC/design issue.

### edgeR

**User-driven pre-filtering:** `filterByExpr()` is preferred over hand-rolled CPM thresholds because it accounts for group structure. Outlier robustness comes from the QL framework.

---

## 7. Recommended Default Pipelines

These are the recommended defaults for a standard pairwise comparison.

<div style="overflow-x:auto; margin: 1.5em 0;">
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 820 520" style="max-width:820px; width:100%; height:auto; font-family: ui-monospace, 'Cascadia Code', 'SF Mono', 'Fira Code', Consolas, monospace;">
  <defs>
    <filter id="shadow" x="-4%" y="-4%" width="108%" height="112%">
      <feDropShadow dx="0" dy="2" stdDeviation="3" flood-color="#000" flood-opacity="0.1"/>
    </filter>
    <linearGradient id="dg" x1="0" y1="0" x2="0" y2="1">
      <stop offset="0%" stop-color="#f0f9fa"/><stop offset="100%" stop-color="#e0f2f3"/>
    </linearGradient>
    <linearGradient id="eg" x1="0" y1="0" x2="0" y2="1">
      <stop offset="0%" stop-color="#f0f9fa"/><stop offset="100%" stop-color="#e0f2f3"/>
    </linearGradient>
  </defs>
  <rect width="820" height="520" rx="12" fill="#ffffff" stroke="#e0e0e0" stroke-width="1"/>
  <text x="410" y="36" text-anchor="middle" fill="#000000" font-size="15" font-weight="700" letter-spacing="1">RECOMMENDED DEFAULT PIPELINES</text>
  <!-- DESeq2 column -->
  <rect x="20" y="56" width="380" height="444" rx="10" fill="url(#dg)" stroke="#42c0c9" stroke-width="2"/>
  <text x="210" y="84" text-anchor="middle" fill="#42c0c9" font-size="14" font-weight="700">DESeq2</text>
  <rect x="44" y="102" width="332" height="52" rx="6" fill="#ffffff" stroke="#42c0c9" stroke-width="1.5" filter="url(#shadow)"/>
  <text x="60" y="122" fill="#42c0c9" font-size="11" font-weight="700">1</text>
  <text x="78" y="122" fill="#000000" font-size="11.5" font-weight="600">DESeqDataSetFromMatrix()</text>
  <text x="78" y="140" fill="#666666" font-size="10">counts + coldata + design formula</text>
  <line x1="210" y1="154" x2="210" y2="170" stroke="#42c0c9" stroke-width="1.5" stroke-dasharray="4,3" opacity="0.5"/>
  <rect x="44" y="170" width="332" height="52" rx="6" fill="#ffffff" stroke="#42c0c9" stroke-width="1.5" filter="url(#shadow)"/>
  <text x="60" y="190" fill="#42c0c9" font-size="11" font-weight="700">2</text>
  <text x="78" y="190" fill="#000000" font-size="11.5" font-weight="600">DESeq()</text>
  <text x="78" y="208" fill="#666666" font-size="10">normalization + dispersion + GLM fit</text>
  <line x1="210" y1="222" x2="210" y2="238" stroke="#42c0c9" stroke-width="1.5" stroke-dasharray="4,3" opacity="0.5"/>
  <rect x="44" y="238" width="332" height="52" rx="6" fill="#ffffff" stroke="#42c0c9" stroke-width="1.5" filter="url(#shadow)"/>
  <text x="60" y="258" fill="#42c0c9" font-size="11" font-weight="700">3</text>
  <text x="78" y="258" fill="#000000" font-size="11.5" font-weight="600">results()</text>
  <text x="78" y="276" fill="#666666" font-size="10">Wald or LRT + independent filtering</text>
  <line x1="210" y1="290" x2="210" y2="306" stroke="#42c0c9" stroke-width="1.5" stroke-dasharray="4,3" opacity="0.5"/>
  <rect x="44" y="306" width="332" height="62" rx="6" fill="#ffffff" stroke="#42c0c9" stroke-width="1.5" filter="url(#shadow)"/>
  <text x="60" y="326" fill="#42c0c9" font-size="11" font-weight="700">4</text>
  <text x="78" y="326" fill="#000000" font-size="11.5" font-weight="600">lfcShrink(type="apeglm")</text>
  <text x="78" y="344" fill="#666666" font-size="10">shrink fold changes for ranking/plots</text>
  <text x="78" y="358" fill="#f48c06" font-size="9.5" font-style="italic">does not change p-values</text>
  <rect x="44" y="384" width="332" height="44" rx="6" fill="#f0f9fa" stroke="#42c0c9" stroke-width="1.5" stroke-dasharray="4,2"/>
  <text x="210" y="404" text-anchor="middle" fill="#42c0c9" font-size="10.5" font-weight="600">Steps 1–3 wrapped — safe defaults</text>
  <text x="210" y="418" text-anchor="middle" fill="#666666" font-size="9.5">Few decisions needed for standard analysis</text>
  <!-- edgeR column -->
  <rect x="420" y="56" width="380" height="444" rx="10" fill="url(#eg)" stroke="#42c0c9" stroke-width="2"/>
  <text x="610" y="84" text-anchor="middle" fill="#42c0c9" font-size="14" font-weight="700">edgeR (QL pipeline)</text>
  <rect x="444" y="102" width="332" height="52" rx="6" fill="#ffffff" stroke="#42c0c9" stroke-width="1.5" filter="url(#shadow)"/>
  <text x="460" y="122" fill="#42c0c9" font-size="11" font-weight="700">1</text>
  <text x="478" y="122" fill="#000000" font-size="11.5" font-weight="600">DGEList() + calcNormFactors()</text>
  <text x="478" y="140" fill="#666666" font-size="10">create object + TMM normalization</text>
  <line x1="610" y1="154" x2="610" y2="170" stroke="#42c0c9" stroke-width="1.5" stroke-dasharray="4,3" opacity="0.5"/>
  <rect x="444" y="170" width="332" height="52" rx="6" fill="#ffffff" stroke="#42c0c9" stroke-width="1.5" filter="url(#shadow)"/>
  <text x="460" y="190" fill="#42c0c9" font-size="11" font-weight="700">2</text>
  <text x="478" y="190" fill="#000000" font-size="11.5" font-weight="600">filterByExpr()</text>
  <text x="478" y="208" fill="#666666" font-size="10">group-aware low-count removal</text>
  <line x1="610" y1="222" x2="610" y2="238" stroke="#42c0c9" stroke-width="1.5" stroke-dasharray="4,3" opacity="0.5"/>
  <rect x="444" y="238" width="332" height="52" rx="6" fill="#ffffff" stroke="#42c0c9" stroke-width="1.5" filter="url(#shadow)"/>
  <text x="460" y="258" fill="#42c0c9" font-size="11" font-weight="700">3</text>
  <text x="478" y="258" fill="#000000" font-size="11.5" font-weight="600">estimateDisp()</text>
  <text x="478" y="276" fill="#666666" font-size="10">common + trended + gene-wise dispersion</text>
  <line x1="610" y1="290" x2="610" y2="306" stroke="#42c0c9" stroke-width="1.5" stroke-dasharray="4,3" opacity="0.5"/>
  <rect x="444" y="306" width="332" height="52" rx="6" fill="#ffffff" stroke="#42c0c9" stroke-width="1.5" filter="url(#shadow)"/>
  <text x="460" y="326" fill="#42c0c9" font-size="11" font-weight="700">4</text>
  <text x="478" y="326" fill="#000000" font-size="11.5" font-weight="600">glmQLFit()</text>
  <text x="478" y="344" fill="#666666" font-size="10">QL GLM — extra variance layer for calibration</text>
  <line x1="610" y1="358" x2="610" y2="374" stroke="#42c0c9" stroke-width="1.5" stroke-dasharray="4,3" opacity="0.5"/>
  <rect x="444" y="374" width="332" height="52" rx="6" fill="#ffffff" stroke="#42c0c9" stroke-width="1.5" filter="url(#shadow)"/>
  <text x="460" y="394" fill="#42c0c9" font-size="11" font-weight="700">5</text>
  <text x="478" y="394" fill="#000000" font-size="11.5" font-weight="600">glmQLFTest()</text>
  <text x="478" y="412" fill="#666666" font-size="10">QL F-test on coef or contrast</text>
  <rect x="444" y="440" width="332" height="44" rx="6" fill="#f0f9fa" stroke="#42c0c9" stroke-width="1.5" stroke-dasharray="4,2"/>
  <text x="610" y="460" text-anchor="middle" fill="#42c0c9" font-size="10.5" font-weight="600">Each step explicit — maximum control</text>
  <text x="610" y="474" text-anchor="middle" fill="#666666" font-size="9.5">More decisions, more flexibility</text>
</svg>
    </div>

---

## 8. Experimental Design Specification

This is where most real-world errors originate — not in the choice of package, but in the choice of model formula.

### Unpaired (independent samples)

Samples in each group are independent biological replicates. No pairing, blocking, or additional covariates.

**Example:** 3 control vs 3 treated mice, all different animals.

**Design formula:** `~ condition`

**DESeq2:**
```r
dds <- DESeqDataSetFromMatrix(
  countData = counts,
  colData   = coldata,
  design    = ~ condition)
dds <- DESeq(dds)
res <- results(dds, contrast = c("condition","treated","control"))
res <- lfcShrink(dds, coef = "condition_treated_vs_control",
                 type = "apeglm")
```

**edgeR:**
```r
design <- model.matrix(~ condition, data = coldata)
y <- DGEList(counts = counts)
y <- calcNormFactors(y)
keep <- filterByExpr(y, design = design)
y <- y[keep, , keep.lib.sizes = FALSE]
y <- estimateDisp(y, design)
fit <- glmQLFit(y, design)
qlf <- glmQLFTest(fit, coef = "conditiontreated")
```

### Paired design

The same subject is measured under two conditions. Including the subject as a blocking factor absorbs inter-subject variability, so the condition test is based on within-subject differences.

**Example:** 4 patients, each contributing a tumor and a normal sample.

**Design formula:** `~ subject + condition`

**DESeq2:**
```r
dds <- DESeqDataSetFromMatrix(
  countData = counts,
  colData   = coldata,
  design    = ~ patient + condition)
dds <- DESeq(dds)
res <- results(dds, contrast = c("condition","tumor","normal"))
res <- lfcShrink(dds, coef = "condition_tumor_vs_normal",
                 type = "apeglm")
```

**edgeR:**
```r
design <- model.matrix(~ patient + condition, data = coldata)
y <- DGEList(counts = counts)
y <- calcNormFactors(y)
keep <- filterByExpr(y, design = design)
y <- y[keep, , keep.lib.sizes = FALSE]
y <- estimateDisp(y, design)
fit <- glmQLFit(y, design)
qlf <- glmQLFTest(fit, coef = "conditiontumor")
```

> **Note:** Without the pairing term, inter-patient variability inflates the error estimate and reduces power. In strongly paired data, this can be the difference between finding 50 vs 500 DE genes.

### Batch correction (blocking)

Batch effects are systematic technical variation. If batch is not confounded with condition, include it as a blocking factor.

**Example:** Samples processed in 2 batches, with conditions represented in both.

**Design formula:** `~ batch + condition`

**DESeq2:**
```r
dds <- DESeqDataSetFromMatrix(
  countData = counts,
  colData   = coldata,
  design    = ~ batch + condition)
dds <- DESeq(dds)
res <- results(dds, contrast = c("condition","treat","ctrl"))
```

**edgeR:**
```r
design <- model.matrix(~ batch + condition, data = coldata)
y <- DGEList(counts = counts)
y <- calcNormFactors(y)
keep <- filterByExpr(y, design = design)
y <- y[keep, , keep.lib.sizes = FALSE]
y <- estimateDisp(y, design)
fit <- glmQLFit(y, design)
qlf <- glmQLFTest(fit, coef = "conditiontreat")
```

> **Note:** If batch and condition are perfectly confounded (all controls in batch 1, all treated in batch 2), no method can separate the effects. This is a design failure.

### Interaction (treatment × genotype)

Tests whether the treatment effect depends on genotype. The interaction term captures this difference.

**Example:** 2 genotypes (WT, KO) × 2 treatments (ctrl, treat), with replicates.

**Design formula:** `~ genotype * treatment`

> **Interpreting interactions:** `treatment` coefficient = treatment effect in the **reference genotype** (WT). `interaction` = **difference** in treatment effect between KO and WT. Treatment effect in KO = `treatment + interaction`. A significant interaction does not mean "no effect" — it means the effect *differs* between genotypes.

**DESeq2:**
```r
dds <- DESeqDataSetFromMatrix(
  countData = counts,
  colData   = coldata,
  design    = ~ genotype * treatment)
dds <- DESeq(dds)

# Interaction: does treatment effect differ by genotype?
res_int <- results(dds, name = "genotypeKO.treatmenttreat")

# Treatment effect in WT (reference genotype):
res_wt  <- results(dds, contrast = c("treatment","treat","ctrl"))
```

**edgeR:**
```r
design <- model.matrix(~ genotype * treatment, data = coldata)
y <- DGEList(counts = counts)
y <- calcNormFactors(y)
keep <- filterByExpr(y, design = design)
y <- y[keep, , keep.lib.sizes = FALSE]
y <- estimateDisp(y, design)
fit <- glmQLFit(y, design)

# Interaction term:
qlf_int <- glmQLFTest(fit, coef = "genotypeKO:treatmenttreat")

# Treatment effect in WT:
qlf_wt  <- glmQLFTest(fit, coef = "treatmenttreat")

# Treatment effect in KO (treatment + interaction):
con <- makeContrasts(
  treatmenttreat + genotypeKO:treatmenttreat,
  levels = design)
qlf_ko  <- glmQLFTest(fit, contrast = con)
```

### Time-course (omnibus test)

With ≥3 time points, test whether any change occurs across time. This is an omnibus test — it identifies genes with any temporal pattern, not the direction.

**Example:** Samples at 0h, 6h, 12h, 24h with 3 replicates each.

**Design formula:** `~ time` (tested via LRT / omnibus QL)

**DESeq2:**
```r
dds <- DESeqDataSetFromMatrix(
  countData = counts,
  colData   = coldata,
  design    = ~ time)

# LRT: full model vs intercept-only (any time effect?)
dds <- DESeq(dds, test = "LRT", reduced = ~ 1)
res <- results(dds)
```

**edgeR:**
```r
design <- model.matrix(~ time, data = coldata)
y <- DGEList(counts = counts)
y <- calcNormFactors(y)
keep <- filterByExpr(y, design = design)
y <- y[keep, , keep.lib.sizes = FALSE]
y <- estimateDisp(y, design)
fit <- glmQLFit(y, design)

# Test all time coefficients simultaneously (ANOVA-like)
qlf <- glmQLFTest(fit, coef = 2:ncol(design))
```

> **Note:** Omnibus significance tells you some difference exists across time — not which time points differ or in which direction. Follow up with planned contrasts or clustering to interpret temporal patterns.

### Continuous covariates (age, RIN)

Continuous variables can be included directly in the formula to adjust for technical or biological confounders.

**Example:** Adjusting for age and RNA integrity (RIN) while testing treatment.

**Design formula:** `~ age + RIN + condition`

**DESeq2:**
```r
dds <- DESeqDataSetFromMatrix(
  countData = counts,
  colData   = coldata,
  design    = ~ age + RIN + condition)
dds <- DESeq(dds)
res <- results(dds, contrast = c("condition","treat","ctrl"))
```

**edgeR:**
```r
design <- model.matrix(~ age + RIN + condition, data = coldata)
y <- DGEList(counts = counts)
y <- calcNormFactors(y)
keep <- filterByExpr(y, design = design)
y <- y[keep, , keep.lib.sizes = FALSE]
y <- estimateDisp(y, design)
fit <- glmQLFit(y, design)
qlf <- glmQLFTest(fit, coef = "conditiontreat")
```

> **On covariates and formula order:** Include covariates to reduce confounding and improve precision. In GLMs, coefficient estimates do not depend on term order (unlike Type I sums of squares in ANOVA). What matters is **reference levels and contrasts** — be explicit about them, as those choices affect interpretation. Only include covariates that are biologically or technically justified; irrelevant covariates waste degrees of freedom.

---

## 9. QC and Diagnostics

No results section is complete without these checks.

### Pre-alignment

- **Library sizes across samples** — Flag outlier libraries. Large imbalances may indicate failed preps.
- **Mapping rates & duplication** — Low mapping or high duplication can indicate degraded RNA or contamination.

### Post-normalization

- **PCA / sample distance heatmap** — Check for batch effects, outliers, and whether conditions separate. The single most informative plot.
- **MA plot of normalized counts** — Should show balanced fold changes around zero. Asymmetry suggests normalization failure.

### Model diagnostics

- **Dispersion plot (mean vs dispersion)** — Gene-wise estimates should scatter around the fitted trend. Wild outliers or flat trends indicate problems.
- **Check for confounded batch + condition** — Inspect the design matrix. If any factor is perfectly correlated with the variable of interest, the model is unidentifiable.

### Results-level

- **P-value histogram** — Should be uniform with a spike near 0 (true positives). U-shapes or spikes at 1 indicate model misspecification.
- **Volcano / MA plot of results** — Sanity-check effect sizes and significance patterns. Inspect the top hits manually against raw counts.

---

## 10. Common Failure Modes

| Issue | Problem |
|-------|---------|
| **n = 1 per group** | Cannot estimate biological variability. DE is not possible. |
| **Batch ≡ condition** | Perfectly confounded. No method can separate the effects. |
| **Unmodeled pairing** | Treating paired samples as independent inflates error and kills power. |
| **Overfitting covariates** | Adding irrelevant covariates wastes degrees of freedom with small n. |
| **No pre-filtering** | Inflates multiple-testing burden and distorts FDR. |
| **Intersecting two tools** | Not validation — both use NB GLMs. Reduces sensitivity for false comfort. |

---

## Rules of Thumb

1. **If unsure and doing standard bulk RNA-seq, DESeq2 is the lowest-risk default.**
2. **If sample sizes are small or you need conservative inference, edgeR + QL is a strong choice.**
3. **Do not intersect results from both packages and call it validation. Use diagnostics and validate the biology.**
4. **Spend more time on design, contrasts, and QC than on package selection. The formula you write matters more than the function you call.**

---

## Test Yourself: DESeq2 vs edgeR Quiz

Think you've got it? Click an answer to check.

<div id="de-quiz"></div>

<style>
#de-quiz{font-family:-apple-system,BlinkMacSystemFont,'Segoe UI',sans-serif;max-width:100%;margin:1.5em 0}
.qz-card{background:#fff5eb;border:1px solid rgba(232,93,4,0.2);border-radius:10px;padding:20px;margin-bottom:16px;box-shadow:0 2px 8px rgba(0,0,0,0.06)}
.qz-q{font-size:15px;font-weight:600;color:#1a1a1a;margin-bottom:14px;line-height:1.5}
.qz-n{display:inline-block;background:rgba(232,93,4,0.15);color:#e85d04;font-size:11px;font-weight:700;padding:2px 8px;border-radius:10px;margin-right:8px;font-family:ui-monospace,monospace}
.qz-opts{display:flex;flex-direction:column;gap:8px}
.qz-o{background:#ffffff;border:1px solid rgba(232,93,4,0.2);border-radius:8px;padding:12px 16px;color:#1a1a1a;font-size:14px;cursor:pointer;transition:all .15s;line-height:1.45;text-align:left}
.qz-o:hover:not(.qz-lk){border-color:#e85d04;background:#fff5eb}
.qz-o.qz-ok{border-color:#2d6a4f;background:#e8f5e9;color:#2d6a4f}
.qz-o.qz-no{border-color:#e85d04;background:#fff5eb;color:#e85d04}
.qz-o.qz-lk{cursor:default;opacity:.55}
.qz-o.qz-ok.qz-lk,.qz-o.qz-no.qz-lk{opacity:1}
.qz-ex{margin-top:12px;padding:12px 16px;border-radius:8px;font-size:13px;line-height:1.6;display:none}
.qz-ex.qz-vis{display:block}
.qz-ex-ok{background:#e8f5e9;border:1px solid #2d6a4f;color:#1b5e20}
.qz-ex-no{background:#fff5eb;border:1px solid #e85d04;color:#b71c1c}
.qz-bar{background:#fff5eb;border:1px solid rgba(232,93,4,0.2);border-radius:10px;padding:16px 20px;margin-top:8px;display:flex;align-items:center;gap:16px}
.qz-fill{flex:1;height:8px;background:rgba(232,93,4,0.15);border-radius:4px;overflow:hidden}
.qz-fill-in{height:100%;border-radius:4px;transition:width .4s ease;background:#e85d04}
.qz-sc{font-family:ui-monospace,monospace;font-size:13px;color:#e85d04;font-weight:700;white-space:nowrap}
.qz-rst{background:rgba(232,93,4,0.15);border:1px solid #e85d04;border-radius:6px;color:#e85d04;font-size:12px;font-family:ui-monospace,monospace;padding:6px 14px;cursor:pointer}
.qz-rst:hover{background:rgba(232,93,4,0.25)}
</style>

<script>
(function(){
var Q=[
{q:"Both DESeq2 and edgeR use which underlying distribution to model RNA-seq counts?",
o:["Poisson","Negative Binomial","Normal (after transformation)","Zero-inflated Poisson"],c:1,
e:"Both packages model counts with a Negative Binomial distribution, which adds a dispersion parameter to handle biological variability beyond what Poisson allows."},
{q:"You have tumor and matched normal tissue from the same 5 patients. What design formula should you use?",
o:["~ condition","~ patient + condition","~ patient * condition","~ condition + patient:condition"],c:1,
e:"~ patient + condition accounts for the pairing. The patient term absorbs inter-individual variability so the condition test reflects within-patient differences."},
{q:"What does edgeR's quasi-likelihood (QL) layer provide that standard NB dispersion alone does not?",
o:["Better normalization","An additional gene-specific variance parameter for improved Type I error control","Automatic LFC shrinkage","Built-in batch correction"],c:1,
e:"The QL framework adds a gene-specific variance parameter on top of the NB dispersion, providing better calibration of false positive rates — especially with small sample sizes."},
{q:"LFC shrinkage in DESeq2 changes which of the following?",
o:["p-values and adjusted p-values","Effect size estimates, ranking, and visualization","The underlying dispersion estimates","The normalization size factors"],c:1,
e:"Shrinkage is applied post hoc to fold-change estimates for interpretability. It affects ranking and visualization but does not change p-values."},
{q:"You ran DESeq2 with a Wald test and edgeR with a QL F-test and got different results. The most likely explanation is:",
o:["One package has a bug","You are comparing different inferential procedures, not just packages","The normalization methods are incompatible","Your count matrix is malformed"],c:1,
e:"Wald tests and QL F-tests are fundamentally different hypothesis tests. Divergent results reflect the testing procedure, not a flaw in either package."},
{q:"All your control samples were processed in batch 1 and all treated in batch 2. What should you do?",
o:["Include batch in the formula: ~ batch + condition","Use ComBat to remove the batch effect first","Acknowledge this is a confounded design — no analysis can separate batch from condition","Switch to edgeR because it handles batches better"],c:2,
e:"When batch and condition are perfectly confounded, no statistical method can separate them. This is a fundamental design failure that must be addressed experimentally."},
{q:"Why is filterByExpr() preferred over a hand-rolled CPM threshold in edgeR?",
o:["It is faster computationally","It accounts for group structure when deciding which genes to keep","It also normalizes the data","It removes batch effects"],c:1,
e:"filterByExpr() uses the experimental design (group sizes) to set appropriate thresholds, rather than applying a single arbitrary cutoff."},
{q:"You run a time-course experiment (0h, 6h, 12h, 24h) and use DESeq2's LRT with reduced = ~ 1. A gene has padj < 0.01. You can conclude:",
o:["The gene is upregulated at 24h","The gene shows some change across time — direction and timing require follow-up","The gene is differentially expressed at every time point","The gene has a linear trend over time"],c:1,
e:"The LRT omnibus test tells you there is some temporal pattern, but not which time points differ or in which direction. Follow up with planned contrasts."},
{q:"In an R model formula, does the order of terms (e.g., ~ age + condition vs ~ condition + age) change the coefficient estimates in a GLM?",
o:["Yes — covariates must come before the variable of interest","No — GLM coefficient estimates are the same regardless of term order","Only in DESeq2, not in edgeR","Only when using the Wald test"],c:1,
e:"In GLMs, the model fit does not depend on term order (unlike Type I sums of squares). What matters is reference levels and contrast specification."},
{q:"Taking the intersection of DESeq2 and edgeR results is considered problematic because:",
o:["The packages use completely different statistical models","Both use NB GLMs, so agreement is expected — the overlap is not independent validation","It inflates false positives","It is computationally expensive"],c:1,
e:"Both share the same modeling framework (NB GLMs), so agreement does not constitute independent validation. The intersection reduces sensitivity without improving specificity."}
];
var ans={},sc=0;
function r(){
var el=document.getElementById('de-quiz');if(!el)return;
var h='';
Q.forEach(function(q,i){
var done=ans[i]!==undefined,ok=ans[i]===q.c;
h+='<div class="qz-card"><div class="qz-q"><span class="qz-n">'+(i+1)+'/10</span>'+q.q+'</div><div class="qz-opts">';
q.o.forEach(function(o,j){
var cls='qz-o';
if(done){cls+=' qz-lk';if(j===q.c)cls+=' qz-ok';else if(j===ans[i]&&j!==q.c)cls+=' qz-no';}
h+='<div class="'+cls+'" data-q="'+i+'" data-o="'+j+'">'+o+'</div>';
});
h+='</div>';
if(done){
var ec=ok?'qz-ex qz-vis qz-ex-ok':'qz-ex qz-vis qz-ex-no';
h+='<div class="'+ec+'">'+(ok?'✓ Correct. ':'✗ Not quite. ')+q.e+'</div>';
}
h+='</div>';
});
var t=Object.keys(ans).length,p=t>0?Math.round(sc/t*100):0;
h+='<div class="qz-bar"><div class="qz-sc">'+sc+'/'+t+(t>0?' ('+p+'%)':'')+'</div>';
h+='<div class="qz-fill"><div class="qz-fill-in" style="width:'+(t>0?sc/Q.length*100:0)+'%"></div></div>';
if(t>0)h+='<button class="qz-rst" onclick="window._qzR()">Reset</button>';
h+='</div>';
el.innerHTML=h;
el.querySelectorAll('.qz-o:not(.qz-lk)').forEach(function(b){
b.addEventListener('click',function(){
var qi=+this.getAttribute('data-q'),oi=+this.getAttribute('data-o');
if(ans[qi]!==undefined)return;
ans[qi]=oi;if(oi===Q[qi].c)sc++;r();
});
});
}
window._qzR=function(){ans={};sc=0;r();};
if(document.readyState==='loading')document.addEventListener('DOMContentLoaded',r);else r();
})();
</script>

---

## References

1. Guo, L. "DESeq2, limma & edgeR for RNA-seq Differential Expression: Which to Use?" [YouTube](https://www.youtube.com/watch?v=Y1FwCDuvSfM){:target="_blank" rel="noopener noreferrer"}.

2. Starmer, J. "StatQuest: DESeq2, part 1, Library Normalization." StatQuest with Josh Starmer. [YouTube](https://www.youtube.com/watch?v=UFB993xufUU){:target="_blank" rel="noopener noreferrer"}.

3. Starmer, J. "StatQuest: edgeR and DESeq2, part 2 - Independent Filtering." StatQuest with Josh Starmer. [YouTube](https://www.youtube.com/watch?v=Gi0JdrxRGy8){:target="_blank" rel="noopener noreferrer"}.

4. Starmer, J. "StatQuest: edgeR, part 1, Library Normalization." StatQuest with Josh Starmer. [YouTube](https://www.youtube.com/watch?v=UFB993xufUU){:target="_blank" rel="noopener noreferrer"}.

---

*Covers bulk RNA-seq and pseudo-bulk DE as of current best practices. Consult DESeq2 and edgeR vignettes for the latest recommendations.*