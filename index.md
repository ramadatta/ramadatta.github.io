---
layout: home
title: ""
sections:
  - { id: about, label: About }
  - { id: research, label: Research }
  - { id: experience, label: Experience }
  - { id: education, label: Education }
  - { id: publications, label: Publications }
  - { id: presentations, label: Presentations }
  - { id: skills, label: Skills }
  - { id: community, label: Community }
  - { id: certifications, label: Certifications }
  - { id: grants, label: Grants & Awards }
  - { id: writing, label: Recent writing }
  - { id: contact, label: Contact }
links:
  - { label: "CV (PDF)", url: "/assets/pdf/Prakki_CV.pdf", download: true }
  - { label: "Google Scholar", url: "https://scholar.google.com/citations?hl=en&user=kiCDFpIAAAAJ&view_op=list_works&sortby=pubdate" }
  - { label: "ORCID", url: "https://orcid.org/0000-0002-9254-2557" }
  - { label: "GitHub", url: "https://github.com/ramadatta" }
  - { label: "LinkedIn", url: "https://www.linkedin.com/in/prakki-sai-rama-sridatta-data/" }
  - { label: "BioStars", url: "https://www.biostars.org/u/3738/" }
  - { label: "YouTube", url: "https://www.youtube.com/@asearchforsolutions" }
  - { label: "Email", url: "mailto:ramadatta.g88@gmail.com" }
---

<div class="home-open">
  <span class="home-open-label">Open to opportunities</span>
  <p class="home-open-lead">Seeking <strong>Senior Scientist</strong> roles in therapeutics companies: computational biology for <strong>target discovery</strong>, with a focus on <strong>single-cell and spatial biology</strong>.</p>
  <p class="home-open-meta">Available from <strong>July 2027</strong> · Based in Munich until May 2027</p>
  <div class="home-open-actions">
    <a class="home-btn home-btn--primary" href="mailto:ramadatta.g88@gmail.com?subject=Opportunity">Get in touch</a>
    <a class="home-btn" href="{{ '/assets/pdf/Prakki_CV.pdf' | relative_url }}" download>Download CV</a>
  </div>
</div>

<div class="home-news">
  <b>News</b>
  <p>Presented a <a href="https://doi.org/10.1183/23120541.LSC-2026.PS336" target="_blank" rel="noopener noreferrer">poster</a> at the Lung Science Conference (2026), Estoril, Portugal · Awarded the EHLRS Grant (2025) for <em>PULP-in-PEAR</em>.</p>
</div>

<section id="about" class="home-section">
  <h2 class="home-title">About Me</h2>

  <p>I am a computational biologist and final-year PhD candidate with 15+ years of experience spanning clinical genomics, NGS workflows, scientific computing, and single-cell disease biology.</p>
  <p>My doctoral research applies human and mouse lung multi-omics to study fibrotic disease states, gene regulatory programs, and perturbation responses with relevance to translational research. As I move toward the final stage of my PhD, I am preparing to transition into industry R&amp;D, where I hope to contribute to computational biology strategies in target discovery, biomarker development, translational medicine, and precision medicine.</p>
  <p>Technically, I work day-to-day in Python and R for single-cell and multi-omics analysis, including Scanpy, Seurat, and Bioconductor. I also bring a strong foundation in UNIX/Linux, shell scripting, AWK/SED, HPC environments, AWS, GitHub-based version control, and reproducible workflow development. Earlier in my career, Perl was a major part of my bioinformatics toolkit, and that background still shapes how I think about robust, practical data handling.</p>
  <p>I care deeply about making analyses reproducible, well-documented, and easy for others to understand, reuse, and build upon. My code and selected projects are available on <a href="https://github.com/ramadatta" target="_blank" rel="noopener noreferrer">GitHub</a>.</p>
  <p>Beyond my research, I enjoy practical scientific community-building. I have spent 14+ years learning from and contributing to <a href="https://www.biostars.org/u/3738/" target="_blank" rel="noopener noreferrer">BioStars</a>, long before LLM-based tools became part of everyday bioinformatics workflows.</p>
  <p>I also maintain a <a href="{{ '/blog/' | relative_url }}">blog</a> where I document discoveries, mistakes, and learning moments from my PhD journey, and I run a small <a href="https://www.youtube.com/@asearchforsolutions" target="_blank" rel="noopener noreferrer">YouTube channel</a> with tutorials and walkthroughs for researchers working through similar problems.</p>
  <p>Sharing what I learn has become one of the most rewarding parts of my work. If something I wrote, answered, or explained ever saved your time without me knowing, let me know! :)</p>
  <p>I value conversations that move naturally between science, technology, ideas, and everyday life. If my work or outlook resonates with you, I would be happy to connect here.</p>

  <figure class="home-quote">
    <blockquote>
      <p>“To ensure that the community can explore and build upon our results without specialized bioinformatics expertise, we have developed an interactive browser-based webtool… Really nice work by the talented Sai Rama Sridatta Prakki.”</p>
    </blockquote>
    <figcaption>
      <strong>Prof. Dr. Herbert Schiller</strong>, Director, Research Unit for Precision Regenerative Medicine, Helmholtz Munich
      <span>On the <a href="https://hschillerlabshiny.shinyapps.io/BleomycinAging/" target="_blank" rel="noopener noreferrer">Bleomycin Aging webtool</a> · <a href="https://www.youtube.com/watch?v=X3j85CMKCWo&amp;t=9s" target="_blank" rel="noopener noreferrer">Video tutorial ↗</a> · <a href="https://x.com/SchillerLab/status/1950536045776290220" target="_blank" rel="noopener noreferrer">Original post, Jul 2025 ↗</a></span>
    </figcaption>
  </figure>
