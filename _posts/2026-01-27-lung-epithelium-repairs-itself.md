---
layout: post
title: "How the Lung Epithelium Repairs Itself: AT2 Cells, Wnt Niches, and Timing"
date: 2026-01-27
description: "A reference-heavy tour of AT2 facultative stem cells and how Wnt signaling supports alveolar repair—until it doesn’t."
tags: [lung biology, regeneration, AT2, Wnt signaling, scRNA-seq, fibrosis]
categories: [lung biology]
giscus_comments: true
---

When the alveolar surface is damaged (infection, toxins, mechanical injury), the lung has to rebuild an ultra-thin gas-exchange barrier without losing integrity. A lot of that regenerative "heavy lifting" falls on **alveolar type 2 (AT2) cells**—surfactant-producing epithelial cells that can act as *facultative* stem/progenitor cells and replenish **alveolar type 1 (AT1) cells** during repair. <a href="https://pmc.ncbi.nlm.nih.gov/articles/PMC11027191/" target="_blank" rel="noopener noreferrer">[1]</a>

<figure>
<div class="mermaid">
flowchart TD
    subgraph Homeostasis["Homeostasis"]
        AT2_H[AT2 Cell<br/>Facultative Stem Cell]
        AT1_H[AT1 Cell<br/>Gas Exchange]
        AT2_H -->|Self-Renewal| AT2_H
        AT2_H -->|Differentiation| AT1_H
    end
    
    subgraph Injury["After Injury"]
        Damage[Epithelial Damage]
        AT2_I1[AT2 Cell]
        AT2_I2[AT2 Cell]
        AT2_I3[AT2 Cell]
        AT1_I[AT1 Cell]
        
        Damage -->|Triggers| AT2_I1
        AT2_I1 -->|Proliferation| AT2_I2
        AT2_I1 -->|Proliferation| AT2_I3
        AT2_I2 -->|Differentiation| AT1_I
        AT2_I3 -->|Differentiation| AT1_I
    end
    
    style AT2_H fill:#90EE90
    style AT2_I1 fill:#90EE90
    style AT2_I2 fill:#90EE90
    style AT2_I3 fill:#90EE90
    style AT1_H fill:#DDA0DD
    style AT1_I fill:#DDA0DD
    style Damage fill:#FFB6C1
</div>
<figcaption><strong>Figure 1.</strong> AT2 cells act as facultative stem cells: during homeostasis, they maintain themselves through self-renewal and slowly differentiate into AT1 cells; after injury, many more AT2s are recruited into proliferation and differentiation programs to restore the epithelial barrier.</figcaption>
</figure>

---

## AT2 cells as facultative stem cells

AT2 cells are often described as the “stem cells” of the alveolus, but the nuance matters:

- At steady state, **only a subset** of AT2 cells is actively cycling.
- After injury, **many more** AT2 cells can be recruited into proliferation and differentiation programs to restore the epithelial barrier. <a href="https://pmc.ncbi.nlm.nih.gov/articles/PMC11027191/" target="_blank" rel="noopener noreferrer">[1]</a>

This “reserve capacity” is what *facultative* stem cells are all about: mostly quiet during homeostasis, but capable of stepping up when tissue demands it.

---

## Two modes of repair: slow renewal vs. acute regeneration

Two patterns show up repeatedly across lineage tracing + single-cell studies:

- **Ageing / steady-state renewal**: rare **Wnt-responsive (often Axin2⁺)** AT2 cells contribute to slow, clonal replacement that can look patchy over time. <a href="https://pmc.ncbi.nlm.nih.gov/articles/PMC5997265/" target="_blank" rel="noopener noreferrer">[2]</a>
- **Acute injury**: many previously "bulk" AT2 cells can transiently turn on a Wnt/Axin2-like program, proliferate, and feed into differentiation trajectories needed for repair. <a href="https://pmc.ncbi.nlm.nih.gov/articles/PMC9068227/" target="_blank" rel="noopener noreferrer">[3]</a>

