# GATK4 Somatic Variant Calling Pipeline

An automated, production-ready workflow for tumor-normal matched somatic variant calling following the **Broad Institute GATK Best Practices** (v4.x).

---

## 🧬 Overview

This repository implements a full somatic mutation identification pipeline using **GATK4 Mutect2**. It includes post-calling filtering steps to handle:
- **Orientation bias artifacts** (e.g., FFPE deamination errors via `LearnReadOrientationModel`).
- **Cross-sample contamination** (via `GetPileupSummaries` and `CalculateContamination`).
- **Germline variants & Panel of Normals (PoN)** filtering using gnomAD resources.

---

## ⚙️ Workflow Architecture

```text
[Tumor BAM] + [Normal BAM] 
        │
        ▼
   GATK Mutect2 ──► Unfiltered VCF & F1R2 Tar
        │
        ├─► LearnReadOrientationModel ──► Read Orientation Model
        ├─► CalculateContamination   ──► Contamination Table
        │
        ▼
 GATK FilterMutectCalls ──► Final Somatic VCF (Pass-only calls)