</section>

<section id="research" class="home-section">
  <h2 class="home-title">Research</h2>

  <p>My current research focuses on understanding lung biology at single-cell resolution at the <strong>Research Unit for Precision Regenerative Medicine</strong> and <strong>Institute of Computational Biology</strong>, Helmholtz Munich.</p>

  <div class="home-tabs" data-tabs>
    <div class="home-tablist" role="tablist" aria-label="Research">
      <button type="button" role="tab" id="tab-focus" aria-controls="panel-focus" aria-selected="true">Focus areas</button>
      <button type="button" role="tab" id="tab-projects" aria-controls="panel-projects" aria-selected="false">Current projects</button>
      <button type="button" role="tab" id="tab-webapps" aria-controls="panel-webapps" aria-selected="false">Web apps</button>
      <button type="button" role="tab" id="tab-tools" aria-controls="panel-tools" aria-selected="false">Tools I've built</button>
      <button type="button" role="tab" id="tab-supervisors" aria-controls="panel-supervisors" aria-selected="false">Supervisors</button>
    </div>

    <div class="home-tabpanel" role="tabpanel" id="panel-focus" aria-labelledby="tab-focus">
      <h3 class="home-subtitle">Focus areas</h3>
      <div class="home-cards home-cards--three">
        <div class="home-card">
          <span class="home-card-kicker">Analysis</span>
          <h3>sc/snRNA-seq analysis</h3>
          <p>Processing and analyzing single-cell/nucleus RNA-seq data from lung tissues (human/mouse)</p>
        </div>
        <div class="home-card">
          <span class="home-card-kicker">Software</span>
          <h3>Interactive web applications</h3>
          <p>Building tools for the lung research community</p>
        </div>
        <div class="home-card">
          <span class="home-card-kicker">Teaching</span>
          <h3>Training &amp; mentorship</h3>
          <p>Helping colleagues improve their bioinformatics skills</p>
        </div>
      </div>
    </div>

    <div class="home-tabpanel" role="tabpanel" id="panel-projects" aria-labelledby="tab-projects">
      <h3 class="home-subtitle">Current projects</h3>
      <div class="home-cards">
        <div class="home-card">
          <span class="home-card-kicker">Project 01 · in vivo</span>
          <h3>Profiling of in vivo IPF Lung Tissues</h3>
          <p>Characterizing Early Disease Cell States in Idiopathic Pulmonary Fibrosis (IPF) Using MicroCT Staging and Single-Nucleus Transcriptomics</p>
          <div class="home-tags"><span>snRNA-seq</span><span>MicroCT</span><span>IPF</span></div>
        </div>
        <div class="home-card">
          <span class="home-card-kicker">Project 02 · ex vivo</span>
          <h3>Exploring of fibrosis induced ex vivo Lung Tissues</h3>
          <p>Time-Resolved Single-Nuclei RNA-Seq in Fibrotic Human Precision-Cut Lung Slices coupled to perturbations</p>
          <div class="home-tags"><span>ex vivo</span><span>PCLS</span><span>perturbation</span></div>
        </div>
      </div>
    </div>

    <div class="home-tabpanel" role="tabpanel" id="panel-webapps" aria-labelledby="tab-webapps">
      <h3 class="home-subtitle">Web apps</h3>
      <div class="home-cards home-cards--three">
        <div class="home-card">
          <span class="home-card-kicker">R Shiny · <span class="home-status home-status--live">Live</span></span>
          <h3>Bleomycin Aging App</h3>
          <p>Interactive R Shiny application for exploring lung aging data</p>
          <div class="home-tags"><span>R Shiny</span><span>mouse lung</span><span>aging</span></div>
          <a class="home-card-link" href="https://hschillerlabshiny.shinyapps.io/BleomycinAging/" target="_blank" rel="noopener noreferrer">Open the app ↗</a>
        </div>
        <div class="home-card home-card--upcoming">
          <span class="home-card-kicker">R Shiny · <span class="home-status">Coming soon</span></span>
          <h3>Galapagos Dataset Explorer</h3>
          <p>Interactive explorer for the Galapagos lung dataset</p>
          <div class="home-tags"><span>R Shiny</span><span>single-cell</span></div>
          <span class="home-card-note">Public release upcoming</span>
        </div>
        <div class="home-card home-card--upcoming">
          <span class="home-card-kicker">R Shiny · <span class="home-status">Coming soon</span></span>
          <h3>Ex Vivo Time-Course Explorer</h3>
          <p>Explore time-resolved single-nuclei RNA-seq of fibrotic human precision-cut lung slices</p>
          <div class="home-tags"><span>R Shiny</span><span>PCLS</span><span>time course</span></div>
          <span class="home-card-note">Public release upcoming</span>
        </div>
      </div>
    </div>

    <div class="home-tabpanel" role="tabpanel" id="panel-tools" aria-labelledby="tab-tools">
      <h3 class="home-subtitle">Tools I've built</h3>
      <div class="home-cards">
        <div class="home-card">
          <span class="home-card-kicker">Hugging Face Space</span>
          <h3>MAPLE</h3>
          <p>Turns marker genes into literature-backed cell type labels, with every call traced to a supporting sentence and PMID.</p>
          <div class="home-tags"><span>cell type annotation</span><span>literature mining</span><span>LLM</span></div>
          <a class="home-card-link" href="https://huggingface.co/spaces/ramadatta88/MAPLE" target="_blank" rel="noopener noreferrer">Try it on Hugging Face ↗</a>
        </div>
        <div class="home-card">
          <span class="home-card-kicker">Web app · Google Gemini</span>
          <h3>MarkerMind</h3>
          <p>AI-powered literature mining: extracts cell types, their marker genes, the methods used to identify them, and the biological context from pasted text or uploaded PDFs.</p>
          <div class="home-tags"><span>marker genes</span><span>literature mining</span><span>TypeScript</span><span>LLM</span></div>
          <div class="home-card-links">
            <a href="https://markermind.netlify.app/" target="_blank" rel="noopener noreferrer">Open the app ↗</a>
            <a href="https://github.com/ramadatta/MarkerMind" target="_blank" rel="noopener noreferrer">GitHub ↗</a>
          </div>
        </div>
        <div class="home-card">
          <span class="home-card-kicker">Python package</span>
          <h3>RUMAPS</h3>
          <p>A Python package for refined UMAP visualizations with clean labeling and interactive exploration, built to fit into single-cell workflows with scanpy and anndata.</p>
          <div class="home-tags"><span>Python</span><span>scanpy</span><span>anndata</span><span>UMAP</span></div>
          <a class="home-card-link" href="https://github.com/ramadatta/rumaps" target="_blank" rel="noopener noreferrer">View on GitHub ↗</a>
        </div>
        <div class="home-card">
          <span class="home-card-kicker">R package · JOSS</span>
          <h3>CPgeneProfiler</h3>
          <p>A lightweight R package to profile the Carbapenamase genes from genome assemblies.</p>
          <div class="home-tags"><span>R</span><span>AMR</span><span>bacterial genomics</span></div>
          <a class="home-card-link" href="https://github.com/ramadatta/CPgeneProfiler" target="_blank" rel="noopener noreferrer">View on GitHub ↗</a>
        </div>
      </div>
    </div>

    <div class="home-tabpanel" role="tabpanel" id="panel-supervisors" aria-labelledby="tab-supervisors">
      <h3 class="home-subtitle">Supervisors</h3>
      <div class="home-cards">
        <div class="home-card">
          <span class="home-card-kicker">Helmholtz Munich</span>
          <h3>Prof. Dr. Herbert Schiller</h3>
          <p>Director, Research Unit for Precision Regenerative Medicine</p>
        </div>
        <div class="home-card">
          <span class="home-card-kicker">Helmholtz Munich</span>
          <h3>Prof. Dr. Fabian Theis</h3>
          <p>Director, Institute of Computational Biology</p>
        </div>
      </div>
    </div>
  </div>