This helps reconcile why you can see both a *rare stem-like subset* and *broad plasticity* depending on the context and timescale.

---

## Wnt/β-catenin: keeping AT2s “progenitor-like” (and why timing matters)

One way to summarize canonical Wnt/β-catenin in the alveolus is:

- **Wnt high** → AT2 identity/progenitor programs are supported.
- **Wnt down** → AT2-to-AT1 differentiation can proceed efficiently.

This shows up as a recurring theme:

- Canonical Wnt/β-catenin supports AT2 progenitor identity/self-renewal. <a href="https://www.nature.com/articles/s41420-019-0147-9" target="_blank" rel="noopener noreferrer">[4]</a>
- Sustained Wnt can interfere with full AT1 maturation; downregulation is often required for successful differentiation. <a href="https://www.nature.com/articles/s42003-021-02118-w" target="_blank" rel="noopener noreferrer">[5]</a>

So Wnt is not just a "proliferation on/off switch"—it's a **cell state regulator** that can be beneficial early (expanding/maintaining a progenitor pool) and harmful later (stalling differentiation).

---

## Wnt is not a simple proliferation switch

Common intuition suggests Wnt directly drives proliferation. In alveolar repair, the reality is more nuanced:

**Wnt confers progenitor identity** — Wnt activity keeps AT2 cells in a stem-like state that is competent to generate AT1 cells and to respond to growth cues. <a href="https://www.nature.com/articles/s41420-019-0147-9" target="_blank" rel="noopener noreferrer">[4]</a>

**Growth-factor synergy** — EGF-family signals (via EGFR) are potent drivers of AT2 proliferation. Wnt signaling potentiates or primes AT2s to respond strongly to EGFR/EGF cues. The combined action dramatically increases proliferation compared with either signal alone. <a href="https://desailab.stanford.edu/sites/g/files/sbiybj24296/files/media/file/sc1.pdf" target="_blank" rel="noopener noreferrer">[11]</a>

**Timing matters** — sustained, high Wnt activity prevents AT2-to-AT1 differentiation, while timely downregulation of Wnt is required for successful epithelial maturation. <a href="https://www.nature.com/articles/s42003-021-02118-w" target="_blank" rel="noopener noreferrer">[5]</a>

This means Wnt doesn't directly cause cells to divide; instead, it maintains a progenitor state that enables robust proliferative responses when growth factors like EGF are present.

---

## Where the Wnt ligands come from: niches and (sometimes) autocrine repair

The alveolus is a classic example of **short-range niche signaling**:

- In steady state, **Pdgfrα⁺ alveolar fibroblast populations** can provide Wnt ligands that support nearby AT2 cells. <a href="https://www.science.org/doi/10.1126/science.aam6603" target="_blank" rel="noopener noreferrer">[6]</a>
- After injury, AT2 cells can also become an **autocrine** source of Wnt ligands (including Wnt7b and others), effectively expanding the Wnt "field" during regeneration. <a href="https://desailab.stanford.edu/sites/g/files/sbiybj24296/files/media/file/sc2.pdf" target="_blank" rel="noopener noreferrer">[7]</a>

<figure>
<div class="mermaid">
flowchart TD
    subgraph SS["Steady State: Wnt Niche"]
        Fib1[Pdgfrα+ Fibroblast]
        Wnt1[Wnt5a / Wnt Ligands]
        FZD1[FZD Receptor]
        AT2_SS[AT2 Cell<br/>β-catenin in nucleus<br/>Progenitor state maintained]
        
        Fib1 -->|Secretes| Wnt1
        Wnt1 -->|Binds| FZD1
        FZD1 -->|Activates| AT2_SS
    end
    
    subgraph AI["After Injury: Expanded Wnt Signaling"]
        Fib2[Pdgfrα+ Fibroblast]
        Wnt_P2[Paracrine Wnt<br/>Wnt5a]
        Wnt_A2[Autocrine Wnt<br/>Wnt7b / Wnt ligands]
        AT2_AI1[AT2 Cell]
        AT2_AI2[AT2 Cell<br/>Increased β-catenin<br/>Proliferation]
        
        Fib2 -->|Paracrine| Wnt_P2
        Wnt_P2 --> AT2_AI1
        AT2_AI1 -->|Produces| Wnt_A2
        Wnt_A2 -->|Autocrine| AT2_AI2
        AT2_AI2 -.->|Self-signaling| AT2_AI2
    end
    
    style Fib1 fill:#FFA500
    style Fib2 fill:#FFA500
    style AT2_SS fill:#90EE90
    style AT2_AI1 fill:#90EE90
    style AT2_AI2 fill:#90EE90
    style Wnt1 fill:#FFD700
    style Wnt_P2 fill:#FFD700
    style Wnt_A2 fill:#FFD700
