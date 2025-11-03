# NGS Metagenomics Lab — Amplicon Workflow (16S/18S/ITS)

This repository documents a hands-on NGS lab project: from environmental sampling and amplicon library prep to Illumina MiSeq sequencing and downstream metagenomic analysis (QC, trimming, taxonomic classification, diversity indices, and result comparison across pipelines).

## Overview
- Samples: environmental soil, river water, compost, and spoiled potato juice.
- Targets: 16S (bacteria/archaea), 18S (eukaryotes), ITS (fungi).
- Outcomes: quality control, adapter/low-quality trimming, taxonomic profiles (Krona/Pavian), top taxa summaries, and Shannon diversity indices computed per sample and marker.
- Deliverable: full illustrated report with methods, results, and appendices.

## Repository layout
- `docs/NGS_Report.pdf` — full report (methods, results, figures, appendices).

## Wet-lab summary
- DNA extraction and cleanup adapted to matrix (soil, water, compost, potato), with mechanical and chemical lysis to remove inhibitors.
- Quantification and purity: NanoDrop; normalization prior to PCR.
- Amplicon PCR: Phusion Plus Master Mix, gel QC, bead-based purification (AMPure/SparQ).
- Index PCR (8 cycles), QC on gel, bead cleanup, pooling.
- Library quantification: NanoDrop + PicoGreen; normalization to target molarity.
- Denaturation and MiSeq run: PhiX spiking, NaOH denaturation, flowcell cleaning, cartridge loading.

## Bioinformatics pipeline
1. Quality control
   - Inspect per-base quality and base composition with FastQC; typical artifacts observed at the first bases (adapter remnants) and quality drop on reverse reads.
2. Trimming
   - Remove adapter-derived leading bases (~first 7 bases) and low-quality tails (Q < 20) with Cutadapt.
3. Taxonomic assignment
   - Classify reads with Kraken2 using appropriate databases:
     - SILVA for 18S
     - SILVA 138 16S for 16S
     - RefSeq Fungi genomes for ITS
   - Export interactive reports with Krona; visualize hierarchies and relative abundances with Pavian.
4. Community summaries
   - Aggregate to phylum and top-genus levels per sample and marker; produce stacked bar charts for quick comparison.
5. Diversity indices
   - Compute Shannon diversity per sample/marker; note that software-calculated indices (e.g., PAST) may differ slightly from manual calculations due to implementation details.
6. Cross-pipeline comparison
   - Contrast results from the “manual” pipeline vs a reference (e.g., Ath) to assess consistency in major phyla and identify database-driven discrepancies.

## Key findings (high level)
- Compost: abundant soil-associated bacteria (e.g., Arthrobacter) consistent with hydrocarbon/plant polymer degradation roles; 18S dominated by nematodes (e.g., Pelodera), reflecting active detrital turnover.
- Water: notable Flavobacterium presence in 16S; 18S highlighted diatom families (e.g., Stephanodiscaceae), typical of eutrophic freshwater systems.
- Soil: diverse Actinobacteriota; interest in Streptomyces taxa given ecological and antibiotic biosynthesis relevance; ITS dominated by Ascomycota.
- Potato juice: rich yeast community (18S/ITS), with fungi like Penicillium/Botrytis aligning with observed spoilage; bacterial profile includes Sphingomonas/Variovorax/Arthrobacter.

## Reproduce (minimal guidance)
- QC: run FastQC on raw FASTQ; confirm adapter/quality artifacts before trimming.
- Trimming: use Cutadapt to remove leading adapter remnants and Q<20 tails; re-check with FastQC.
- Classification: run Kraken2 against marker-appropriate databases (SILVA 16S/18S, RefSeq Fungi ITS).
- Visualization: generate Krona HTMLs; explore taxonomic trees in Pavian.
- Summaries: aggregate counts to phylum/top-10 genus per sample; plot stacked bars.
- Diversity: compute Shannon indices per sample and marker; document parameters/software versions for reproducibility.

## Notes and limitations
- Database taxonomy changes (naming/version) can alter assignments; report database versions used.
- Reverse reads often show lower quality; aggressive trimming improves classification but reduces read length.
- ITS results may be sparse for low-yield libraries; interpret with caution and consider re-sequencing thresholds.
