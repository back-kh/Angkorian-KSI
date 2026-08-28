# Angkorian-KSI

[![Paper](https://img.shields.io/badge/Paper-Springer-0645AD.svg)](https://doi.org/10.1007/978-3-032-36039-7_23)
[![Conference](https://img.shields.io/badge/Conference-ICDAR%202026-8A2BE2.svg)](https://icdar2026.org/)
[![DOI](https://img.shields.io/badge/DOI-10.1007%2F978--3--032--36039--7__23-0A7BBB.svg)](https://doi.org/10.1007/978-3-032-36039-7_23)

## A Multi-task Benchmark for Khmer Stone Inscription Analysis

This is the official repository for **Angkorian-KSI: A Multi-task Benchmark for Khmer Stone Inscription Analysis**, published in the proceedings of the **18th International Conference on Document Analysis and Recognition (ICDAR 2026)**.

> **Publication update:** The chapter was first published online on **24 August 2026** in *Document Analysis and Recognition – ICDAR 2026*, Lecture Notes in Computer Science, volume 16974, pages 387–404. Springer’s official bibliographic citation uses the publication year **2027**.

## Overview

Khmer stone inscriptions are essential records of Cambodia’s linguistic, historical, religious, and cultural heritage. Their computational analysis is substantially more difficult than the analysis of planar manuscripts because carved surfaces are affected by relief-induced shadows, erosion, biological growth, material loss, irregular illumination, complex stone textures, and script variation across historical periods.

**Angkorian-KSI** introduces the first multi-task benchmark designed specifically for automated Khmer stone inscription analysis. The benchmark was curated from in-situ captures collected across multiple sites within a UNESCO World Heritage archaeological region in Cambodia. It establishes a unified evaluation setting for structural detection, inscription binarization, and historical script-period classification.

## Benchmark Tasks

| Task | Objective | Input | Output |
| --- | --- | --- | --- |
| **KSI-LA** | Layout analysis and structural detection | Full inscription images | Text-region and text-line bounding boxes |
| **KSI-B** | Carved-text image binarization | Text-region and text-line crops | Binary foreground–background masks |
| **KSI-C** | Historical script-period classification | Full images and annotated text regions | Pre-Angkorian, Angkorian, or Post-Angkorian labels |

## Dataset at a Glance

| Benchmark component | Number |
| --- | ---: |
| Full inscription images | 230 |
| Annotated text regions | 760 |
| Annotated text lines | 2,733 |
| Binarization masks | 3,493 |
| Script-period labels | 534 |

These figures correspond to the benchmark reported in the published paper.

## Evaluation Protocol

- Dataset partitions are defined at the source-image level to prevent derived crops from crossing evaluation splits.
- Temple-level separation is applied where feasible to reduce site-specific leakage.
- Text-region and text-line samples inherit the split of their source image.
- Representative detection, binarization, convolutional, and transformer-based baselines are used to measure the domain gap between conventional document images and degraded carved-stone inscriptions.

The benchmark is intended to support reproducible comparison rather than to promote a single model architecture. The reported results show that methods developed for ordinary document images do not transfer reliably to carved Khmer inscriptions, motivating dedicated models and evaluation protocols for this domain.

## Paper

- **Title:** Angkorian-KSI: A Multi-task Benchmark for Khmer Stone Inscription Analysis
- **Authors:** Nimol Thuon, Jun Du, Ranysakol Thuon, and Panhapin Theang
- **Venue:** Document Analysis and Recognition – ICDAR 2026
- **Series:** Lecture Notes in Computer Science, volume 16974
- **Pages:** 387–404
- **Publisher:** Springer, Cham
- **DOI:** [10.1007/978-3-032-36039-7_23](https://doi.org/10.1007/978-3-032-36039-7_23)
- **First published online:** 24 August 2026

## Citation

If this benchmark or repository supports your research, please cite the published paper:

```bibtex
@inproceedings{thuon2027angkorian,
  author    = {Thuon, Nimol and Du, Jun and Thuon, Ranysakol and Theang, Panhapin},
  title     = {Angkorian-KSI: A Multi-task Benchmark for Khmer Stone Inscription Analysis},
  booktitle = {Document Analysis and Recognition -- ICDAR 2026},
  series    = {Lecture Notes in Computer Science},
  volume    = {16974},
  pages     = {387--404},
  publisher = {Springer},
  address   = {Cham},
  year      = {2027},
  doi       = {10.1007/978-3-032-36039-7_23}
}
```

## Project Team

| Contributor | Role and affiliation |
| --- | --- |
| **Dr. Nimol Thuon** | Project lead; NERC-SLIP, University of Science and Technology of China, Hefei, China |
| **Prof. Jun Du** | Co-author and corresponding author; NERC-SLIP, University of Science and Technology of China, Hefei, China |
| **Ranysakol Thuon** | Co-author; Paragon International University, Phnom Penh, Cambodia |
| **Panhapin Theang** | Co-author; Université Paris Cité, Paris, France |

## Data and Resource Access

Documentation, annotation formats, official split files, evaluation utilities, project updates, and a curated public subset are being prepared through the [Angkorian-AI project website](https://angkorianai.github.io/).

Because the benchmark contains imagery acquired at protected cultural-heritage sites and remains subject to site-level and data-sharing conditions, access to the complete dataset is considered upon reasonable academic request and approval. Publication of this repository does not by itself grant permission to redistribute restricted benchmark images.

## Acknowledgements

This research was supported by the National Natural Science Foundation of China under Grant No. **U25A20409**. The authors thank the Khmer epigraphic experts who assisted with annotation verification and the contributors from Cambodia, China, and France who supported data organization, documentation, and benchmark preparation.

## Research Scope

Angkorian-KSI is part of a broader effort to advance low-resource historical document analysis and the responsible digital preservation of Khmer and Southeast Asian cultural heritage.
