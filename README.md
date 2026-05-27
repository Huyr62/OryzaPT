# Oryza Pan-Transcriptome Pipeline

## Overview

This repository contains the computational pipeline used for constructing a cross-species pan-transcriptome in the genus *Oryza*.

Rice (*Oryza*) is both a globally important food crop and a well-established model organism with rich genomic and transcriptomic resources. With the rapid development of high-throughput sequencing technologies, pan-transcriptomics has emerged as a powerful framework for capturing the complete transcript diversity across multiple individuals, species, and conditions.

While most existing pan-transcriptome studies focus on variation within a single species, systematic cross-species pan-transcriptome construction remains limited. This project addresses that gap by developing a scalable computational workflow for transcriptome assembly, transcript classification, homologous gene identification, expression quantification, and co-expression network preparation across multiple *Oryza* species.

This pipeline was developed as part of a large-scale comparative transcriptomics study involving:

- **23 Oryza species/subspecies**
- **883 transcriptome samples**
- **Second-generation (short-read) sequencing data**
- **Third-generation (long-read) sequencing data**
- Publicly available transcriptomic datasets
- Species-specific transcriptome reconstruction and genus-level pan-transcriptome integration

The resulting resources support downstream studies in:

- comparative transcriptomics
- gene expression divergence
- homologous gene evolution
- non-coding RNA regulation
- rice molecular breeding
- functional genomics

---

## Research Objectives

The major goals of this project are:

1. Construct species-specific reference transcriptomes (**OryzaSRT**) for multiple *Oryza* species
2. Integrate species transcriptomes into a genus-level pan-transcriptome (**OryzaPRT**)
3. Classify transcripts into:
   - protein-coding genes
   - small peptide coding genes
   - long non-coding RNAs
4. Identify homologous genes across species
5. Quantify transcript abundance
6. Enable downstream co-expression network analysis
7. Build reusable infrastructure for pan-transcriptome database development

---

## Pipeline Workflow

The workflow consists of the following major steps:

### Step 1. Raw Data Quality Control

Short-read RNA-seq data are cleaned using **fastp** for:

- adapter trimming
- low-quality base removal
- paired-end quality filtering

Tools:
- fastp

Output:
- cleaned FASTQ files

---

### Step 2. Read Alignment

#### Short-read alignment

Second-generation RNA-seq reads are aligned to species reference genomes using:

- HISAT2

Post-processing:

- SAM → BAM conversion
- coordinate sorting

Tools:
- HISAT2
- samtools

---

#### Long-read alignment

Third-generation long-read transcriptomic data are aligned using:

- minimap2

Post-processing:

- SAM → sorted BAM

Tools:
- minimap2
- samtools

---

### Step 3. Transcript Assembly

Transcript reconstruction is performed independently for each sample using:

- StringTie

Both short-read and long-read assemblies are generated.

Outputs:

- sample-level GTF files
- gene abundance files

---

### Step 4. Hybrid Transcriptome Merging

Assemblies from second- and third-generation sequencing are merged to improve transcript completeness.

Tools:

- StringTie merge

Then transcript models are compared against reference annotations using:

- gffcompare

Purpose:

- identify known transcripts
- detect novel transcripts
- improve annotation consistency

---

### Step 5. Species-Specific Reference Transcriptome Construction

Merged transcript assemblies are integrated with reference annotations to generate species-specific transcriptomes.

Resource generated:

**OryzaSRT (Oryza Species Reference Transcriptome)**

This serves as the standardized transcriptome reference for each species.

---

### Step 6. Expression Quantification

Transcript abundance is quantified using StringTie in expression estimation mode.

Outputs:

- transcript abundance
- gene abundance
- expression matrices for downstream analysis

Used for:

- differential expression
- co-expression analysis
- pan-transcriptome expression profiling

---

### Step 7. ORF Prediction

Open reading frames are predicted using:

- ORFfinder

This identifies candidate coding regions within assembled transcripts.

Outputs:

- ORF prediction files
- amino acid sequences

---

### Step 8. Transcript Functional Classification

Based on ORF characteristics, transcripts are classified into:

#### Protein-coding genes (mRNA)

Criteria:

- longest ORF ≥ 100 aa

---

#### Small peptide coding genes

Criteria:

- all ORFs < 100 aa

---

#### Long non-coding RNAs (lncRNAs)

Criteria:

- no detectable coding ORF

---

This classification is performed using custom Python + shell scripts.

---

### Step 9. Sequence Extraction

Transcript or protein sequences are extracted for downstream analysis.

Generated resources:

- transcript FASTA
- protein FASTA
- classified transcript subsets

Applications:

- homology analysis
- orthology inference
- comparative genomics

---

### Step 10. Homologous Gene Identification

Protein-based orthology inference:

- OrthoFinder

Nucleotide similarity analysis:

- BLASTN

Purpose:

- identify homologous genes
- establish ortholog groups
- support cross-species transcriptome integration

---

### Step 11. Pan-Transcriptome Construction

Species-specific transcriptomes are integrated into a genus-level pan-transcriptome.

Resource generated:

**OryzaPRT (Oryza Pan-Reference Transcriptome)**

Gene categories include:

- Core genes
  - present across species
- Dispensable genes
  - present in subsets of species
- Species-specific genes
  - unique to individual species

Gene functional types:

- protein-coding genes
- small peptide genes
- non-coding genes

---

## Software Requirements

Recommended software versions:

| Tool | Purpose |
|------|---------|
| fastp | Quality control |
| HISAT2 | Short-read alignment |
| minimap2 | Long-read alignment |
| samtools | BAM processing |
| StringTie | Transcript assembly / quantification |
| gffcompare | Transcript annotation comparison |
| ORFfinder | ORF prediction |
| OrthoFinder | Ortholog detection |
| BLAST+ | Sequence similarity analysis |
| Python 3 | Custom scripts |
| awk / sed / bash | File processing |

---

## Directory Structure

Example structure:

```bash
project/
├── raw_data/
├── clean_data/
├── alignment/
│   ├── hisat2/
│   └── minimap2/
├── assembly/
│   ├── stringtie/
│   └── merged/
├── annotation/
├── quantification/
├── orf_prediction/
├── transcript_classification/
├── homolog_analysis/
├── pan_transcriptome/
└── scripts/
```

---

## Outputs

Main outputs include:

### Species-level resources

- species reference transcriptomes
- transcript annotations
- gene abundance matrices
- classified transcript sets

---

### Cross-species resources

- homologous gene clusters
- ortholog assignments
- pan-transcriptome gene sets
- expression-ready matrices

---

## Biological Significance

This workflow enables:

- cross-species transcriptome integration
- evolutionary comparison of gene expression
- identification of conserved regulatory modules
- discovery of species-specific adaptive transcripts
- non-coding RNA regulatory analysis
- molecular breeding resource development

---

## Future Extensions

Potential future modules:

- co-expression network construction
- module preservation analysis
- ncRNA regulatory network inference
- web database deployment
- transcript visualization platform integration

---

## Citation

If you use this pipeline in your research, please cite the associated study.

---

## Contact

For questions, collaboration, or technical discussion, please open an issue or contact the repository maintainer.