</section>

<section id="experience" class="home-section">
  <h2 class="home-title">Experience</h2>

  <div class="home-row">
    <span class="home-date">Jun 2023 – Present</span>
    <div>
      <h3>Doctoral Candidate — Computational Biology</h3>
      <p class="home-org">Helmholtz Center Munich · Germany</p>
      <ul>
        <li>Analyze human and mouse sc/snRNA-seq datasets from lung tissues to study cell states and molecular programs associated with pulmonary fibrosis.</li>
        <li>Work with IPF and precision-cut lung slice datasets to investigate disease biology and experimental perturbation responses.</li>
        <li>Develop computational tools and applications to support data exploration and interpretation for the lung research community.</li>
        <li>Train wet-lab and computational colleagues in bioinformatics workflows, single-cell analysis, and reproducible data analysis practices.</li>
      </ul>
    </div>
  </div>
  <div class="home-row">
    <span class="home-date">Jul 2019 – May 2023</span>
    <div>
      <h3>Senior Bioinformatician</h3>
      <p class="home-org">National Centre for Infectious Diseases · Singapore</p>
      <ul>
        <li>Performed bacterial whole-genome sequencing analysis and reporting to support infectious disease surveillance and infection-control investigations.</li>
        <li>Developed and published the R package <strong>CPgeneProfiler</strong> for bacterial genomic feature profiling.</li>
        <li>Built automated and reproducible clinical bioinformatics workflows using workflow management systems.</li>
        <li>Managed sequencing datasets, HPC-based analysis pipelines, and computational infrastructure for clinical and public health genomics projects.</li>
      </ul>
    </div>
  </div>
  <div class="home-row">
    <span class="home-date">Aug 2017 – Jun 2019</span>
    <div>
      <h3>Research Associate</h3>
      <p class="home-org">Saw Swee Hock School of Public Health, NUS / NCID · Singapore</p>
      <ul>
        <li>Developed standard operating procedures for bacterial genome sequence analysis.</li>
        <li>Managed bacterial genomic datasets and associated metadata for infectious disease research and surveillance.</li>
        <li>Optimized HPC workflows for large-scale genome analysis and computational performance.</li>
      </ul>
    </div>
  </div>
  <div class="home-row">
    <span class="home-date">Jun 2012 – Dec 2016</span>
    <div>
      <h3>Bioinformatics Engineer</h3>
      <p class="home-org">Temasek Life Sciences Laboratory, NUS · Singapore</p>
      <ul>
        <li>Contributed to the <strong>Asian Seabass Genome Project</strong> in collaboration with the South African National Bioinformatics Institute.</li>
        <li>Performed genome assembly, transcriptome analysis, and comparative genomics for aquaculture and evolutionary genomics projects.</li>
        <li>Applied computational genomics approaches across Asian seabass, tilapia, and arowana datasets.</li>
      </ul>
    </div>
  </div>
  <div class="home-row">
    <span class="home-date">Sep 2011 – Apr 2012</span>
    <div>
      <h3>Research Intern</h3>
      <p class="home-org">Genome Institute of Singapore, A*STAR · Singapore</p>
      <ul>
        <li>Conducted computational analysis of algal genome and transcriptome datasets, including <em>Botryococcus</em> species.</li>
      </ul>
    </div>
  </div>
