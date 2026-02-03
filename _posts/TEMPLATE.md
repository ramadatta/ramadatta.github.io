---
# REQUIRED FIELDS
layout: post
title: "Your Post Title Here"
date: YYYY-MM-DD  # Format: 2026-01-27 (year-month-day)
description: "A brief description that appears in previews and search results (keep under 160 characters)"
tags: [tag1, tag2, tag3]  # Array of tags for categorization
categories: [category]  # Single category (e.g., lung biology, bioinformatics, tutorial)

# OPTIONAL FIELDS
giscus_comments: true  # Enable comments (set to false or remove to disable)
thumbnail: /assets/images/your-image.png  # Optional thumbnail image for blog listing
---

<!-- 
FILENAME FORMAT: YYYY-MM-DD-title-slug.md
Example: 2026-01-27-lung-epithelium-repairs-itself.md
-->

<!-- INTRODUCTION -->
Your opening paragraph should hook the reader and introduce the main topic. You can include **bold text**, *italic text*, and inline citations like this: <a href="https://example.com/paper" target="_blank" rel="noopener noreferrer">[1]</a>

---

## Section Header (H2)

Use H2 headers (`##`) for major sections. You can include:

- **Bullet points** with bold emphasis
- Regular bullet points
- Links to [other posts]({% post_url YYYY-MM-DD-other-post-title %})
- Inline citations: <a href="https://example.com/paper2" target="_blank" rel="noopener noreferrer">[2]</a>

### Subsection (H3)

Use H3 headers (`###`) for subsections within major sections.

---

## Code Examples

Include code blocks with syntax highlighting:

```python
# Python example
import scanpy as sc
adata = sc.read_h5ad('data.h5ad')
sc.pp.normalize_total(adata)
```

```r
# R example
library(Seurat)
seurat_obj <- Read10X(data.dir = "path/to/data")
```

```bash
# Bash/command line example
git add _posts/YYYY-MM-DD-new-post.md
git commit -m "Add new post"
git push
```

---

## Figures: Mermaid Diagrams

Use Mermaid diagrams for flowcharts, process diagrams, or conceptual illustrations:

<figure>
<div class="mermaid">
flowchart TD
    subgraph Section1["Section 1"]
        A[Node A]
        B[Node B]
        A -->|Label| B
    end
    
    subgraph Section2["Section 2"]
        C[Node C]
        D[Node D]
        C -->|Label| D
    end
    
    style A fill:#90EE90
    style B fill:#DDA0DD
    style C fill:#FFA500
    style D fill:#FFD700
</div>
<figcaption><strong>Figure 1.</strong> Description of what the diagram shows. Include key concepts and what readers should take away.</figcaption>
</figure>

**Mermaid Tips:**
- Use `subgraph` for grouping related nodes
- Use `style` for colors (hex codes)
- Keep node labels concise
- Use descriptive arrow labels

---

## Figures: Regular Images

For static images (PNG, JPG, SVG):

<figure>
  <img src="{{ '/assets/images/your-image.png' | relative_url }}" alt="Descriptive alt text for accessibility" />
  <figcaption><strong>Figure 2.</strong> Caption describing the image content and its relevance to the post.</figcaption>
</figure>

**Image Tips:**
- Save images in `assets/images/`
- Use descriptive alt text for accessibility
- Use `{{ '/assets/images/filename.png' | relative_url }}` for Jekyll paths
- Keep file sizes reasonable for web

---

## Lists and Formatting

### Numbered Lists

1. First item
2. Second item
3. Third item with **bold** and <a href="https://example.com" target="_blank" rel="noopener noreferrer">[3]</a> citation

### Bullet Lists

- Item one
- Item two with *emphasis*
- Item three with **strong emphasis**

### Nested Lists

- Main point
  - Sub-point one
  - Sub-point two
- Another main point
  - Sub-point

---

## Tables

| Column 1 | Column 2 | Column 3 |
|----------|----------|----------|
| Data 1   | Data 2   | Data 3   |
| Data 4   | Data 5   | Data 6   |

---

## Inline Citations Format

Use HTML anchor tags for citations that open in new tabs:

- Single citation: <a href="https://pmc.ncbi.nlm.nih.gov/articles/PMC12345678/" target="_blank" rel="noopener noreferrer">[1]</a>
- Multiple citations: <a href="https://example.com/paper1" target="_blank" rel="noopener noreferrer">[2]</a>, <a href="https://example.com/paper2" target="_blank" rel="noopener noreferrer">[3]</a>

**Citation Format:**
```html
<a href="URL_HERE" target="_blank" rel="noopener noreferrer">[X]</a>
```

---

## Summary or Conclusion

Wrap up your post with key takeaways or a conclusion. You can:

- Summarize main points
- Provide next steps
- Link to related posts: [**Related Post Title**]({% post_url YYYY-MM-DD-related-post %})
- Invite discussion or feedback

---

## References

Format references with proper HTML links that open in new tabs:

1. **Paper Title (Journal, Year)**: <a href="https://example.com/paper1" target="_blank" rel="noopener noreferrer">https://example.com/paper1</a>
2. **Another Paper (Journal, Year; DOI: 10.xxxx/xxxxx)**: <a href="https://example.com/paper2" target="_blank" rel="noopener noreferrer">https://example.com/paper2</a>
3. **Preprint (bioRxiv)**: <a href="https://www.biorxiv.org/content/10.1101/YYYY.MM.DD.XXXXXXv1.full.pdf" target="_blank" rel="noopener noreferrer">https://www.biorxiv.org/content/10.1101/YYYY.MM.DD.XXXXXXv1.full.pdf</a>
4. **PMC Article (PMC12345678)**: <a href="https://pmc.ncbi.nlm.nih.gov/articles/PMC12345678/" target="_blank" rel="noopener noreferrer">https://pmc.ncbi.nlm.nih.gov/articles/PMC12345678/</a>
5. **Related PDFs**: <a href="https://example.com/main.pdf" target="_blank" rel="noopener noreferrer">https://example.com/main.pdf</a> (and related <a href="https://example.com/supplement.pdf" target="_blank" rel="noopener noreferrer">supplement.pdf</a>)

**Reference Format Tips:**
- Number references sequentially
- Include descriptive text before the URL
- Use HTML anchor tags for all links
- Add `target="_blank" rel="noopener noreferrer"` for new tab opening
- Include journal names, DOIs, or PMC IDs when available

---

<!-- 
TEMPLATE USAGE INSTRUCTIONS:

1. Copy this template: cp _posts/TEMPLATE.md _posts/YYYY-MM-DD-your-title-slug.md
2. Replace all placeholder text (YYYY-MM-DD, Your Title, etc.)
3. Remove sections you don't need
4. Add your content
5. Update references section
6. Test locally: bundle exec jekyll serve
7. Commit and push to GitHub

COMMON PATTERNS:

- Scientific posts: Include introduction, methods/results, discussion, references
- Tutorial posts: Include overview, step-by-step instructions, code examples, summary
- Opinion/reflection posts: Include personal narrative, insights, conclusion

REMEMBER:
- Date format: YYYY-MM-DD (e.g., 2026-01-27)
- Filename format: YYYY-MM-DD-title-slug.md
- Use horizontal rules (---) between major sections
- Keep descriptions under 160 characters
- Use proper heading hierarchy (H2 for sections, H3 for subsections)
-->