</div>
<figcaption><strong>Figure 2.</strong> A short-range Wnt niche model: in steady state, Pdgfrα+ fibroblasts secrete Wnt5a that maintains Wnt-responsive AT2s via FZD receptors; after injury, AT2 cells themselves produce Wnt ligands (including Wnt7b), creating autocrine signaling that expands the Wnt field and recruits more AT2s into repair programs.</figcaption>
</figure>

---

## How researchers measure “Wnt activity” in AT2 cells

In practice, Wnt activity is inferred using a combination of:

- **Reporter/target genes** (e.g., *Axin2* as a canonical Wnt target). <a href="https://pmc.ncbi.nlm.nih.gov/articles/PMC5997265/" target="_blank" rel="noopener noreferrer">[2]</a>
- **Single-cell and spatial transcriptomics** to map which cell types produce Wnt ligands and which are responding. <a href="https://desailab.stanford.edu/sites/g/files/sbiybj24296/files/media/file/sc2.pdf" target="_blank" rel="noopener noreferrer">[7]</a>
- **Perturbations** (e.g., blocking Wnt secretion machinery, deleting β-catenin, or forcing β-catenin activation) to test necessity vs sufficiency. <a href="https://www.nature.com/articles/s41420-019-0147-9" target="_blank" rel="noopener noreferrer">[4]</a>

---

## When repair goes wrong: stalled progenitors and fibrosis-linked signaling

Disordered repair in fibrotic disease is often framed as “failed regeneration”:

- Fibrotic lungs can show **hyperplastic epithelial populations** that don't successfully reconstitute normal AT1 structure/function. <a href="https://www.jci.org/articles/view/170504" target="_blank" rel="noopener noreferrer">[8]</a>
- Multiple studies report **dysregulated Wnt signaling** across epithelial and stromal compartments, including WNT5A-associated profibrotic behaviors. <a href="https://www.atsjournals.org/doi/10.1164/rccm.201708-1580OC" target="_blank" rel="noopener noreferrer">[9]</a>
- Crosstalk among Wnt, TGFβ, and altered niche interactions is frequently implicated in shifting outcomes from regeneration toward scarring. <a href="https://www.biorxiv.org/content/10.1101/2023.08.02.551383v1.full.pdf" target="_blank" rel="noopener noreferrer">[10]</a>

This is why therapeutic "Wnt modulation" is tricky: Wnt is deeply regenerative in many tissues, so **cell-type specificity and timing** are the whole game. <a href="https://www.nature.com/articles/s41420-019-0147-9" target="_blank" rel="noopener noreferrer">[4]</a>

---

## A practical checklist (if you’re designing experiments)

- **Map Wnt-responsive AT2s** (e.g., *Axin2* target programs) across baseline vs injury timepoints. <a href="https://pmc.ncbi.nlm.nih.gov/articles/PMC5997265/" target="_blank" rel="noopener noreferrer">[2]</a>
- **Identify the niche** (which fibroblasts are adjacent? which ligands/receptors are enriched?) with single-cell + spatial methods. <a href="https://www.science.org/doi/10.1126/science.aam6603" target="_blank" rel="noopener noreferrer">[6]</a>
- **Measure both halves of repair**:
  - AT2 proliferation/expansion
  - successful AT2→AT1 differentiation/maturation <a href="https://www.nature.com/articles/s42003-021-02118-w" target="_blank" rel="noopener noreferrer">[5]</a>