</section>

<section id="education" class="home-section">
  <h2 class="home-title">Education</h2>

  <div class="home-row">
    <span class="home-date">2023 – Present</span>
    <div><h3>Doctoral Candidate in Bioinformatics</h3><p class="home-org">Helmholtz Center Munich, Germany</p></div>
  </div>
  <div class="home-row">
    <span class="home-date">2010 – 2012</span>
    <div><h3>Master of Science in Bioinformatics</h3><p class="home-org">Nanyang Technological University (NTU), Singapore</p></div>
  </div>
  <div class="home-row">
    <span class="home-date">2006 – 2010</span>
    <div><h3>Bachelor of Technology in Bioinformatics</h3><p class="home-org">Bharath University, Chennai, India</p></div>
  </div>
</section>

<section id="publications" class="home-section">
  <h2 class="home-title">Selected publications <a href="https://scholar.google.com/citations?hl=en&user=kiCDFpIAAAAJ&view_op=list_works&sortby=pubdate" target="_blank" rel="noopener noreferrer">All on Google Scholar →</a></h2>

  <p><strong>15+ peer-reviewed publications</strong> in genomics, infectious diseases, and single-cell biology.</p>

  <div class="home-pub">
    <span class="home-date">2025</span>
    <div>
      <p class="home-pub-title">Single cell decomposition of multicellular aging programs associated with impaired lung regeneration.<span class="home-badge">preprint</span></p>
      <p class="home-pub-authors">Gote-Schniering J, Melo-Narváez MC, Boosarpu G, Lauer D, <u>Prakki SRS</u>, et al.</p>
      <p class="home-pub-venue"><em>bioRxiv</em> · <a href="https://doi.org/10.1101/2025.07.24.666371" target="_blank" rel="noopener noreferrer">DOI</a></p>
    </div>
  </div>
  <div class="home-pub">
    <span class="home-date">2025</span>
    <div>
      <p class="home-pub-title">Plasmid dynamics driving carbapenemase gene dissemination in healthcare environments.</p>
      <p class="home-pub-authors">Koh V, Cabrera R, <u>Prakki SRS</u>, et al.</p>
      <p class="home-pub-venue"><em>Nature Communications</em> 16:9522 · <a href="https://doi.org/10.1038/s41467-025-64515-7" target="_blank" rel="noopener noreferrer">DOI</a></p>
    </div>
  </div>
  <div class="home-pub">
    <span class="home-date">2023</span>
    <div>
      <p class="home-pub-title">Dissemination of Pseudomonas aeruginosa bla<sub>NDM-1</sub> positive ST308 clone in Singapore.</p>
      <p class="home-pub-authors"><u>Prakki SRS</u>, Ng OT, et al.</p>
      <p class="home-pub-venue"><em>Microbiology Spectrum</em> · <a href="https://doi.org/10.1128/spectrum.04033-22" target="_blank" rel="noopener noreferrer">DOI</a></p>
    </div>
  </div>
  <div class="home-pub">
    <span class="home-date">2022</span>
    <div>
      <p class="home-pub-title">Whole Genome Sequencing Reveals Hidden Transmission of Carbapenemase-producing Enterobacterales.</p>
      <p class="home-pub-authors">Marimuthu K, Venkatachalam I, <u>Prakki SRS</u>, Ng OT, et al.</p>
      <p class="home-pub-venue"><em>Nature Communications</em> · <a href="https://doi.org/10.1038/s41467-022-30059-7" target="_blank" rel="noopener noreferrer">DOI</a></p>
    </div>
  </div>
  <div class="home-pub">
    <span class="home-date">2020</span>
    <div>
      <p class="home-pub-title">CPgeneProfiler: A lightweight R package to profile the Carbapenamase genes from genome assemblies.<span class="home-badge">software</span><span class="home-badge">R</span></p>
      <p class="home-pub-authors"><u>Prakki SRS</u>, et al.</p>
      <p class="home-pub-venue"><em>Journal of Open Source Software</em> 5(54):2473 · <a href="https://doi.org/10.21105/joss.02473" target="_blank" rel="noopener noreferrer">DOI</a> · <a href="https://github.com/ramadatta/CPgeneProfiler" target="_blank" rel="noopener noreferrer">Code</a></p>
    </div>
  </div>
  <div class="home-pub">
    <span class="home-date">2016</span>
    <div>
      <p class="home-pub-title">Chromosomal-level assembly of the Asian seabass genome using long sequence reads and multi-layered scaffolding.</p>
      <p class="home-pub-authors">Vij S, Kuhl H, ..., <u>Prakki SRS</u>, et al.</p>
      <p class="home-pub-venue"><em>PLoS Genetics</em> 12(4):e1005954 · <a href="https://doi.org/10.1371/journal.pgen.1005954" target="_blank" rel="noopener noreferrer">DOI</a></p>
    </div>
  </div>
