# NGS Single Cell RNA Seq

## Biological Context: Our Example Dataset
This repository contains analysis materials and metadata for GSE174609, a study investigating immune cell responses in periodontitis patients before and after treatment.

## Scientific Background
Periodontitis is a chronic inflammatory disease affecting tissues surrounding teeth. While primarily a local oral disease, evidence suggests systemic impacts through:

- Elevated circulating inflammatory mediators
- Bacteremia from periodontal pathogens
- Changes in systemic immune cell populations
- Associations with cardiovascular disease, diabetes, and rheumatoid arthritis

## Research Question
How does non-surgical periodontal therapy (scaling and root planing) affect the systemic immune cell landscape, and can these changes explain links between oral and systemic health?

## Experimental Design

- Samples: 12 total
  - 4 healthy donors (controls)
  - 4 periodontitis patients before treatment (pre-treatment)
  - 4 periodontitis patients after treatment (post-treatment, same individuals)
- Cell Type: Peripheral blood mononuclear cells (PBMCs)
- Technology: 10x Genomics Chromium 3′ v3.1
- Goal: Identify immune cell populations affected by periodontitis and treatment

## Expected Cell Types in PBMCs

- T cells (CD4+, CD8+, regulatory, memory, naive)
- B cells (naive, memory, plasma cells)
- Monocytes (classical, intermediate, non-classical)
- NK cells (natural killer cells)
- Dendritic cells
- Potentially rare populations

## Repository Structure

- `raw_data/` - FASTQ files (not included in repo)
- `references/` - reference genomes and Cell Ranger indices (prefer to reference externally)
- `cellranger_output/` - results from `cellranger count`
- `analysis/` - downstream analysis notebooks and scripts
- `docs/` - notes, QC reports, figures

## Usage

1. Set absolute paths for data and references (avoid `~` on clusters):

```bash
FASTQ_DIR=/absolute/path/to/raw_data
TRANSCRIPTOME=/absolute/path/to/refdata-gex-GRCh38-2024-A
SAMPLE=Healthy_1
```

2. Example `cellranger count` command:

```bash
cellranger count \\
  --id="${SAMPLE}" \\
  --fastqs="${FASTQ_DIR}" \\
  --sample="${SAMPLE}" \\
  --transcriptome="${TRANSCRIPTOME}" \\
  --expect-cells=5000 \\
  --localcores=16 \\
  --localmem=64 \\
  --chemistry=SC3Pv3 \\
  --create-bam=false
```

3. To create the GitHub repository and push this project (requires GitHub auth):

```bash
git init
git add .
git commit -m "Initial commit: add README and project layout"
# Create repo with gh (if available and authenticated):
gh repo create "NGS-Single-Cell-RNA-Seq" --public --source=. --remote=origin --push --confirm
# OR create remote manually on GitHub and then:
git remote add origin <git@github.com:YOUR_USERNAME/NGS-Single-Cell-RNA-Seq.git>
git branch -M main
git push -u origin main
```

## Notes
- Do not commit raw FASTQ, large reference files, or large `cellranger_output` directories. Use `.gitignore` and/or store large files in an object store (S3) or on institutional storage.

## Contact
For questions about the pipeline or repository layout, open an issue or contact the maintainers.