If you're interested in the broader "context and timing" story, this connects closely with my earlier post: [**The Wnt Paradox: How the Same Pathway Can Heal and Harm the Lung**]({% post_url 2026-01-08-WNT-activation-heals-lung %}).

---

## Key experimental insights

Several key findings have shaped current understanding:

- **Lineage tracing** shows a small fraction of AT2 cells fuel slow, clonal expansion in ageing lungs. <a href="https://pmc.ncbi.nlm.nih.gov/articles/PMC5997265/" target="_blank" rel="noopener noreferrer">[2]</a>
- **Single-cell analyses** reveal fibroblast subsets that express Wnt5a localized next to AT2s in steady state. <a href="https://www.science.org/doi/10.1126/science.aam6603" target="_blank" rel="noopener noreferrer">[6]</a>
- **Acute epithelial ablation or injury** triggers widespread AT2 proliferation and induction of autocrine Wnt ligand expression (including Wnt7b) by AT2s themselves. <a href="https://pmc.ncbi.nlm.nih.gov/articles/PMC9068227/" target="_blank" rel="noopener noreferrer">[3]</a>, <a href="https://desailab.stanford.edu/sites/g/files/sbiybj24296/files/media/file/sc2.pdf" target="_blank" rel="noopener noreferrer">[7]</a>
- **Blocking Wnt secretion** or Wnt response reduces AT2 proliferation and delays repair. Conversely, preventing the normal downregulation of Wnt blocks AT2-to-AT1 differentiation. <a href="https://www.nature.com/articles/s41420-019-0147-9" target="_blank" rel="noopener noreferrer">[4]</a>, <a href="https://www.nature.com/articles/s42003-021-02118-w" target="_blank" rel="noopener noreferrer">[5]</a>

---

## Common misconceptions and pitfalls

**Misconception: All AT2 cells are dedicated stem cells.**  
**Correction:** Most AT2 cells perform surfactant and homeostatic roles; only a subset act as stem cells in steady state, and many can become facultative stem cells after injury. <a href="https://pmc.ncbi.nlm.nih.gov/articles/PMC11027191/" target="_blank" rel="noopener noreferrer">[1]</a>

**Misconception: Wnt directly causes proliferation.**  
**Correction:** Wnt maintains progenitor identity and enables responsiveness to proliferative factors such as EGF but is not the sole mitogenic trigger. EGFR/EGF signals drive proliferation; Wnt primes AT2s to respond robustly. <a href="https://desailab.stanford.edu/sites/g/files/sbiybj24296/files/media/file/sc1.pdf" target="_blank" rel="noopener noreferrer">[11]</a>

**Pitfall: Targeting Wnt without cell specificity risks worsening outcomes.**  
Therapies must distinguish between necessary transient Wnt activity for repair and chronic aberrant activation that may drive pathology. <a href="https://www.nature.com/articles/s41420-019-0147-9" target="_blank" rel="noopener noreferrer">[4]</a>, <a href="https://www.atsjournals.org/doi/10.1164/rccm.201708-1580OC" target="_blank" rel="noopener noreferrer">[9]</a>

---

## Summary: core takeaways

- **Alveolar repair uses two modes:** rare, fibroblast-supported AT2 stem cells in steady state and widespread, autocrine-activated AT2 proliferation after acute injury.
- **Wnt signaling sets stemness and fate:** it maintains progenitor competence and must be downregulated for AT2-to-AT1 differentiation.
- **Wnt and EGF cooperate:** Wnt primes AT2 cells so they can respond robustly to proliferative growth factors.
- **Disruption of this niche communication is linked to fibrosis:** aberrant Wnt activity and altered fibroblast-epithelial crosstalk can impede regenerative repair and promote scarring.
- **Therapeutic approaches need precision:** cell-specific, temporally controlled modulation of Wnt and growth-factor pathways holds promise but requires caution.