</section>

<section id="presentations" class="home-section">
  <h2 class="home-title">Poster presentations</h2>

  <div class="home-row">
    <span class="home-date">2026</span>
    <div><h3>ERS Lung Science Conference (LSC) — Estoril, Portugal</h3><p class="home-org">Poster: Time-Resolved Single-Nuclei RNA-Seq in Fibrotic Human Precision-Cut Lung Slices coupled to Perturbations · <em>ERJ Open Research</em> 12(suppl 18):PS336 · <a href="https://doi.org/10.1183/23120541.LSC-2026.PS336" target="_blank" rel="noopener noreferrer">DOI</a></p></div>
  </div>

  <div class="home-row">
    <span class="home-date">2025</span>
    <div><h3>Single Cell Genomics (SCG) — Stockholm, Sweden</h3><p class="home-org">Poster: Dissecting Early Disease Cell States in Idiopathic Pulmonary Fibrosis (IPF) Using MicroCT Staging and Single-Nucleus Transcriptomics</p></div>
  </div>
  <div class="home-row">
    <span class="home-date">2024</span>
    <div><h3>Deutsches Zentrum für Lungenforschung (DZL) — Bad Nauheim, Germany</h3><p class="home-org">Poster: Towards perturbation analysis of cell lineage specific transcriptional regulons in pulmonary fibrosis</p></div>
  </div>
  <div class="home-row">
    <span class="home-date">2022</span>
    <div><h3>Singapore Health and Biomedical Conference (SHBC) — Singapore</h3><p class="home-org">Poster: Dissemination of Pseudomonas aeruginosa blaNDM-1 positive ST308 clone in Singapore</p></div>
  </div>
  <div class="home-row">
    <span class="home-date">2019</span>
    <div><h3>European Society of Clinical Microbiology and Infectious Diseases (ECCMID) — Amsterdam, Netherlands</h3><p class="home-org">Poster: Tracking plasmid-mediated horizontal gene transmission of blaNDM across Enterobacteriaceae</p></div>
  </div>
  <div class="home-row">
    <span class="home-date">2018</span>
    <div><h3>European Society of Clinical Microbiology and Infectious Diseases (ECCMID) — Madrid, Spain</h3><p class="home-org">Poster: Population whole-genome sequencing uncovers plasmid-mediated NDM persistence</p></div>
  </div>
  <div class="home-row">
    <span class="home-date">2014</span>
    <div><h3>Plant and Animal Genome (PAG) Asia — Singapore</h3><p class="home-org">Poster: Improved Assembly of the Asian Seabass Transcriptome</p></div>
  </div>
  <div class="home-row">
    <span class="home-date">2013</span>
    <div><h3>Plant and Animal Genome (PAG) — San Diego, California</h3><p class="home-org">Poster: Sequencing and Assembly of the Asian seabass Transcriptome</p></div>
  </div>
