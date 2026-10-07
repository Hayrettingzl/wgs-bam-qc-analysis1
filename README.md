# WGS BAM Quality Control Analysis

Post-alignment quality-control analysis of a public **NA12878 whole-genome sequencing test BAM** using Picard Tools and PowerShell.

The goal of this project was to evaluate alignment quality, mapping confidence, paired-end library characteristics, and regional high-quality coverage, and to investigate why a large fraction of reads had low MAPQ values.

## Dataset

- **Sample:** NA12878
- **Reference build:** GRCh37 / b37
- **Input BAM:** `CEUTrio.HiSeq.WGS.b37.NA12878.20.21.bam`
- **Data type:** restricted WGS test subset containing regions from chromosomes 20 and 21

The BAM itself is not stored in this repository because it is a large source file.

## Analysis workflow

1. Validate the BAM structure and metadata.
2. Calculate alignment summary metrics.
3. Convert BAM to SAM for MAPQ analysis.
4. Examine MAPQ distribution of primary mapped reads.
5. Evaluate paired-end insert-size distribution.
6. Calculate high-quality coverage using a matching GRCh37/b37 reference.
7. Compare chromosome 20 and chromosome 21 test regions.

## Tools

- **Picard Tools 3.5.0**
- **OpenJDK 17**
- **PowerShell**
- **GRCh37/b37 reference sequence**

The main commands used in the analysis are available in [`scripts/commands.txt`](scripts/commands.txt).

## Main results

| Metric | Result |
|---|---:|
| Total reads | 654,610 |
| Primary mapped reads | 583,884 |
| Mapping rate | 89.20% |
| MAPQ = 0 | 49.32% |
| MAPQ < 20 | 55.41% |
| MAPQ >= 20 | 44.59% |
| MAPQ >= 30 | 38.41% |
| Reads aligned in pairs | 96.45% |
| Improper pairs | 3.85% |
| Mean read length | 101 bp |
| Median insert size | 396 bp |
| Mean insert size | 391.59 bp |

## Key finding: regional MAPQ difference

The global MAPQ distribution initially suggested poor alignment quality because almost half of the primary mapped reads had MAPQ 0. However, regional analysis showed that this problem was highly localized to the chromosome 21 test region.

| Metric | chr20 | chr21 |
|---|---:|---:|
| Mean high-quality coverage | 71.55x | 18.43x |
| Median high-quality coverage | 73x | 2x |
| Excluded because of low MAPQ | 0.58% | 88.68% |
| >=1x coverage | 99.97% | 57.91% |
| >=10x coverage | 99.57% | 32.55% |
| >=20x coverage | 99.27% | 22.70% |
| >=30x coverage | 98.48% | 16.20% |
| >=50x coverage | 93.97% | 9.48% |

This demonstrates that a high number of mapped reads does not necessarily mean high-quality usable coverage. In the chr21 interval, many reads were mapped but were excluded from high-quality coverage because their mapping positions were uncertain.

## Figures

### MAPQ distribution
![MAPQ distribution](figures/Figure1_MAPQ_distribution.png)

### Insert-size distribution
![Insert-size distribution](figures/Figure2_Insert_size_distribution.png)

### chr20 vs chr21 coverage depth
![chr20 vs chr21 coverage](figures/Figure3_chr20_chr21_coverage_comparison.png)

### Coverage breadth by depth threshold
![Coverage breadth](figures/Figure4_Coverage_breadth_thresholds.png)

## Repository structure

```text
wgs-bam-qc-analysis/
|-- README.md
|-- .gitignore
|-- figures/
|-- results/
|-- scripts/
`-- report/