---

## References

1. **AT2 cells as facultative stem/progenitors (review/article; PMC11027191)**: <a href="https://pmc.ncbi.nlm.nih.gov/articles/PMC11027191/" target="_blank" rel="noopener noreferrer">https://pmc.ncbi.nlm.nih.gov/articles/PMC11027191/</a>
2. **Rare Wnt-active/Axin2⁺ AT2 cells and clonal renewal (PMC5997265)**: <a href="https://pmc.ncbi.nlm.nih.gov/articles/PMC5997265/" target="_blank" rel="noopener noreferrer">https://pmc.ncbi.nlm.nih.gov/articles/PMC5997265/</a>
3. **Bulk AT2 recruitment / transient Wnt(Axin2)-like activation after injury (PMC9068227)**: <a href="https://pmc.ncbi.nlm.nih.gov/articles/PMC9068227/" target="_blank" rel="noopener noreferrer">https://pmc.ncbi.nlm.nih.gov/articles/PMC9068227/</a>
4. **Canonical Wnt/β-catenin maintains progenitor identity (Nature link)**: <a href="https://www.nature.com/articles/s41420-019-0147-9" target="_blank" rel="noopener noreferrer">https://www.nature.com/articles/s41420-019-0147-9</a>
5. **Wnt downregulation required for AT2→AT1 maturation (Nature Communications link)**: <a href="https://www.nature.com/articles/s42003-021-02118-w" target="_blank" rel="noopener noreferrer">https://www.nature.com/articles/s42003-021-02118-w</a>
6. **Mesenchymal Wnt niche adjacent to AT2 cells (Science; DOI: 10.1126/science.aam6603)**: <a href="https://www.science.org/doi/10.1126/science.aam6603" target="_blank" rel="noopener noreferrer">https://www.science.org/doi/10.1126/science.aam6603</a>
7. **scRNA-seq/spatial + injury-associated Wnt ligand programs (Desai lab PDFs)**: <a href="https://desailab.stanford.edu/sites/g/files/sbiybj24296/files/media/file/sc2.pdf" target="_blank" rel="noopener noreferrer">https://desailab.stanford.edu/sites/g/files/sbiybj24296/files/media/file/sc2.pdf</a> (and related <a href="https://desailab.stanford.edu/sites/g/files/sbiybj24296/files/media/file/sc1.pdf" target="_blank" rel="noopener noreferrer">sc1.pdf</a>)
8. **Aberrant epithelial states in fibrotic lungs (JCI link)**: <a href="https://www.jci.org/articles/view/170504" target="_blank" rel="noopener noreferrer">https://www.jci.org/articles/view/170504</a>
9. **WNT5A and profibrotic signaling (ATS; DOI: 10.1164/rccm.201708-1580OC)**: <a href="https://www.atsjournals.org/doi/10.1164/rccm.201708-1580OC" target="_blank" rel="noopener noreferrer">https://www.atsjournals.org/doi/10.1164/rccm.201708-1580OC</a>
10. **Niche signaling / Wnt–TGFβ crosstalk in disordered repair (bioRxiv)**: <a href="https://www.biorxiv.org/content/10.1101/2023.08.02.551383v1.full.pdf" target="_blank" rel="noopener noreferrer">https://www.biorxiv.org/content/10.1101/2023.08.02.551383v1.full.pdf</a>
11. **Wnt–EGFR synergy in AT2 proliferation and repair (Desai lab PDFs)**: <a href="https://desailab.stanford.edu/sites/g/files/sbiybj24296/files/media/file/sc1.pdf" target="_blank" rel="noopener noreferrer">https://desailab.stanford.edu/sites/g/files/sbiybj24296/files/media/file/sc1.pdf</a> (and related <a href="https://desailab.stanford.edu/sites/g/files/sbiybj24296/files/media/file/sc2.pdf" target="_blank" rel="noopener noreferrer">sc2.pdf</a>)