</section>

<section id="skills" class="home-section">
  <h2 class="home-title">Technical skills</h2>

  <div class="home-skill">
    <h3>Programming</h3>
    <div class="home-tags"><span>Python</span><span>R</span><span>PERL</span><span>Bash/Shell</span><span>AWK</span><span>SED</span></div>
  </div>
  <div class="home-skill">
    <h3>AI app development</h3>
    <div class="home-tags"><span>Agentic LLM applications</span><span>Literature-grounded pipelines (e.g. MAPLE)</span><span>Google Gemini</span><span>Hugging Face Spaces</span></div>
  </div>
  <div class="home-skill">
    <h3>AI-assisted coding</h3>
    <div class="home-tags"><span>Cursor</span><span>VS Code</span><span>Codex</span><span>Claude Cowork</span></div>
  </div>
  <div class="home-skill">
    <h3>AI for research</h3>
    <div class="home-tags"><span>Knowledge synthesis (NotebookLM)</span></div>
  </div>
  <div class="home-skill">
    <h3>Domains with Bioinformatics experience</h3>
    <div class="home-tags"><span>Single Cell Genomics</span><span>Bacterial Genomics</span><span>Fish Genomics</span></div>
  </div>
  <div class="home-skill">
    <h3>WebApp development</h3>
    <div class="home-tags"><span>Shiny for Python</span><span>Shiny for R</span></div>
  </div>
  <div class="home-skill">
    <h3>Infrastructure</h3>
    <div class="home-tags"><span>HPC</span><span>Linux</span><span>Git/GitHub</span></div>
  </div>
  <div class="home-skill">
    <h3>Languages</h3>
    <div class="home-tags"><span>English (professional)</span><span>Telugu (native)</span><span>Tamil (intermediate)</span><span>Hindi (limited)</span><span>Odia (limited)</span></div>
  </div>
</section>

<section id="community" class="home-section">
  <span id="contributions"></span>
  <h2 class="home-title">Community</h2>

  <div class="home-cards">
    <div class="home-card">
      <span class="home-card-kicker">Peer review</span>
      <h3>Journal reviewer</h3>
      <ul class="home-card-list">
        <li><em>Journal of Open Source Software</em>: PPanGGOLiN v2, a prokaryotic pangenome analysis tool (software, documentation and paper), 2026 · <a href="https://github.com/openjournals/joss-reviews/issues/11101" target="_blank" rel="noopener noreferrer">Review thread ↗</a></li>
        <li><em>BMC Infectious Diseases</em> (Springer Nature), 2023</li>
      </ul>
      <a class="home-card-link" href="https://orcid.org/0000-0002-9254-2557" target="_blank" rel="noopener noreferrer">ORCID record ↗</a>
    </div>
    <div class="home-card">
      <span class="home-card-kicker">Q&amp;A forum · <span class="home-status home-status--live">14+ years</span></span>
      <h3>Biostar</h3>
      <p>Active community member</p>
      <a class="home-card-link" href="https://www.biostars.org/u/3738/" target="_blank" rel="noopener noreferrer">Profile: Prakki Rama ↗</a>
    </div>
    <div class="home-card">
      <span class="home-card-kicker">Teaching · YouTube</span>
      <h3>Tutorial Videos</h3>
      <p>Tutorials and walkthroughs for researchers working through bioinformatics problems</p>
      <a class="home-card-link" href="https://www.youtube.com/@asearchforsolutions" target="_blank" rel="noopener noreferrer">A Search For Solutions ↗</a>
    </div>
    <div class="home-card">
      <span class="home-card-kicker">Blogger · archive</span>
      <h3>Blog</h3>
      <p>Fixes in software/bugs/myrants in the past years</p>
      <a class="home-card-link" href="http://asearchforsolutions.blogspot.com/" target="_blank" rel="noopener noreferrer">asearchforsolutions.blogspot.com ↗</a>
    </div>
  </div>
</section>

<section id="certifications" class="home-section">
  <h2 class="home-title">Certifications</h2>

  <ul class="home-list">
    <li><strong>IdeaLab</strong>, two-day start-up workshop, UnternehmerTUM &amp; TUM Venture Labs <span>Aug 2026</span></li>
    <li>Data Analysis and Statistical Inference <span>Duke University</span></li>
    <li>The Data Scientist's Toolbox <span>Johns Hopkins</span></li>
    <li>R Programming <span>Johns Hopkins</span></li>
    <li>Bioinformatics: Life Sciences on Your Computer <span>Johns Hopkins</span></li>
    <li>Data Wrangling in R <span>LinkedIn Learning</span></li>
    <li>Data Visualization with ggplot2 <span>LinkedIn Learning</span></li>
    <li>Tools for Data Science <span>IBM</span></li>
    <li>Big Data Systems Management <span>Temasek Polytechnic</span></li>
  </ul>
</section>

<section id="grants" class="home-section">
  <h2 class="home-title">Grants &amp; awards</h2>

  <div class="home-row">
    <span class="home-date">2025</span>
    <div>
      <h3>Honorable Mention, Posit Table Contest</h3>
      <p class="home-org"><em>Paris 2024 Olympics: Medal Performance Analysis</em>, a publication-quality data table built in R<br>Posit · <a href="https://posit.co/blog/2025-table-contest-winners" target="_blank" rel="noopener noreferrer">Results</a> · <a href="https://github.com/rich-iannone/table-contest/discussions/14" target="_blank" rel="noopener noreferrer">Entry</a> · <a href="https://github.com/ramadatta/paris-2024-olympics-table" target="_blank" rel="noopener noreferrer">Code</a></p>
    </div>
  </div>

  <div class="home-row">
    <span class="home-date">2025</span>
    <div>
      <h3>Environmental Health and Lung Research School (EHLRS) Grant Award (€2,000)</h3>
      <p class="home-org"><em>Project: "Plasticity Unforeseen: Loss of PTK7/SOX4 in human fibrotic lung slices drives Pulmonary Epithelial AT2 Reprogramming (PULP-in-PEAR)"</em><br>Helmholtz Munich, Comprehensive Pneumology Center</p>
    </div>
  </div>
  <div class="home-row">
    <span class="home-date">2023</span>
    <div>
      <h3>Winner, Theis Lab Christmas Competition: “Ugly Plot” Design</h3>
      <p class="home-org">Custom ggplot recognized for “exceptional excellence in the field of ugly plot design”<br>Institute of Computational Biology, Helmholtz Munich · <a href="https://x.com/Prakki_Rama/status/1735253164855636272" target="_blank" rel="noopener noreferrer">Post ↗</a></p>
    </div>
  </div>
  <div class="home-row">
    <span class="home-date">2009</span>
    <div>
      <h3>Best Performer Award</h3>
      <p class="home-org"><em>Poster presentation at National Symposium on "Insilico Drug Design against Leishmaniasis"</em><br>SRM University, Chennai, India</p>
    </div>
  </div>
</section>

<section id="writing" class="home-section">
  <h2 class="home-title">Recent writing <a href="{{ '/blog/' | relative_url }}">The Lab Notebook →</a></h2>

  {% for post in site.posts limit: 5 %}
    <div class="home-post">
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
      <span>{{ post.date | date: "%b %Y" }}</span>
    </div>
  {% endfor %}
</section>

<section id="contact" class="home-section">
  <h2 class="home-title">Contact</h2>

  <p>I'd be happy to connect! Feel free to reach out:</p>

  <div class="home-row">
    <span class="home-date">Email</span>
    <div><a href="mailto:ramadatta.g88@gmail.com">ramadatta.g88@gmail.com</a></div>
  </div>
  <div class="home-row">
    <span class="home-date">LinkedIn</span>
    <div><a href="https://www.linkedin.com/in/prakki-sai-rama-sridatta-data/" target="_blank" rel="noopener noreferrer">prakki-sai-rama-sridatta-data</a></div>
  </div>
  <div class="home-row">
    <span class="home-date">GitHub</span>
    <div><a href="https://github.com/ramadatta" target="_blank" rel="noopener noreferrer">@ramadatta</a></div>
  </div>
  <div class="home-row">
    <span class="home-date">Location</span>
    <div>Munich, Germany</div>
  </div>
</section>
