# Angkorian-KSI

[![Paper](https://img.shields.io/badge/Paper-Springer-0645AD.svg)](https://doi.org/10.1007/978-3-032-36039-7_23)
[![Conference](https://img.shields.io/badge/Conference-ICDAR%202026-8A2BE2.svg)](https://icdar2026.org/)
[![DOI](https://img.shields.io/badge/DOI-10.1007%2F978--3--032--36039--7__23-0A7BBB.svg)](https://doi.org/10.1007/978-3-032-36039-7_23)
[![KSI-Small](https://img.shields.io/badge/KSI--Small-Coming%20Soon-F59E0B.svg)](#ksi-small-coming-soon)
[![Access](https://img.shields.io/badge/Full%20Benchmark-Restricted-B91C1C.svg)](#data-access-and-use-restrictions)

## A Multi-task Benchmark for Khmer Stone Inscription Analysis

Official repository for **Angkorian-KSI: A Multi-task Benchmark for Khmer Stone Inscription Analysis**, published in the proceedings of the **18th International Conference on Document Analysis and Recognition (ICDAR 2026, Vienna, Austria)**.

> **Publication status:** First published online on **24 August 2026** in *Document Analysis and Recognition - ICDAR 2026*, Lecture Notes in Computer Science, volume 16974, pages 387-404. Springer’s official bibliographic citation uses the publication year **2027**.

**Paper:** [Springer](https://link.springer.com/chapter/10.1007/978-3-032-36039-7_23) | [DOI](https://doi.org/10.1007/978-3-032-36039-7_23) | [Angkorian-AI](https://angkorianai.github.io/)

## Overview

Khmer stone inscriptions are primary records of Cambodia’s linguistic, historical, religious, and cultural heritage. Automated analysis is difficult because carved stone differs fundamentally from planar documents: relief-induced shadows, erosion, biological growth, surface damage, irregular illumination, complex stone texture, and gradual script evolution all obscure the inscription structure.

**Angkorian-KSI** is the first multi-task benchmark designed specifically for automated Khmer stone inscription analysis. It was curated from in-situ captures collected across **10 temple sites** within a UNESCO World Heritage archaeological region in Cambodia and supports three connected tasks:

- **KSI-LA:** layout analysis of text regions, text lines, and applicable non-text elements.
- **KSI-B:** binary text-mask extraction from degraded stone surfaces.
- **KSI-C:** historical script-period classification across Pre-Angkorian, Angkorian, and Post-Angkorian periods.

## Benchmark Workflow

![Angkorian-KSI benchmark workflow](assets/workflow.png)

*Figure 3 from the paper. The workflow connects in-situ acquisition and annotation with layout analysis, binarization, and historical script-period classification.*

The annotation pipeline uses three quality-control stages:

1. Automatic initialization using classical image processing and detection-assisted preprocessing.
2. Manual refinement with polygon-based and pixel-accurate annotation tools.
3. Expert verification of degraded, ambiguous, or partially broken inscriptions.

## Benchmark Tasks

| Task | Input | Ground truth | Primary evaluation |
| --- | --- | --- | --- |
| **KSI-LA** | Full inscription images | Text-region, text-line, and applicable non-text boxes | Per-class AP@0.5 and mAP@0.5 |
| **KSI-B** | Text-region and text-line crops | Pixel-level binary masks | Precision, recall, F1, PSNR, and DRD |
| **KSI-C** | Full images and text-region crops | Historical period labels | Accuracy and macro-F1 |

## Dataset Statistics

| Component | Quantity | Annotation type |
| --- | ---: | --- |
| Full inscription images | 230 | Layout and metadata |
| Annotated text regions | 760 | Polygon masks |
| Annotated text lines | 2,733 | Polygon masks |
| Binarization masks | 3,493 | Binary masks |
| Script-period labels | 534 | Page/region labels |
| Temple sites | 10 | Site metadata |

### Page-level split

| Split | Pre-Angkorian | Angkorian | Post-Angkorian | Total |
| --- | ---: | ---: | ---: | ---: |
| Train (70%) | 28 | 112 | 21 | 161 |
| Validation (15%) | 6 | 24 | 4 | 34 |
| Test (15%) | 6 | 24 | 5 | 35 |
| **All** | **40** | **160** | **30** | **230** |

Splits are assigned at the full-image level. Temple-level separation is enforced whenever sufficient samples are available, and every derived region or line crop inherits the split of its source image.

## Annotation Examples

![Angkorian-KSI annotation examples](assets/annotations.png)

*Figure 4 from the paper. Representative structural, binarization, and script-period annotations.*

Ambiguous regions are handled conservatively. Annotators do not reconstruct unreadable characters or invent missing stroke boundaries when the available visual evidence is insufficient.

## Experimental Setup

- **Hardware:** two NVIDIA RTX A6000 GPUs.
- **Optimization:** AdamW, initial learning rate 1 × 10^-4, weight decay 1 × 10^-4, batch size 16, and 100 epochs.
- **Model selection:** validation mAP@0.5 for KSI-LA, validation F1 for KSI-B, and validation macro-F1 for KSI-C.
- **Binarization training:** pixel-wise binary cross-entropy, optionally combined with Dice loss.

## Benchmark Results

All results below are reproduced from the published paper and use the official Angkorian-KSI splits.

### KSI-LA: Layout Analysis

| Method | Text-region AP@0.5 | Text-line AP@0.5 | Other AP@0.5 | mAP@0.5 |
| --- | ---: | ---: | ---: | ---: |
| YOLOv8 | 80.12 | 60.34 | 40.25 | 60.24 |
| YOLOv11 | 84.78 | 67.91 | 48.37 | 67.02 |
| Faster R-CNN | 88.43 | 72.18 | 52.46 | 71.02 |
| **DETR** | **92.67** | **79.84** | **57.63** | **76.71** |

DETR obtains the strongest overall layout result, while text-line and non-text detection remain more difficult because of dense line spacing, shadows, adjacent structures, and decorative carvings.

![Qualitative layout-detection results](assets/layout-results.png)

*Figure 5 from the paper. Qualitative comparison of YOLOv11, Faster R-CNN, DETR, and ground truth.*

### KSI-B: Binarization

| Method | Precision ↑ | Recall ↑ | F1 ↑ | PSNR ↑ | DRD ↓ |
| --- | ---: | ---: | ---: | ---: | ---: |
| Otsu | 0.18 | 0.51 | 0.23 | 12.0 | 18.7 |
| Sauvola | 0.22 | 0.44 | 0.28 | 12.6 | 17.5 |
| DAE | 0.61 | 0.53 | 0.57 | 17.8 | 8.9 |
| DE-GAN | 0.78 | **0.72** | 0.75 | 21.9 | 4.6 |
| **PALM-GAN** | **0.80** | **0.72** | **0.77** | **23.4** | **4.2** |

Learning-based approaches substantially outperform classical thresholding. PALM-GAN achieves the best F1, PSNR, and DRD, although faint strokes in heavily eroded regions remain challenging.

![Qualitative binarization results](assets/binarization-results.png)

*Figure 6 from the paper. Comparison of Otsu, Sauvola, DAE, DE-GAN, PALM-GAN, and ground truth.*

### KSI-C: Script-period Classification

| Method | Page accuracy ↑ | Page macro-F1 ↑ | Region accuracy ↑ | Region macro-F1 ↑ |
| --- | ---: | ---: | ---: | ---: |
| ResNet50 | 0.82 | 0.76 | 0.84 | 0.80 |
| **EfficientNetB0** | **0.86** | **0.79** | **0.89** | **0.82** |
| ViT-B/16 | 0.71 | 0.67 | 0.75 | 0.71 |
| Swin-T | 0.76 | 0.71 | 0.80 | 0.74 |

EfficientNetB0 performs best at both page and region levels. Region-level evaluation is consistently stronger, indicating that full-image background texture and layout variation add noise to historical period recognition.

#### Region-level per-class F1

| Method | Pre-Angkorian | Angkorian | Post-Angkorian |
| --- | ---: | ---: | ---: |
| ResNet50 | 0.74 | 0.85 | 0.81 |
| **EfficientNetB0** | **0.77** | **0.88** | **0.83** |
| ViT-B/16 | 0.63 | 0.78 | 0.71 |
| Swin-T | 0.68 | 0.81 | 0.73 |

![Qualitative script-classification results](assets/classification-results.png)

*Figure 7 from the paper. Correct and incorrect predictions across CNN and transformer-based classifiers.*

## Main Findings

- Carved stone creates a substantial domain gap relative to conventional ink-based document images.
- Global structural modeling benefits layout analysis; DETR achieves the strongest KSI-LA result.
- Learning-based binarization is substantially more robust than classical thresholding under stone texture and relief shading.
- CNN classifiers outperform the tested transformer classifiers in the limited-data KSI-C setting.
- Errors frequently involve heavily degraded samples and adjacent historical periods with gradual paleographic transitions.

## Release Status

| Resource | Status | Access |
| --- | --- | --- |
| Published paper | Available | [Springer / DOI](https://doi.org/10.1007/978-3-032-36039-7_23) |
| README and benchmark documentation | Available | This repository |
| **Angkorian-KSI-Small (KSI-Small)** public test sample | **Coming soon** | Limited public research sample |
| Evaluation code and official split files | Coming soon | This repository / Angkorian-AI |
| Complete Angkorian-KSI benchmark | Restricted | Approved cultural-heritage research only |

## KSI-Small: Coming Soon

**Angkorian-KSI-Small (KSI-Small)** will provide a limited public test sample from the benchmark so researchers can inspect the data structure, test input/output formats, and evaluate the basic workflow without receiving the restricted complete collection.

> [!CAUTION]
> **KSI-Small is an intentionally small, data-limited sample.** It does not represent the scale, diversity, or statistical reliability of the complete Angkorian-KSI benchmark. The results below are preliminary and diagnostic; they should not be treated as definitive model rankings or directly compared with the full-benchmark results reported above.

### Preliminary KSI-B Results on Angkorian-KSI-Small

| Method | Track | F ↑ | pseudo-F ↑ | PSNR ↑ | DRD ↓ | Seconds/image ↓ |
| --- | --- | ---: | ---: | ---: | ---: | ---: |
| Otsu | Train-tuned | 0.155 | 0.146 | 2.867 | 102.43 | **0.0077** |
| Sauvola | Train-tuned | 0.141 | 0.134 | 6.163 | 47.39 | 0.0190 |
| Wolf-Jolion | Train-tuned | 0.140 | 0.128 | 6.367 | 43.14 | 0.0128 |
| SAE/DAE | Zero-shot | 0.219 | 0.205 | 5.202 | 62.70 | 0.6111 |
| DP-LinkNet | Zero-shot | 0.088 | 0.097 | **8.553** | **22.85** | 0.4786 |
| DE-GAN | Zero-shot | 0.3550 | **0.347** | 3.807 | 89.52 | 3.0661 |
| PALM-GAN | Zero-shot | **0.3611** | 0.341 | 3.591 | 90.68 | 2.8498 |
| DocEnTr | Zero-shot | 0.130 | 0.129 | 8.378 | 24.76 | 4.3325 |
| DocDiff pilot | DIBCO zero-shot | 0.203 | 0.199 | 0.695 | 165.92 | 5.4442 |
| SAE/DAE | KSI fine-tuned | 0.163 | 0.165 | 6.630 | 41.53 | 0.6107 |
| DocDiff pilot | KSI fine-tuned | 0.227 | 0.227 | 1.526 | 135.56 | 5.4155 |

**Metric directions:** higher values are better for F, pseudo-F, and PSNR; lower values are better for DRD and seconds per image. Bold indicates the best reported value for each metric.

#### Track definitions

- **Train-tuned:** classical method parameters were selected using the available KSI-Small training portion.
- **Zero-shot:** the pretrained method was evaluated without KSI-Small fine-tuning.
- **DIBCO zero-shot:** the DocDiff pilot was evaluated in its DIBCO-domain setting without KSI-Small fine-tuning.
- **KSI fine-tuned:** the model was adapted using the limited KSI-Small training portion.

#### Preliminary observations

- PALM-GAN records the highest F score (**0.3611**), while DE-GAN obtains the highest pseudo-F (**0.347**).
- DP-LinkNet obtains both the highest PSNR (**8.553**) and the lowest DRD (**22.85**).
- Otsu is the fastest method at **0.0077 seconds per image**.
- No method dominates every metric, reflecting different sensitivity to stroke preservation, background suppression, and image fidelity.
- KSI fine-tuning does not consistently improve performance in this pilot. This may reflect the very limited training sample and should not be interpreted as a general conclusion about fine-tuning on the complete benchmark.

Because KSI-Small contains limited data and restricted coverage of sites, historical periods, degradation patterns, and surface conditions, its scores may have high variance. The sample is intended for pipeline verification, format testing, and preliminary reproducibility checks—not for training data-hungry models or drawing broad performance conclusions.

The sample will be released separately with its own permitted-use notice. Availability of KSI-Small will **not** imply open access to, or redistribution rights for, the complete Angkorian-KSI benchmark.

## Data Access and Use Restrictions

> [!IMPORTANT]
> **The complete Angkorian-KSI benchmark is not an open or unrestricted dataset. Access and use are strictly limited to approved, non-commercial cultural-heritage research.**

Because the collection contains imagery acquired at protected heritage sites and is subject to site-level, ethical, and data-sharing conditions:

- Full benchmark access requires a reasonable academic request, research-purpose review, and explicit approval from the project team.
- Approved use must remain within the stated cultural-heritage research scope.
- Commercial use, resale, public re-hosting, and redistribution of the complete images or annotations are not permitted.
- Access approval does not transfer ownership or grant permission for any use beyond the approved project.
- Researchers must follow any additional attribution, reporting, security, and deletion conditions provided with approved access.

Public visibility of this repository, its documentation, paper figures, results, or KSI-Small does **not** grant access to the full benchmark and does **not** waive these restrictions.

Access announcements, documentation, and permitted public resources will be provided through the [Angkorian-AI project website](https://angkorianai.github.io/).

## Paper Information

- **Title:** Angkorian-KSI: A Multi-task Benchmark for Khmer Stone Inscription Analysis
- **Authors:** Nimol Thuon, Jun Du, Ranysakol Thuon, and Panhapin Theang
- **Venue:** Document Analysis and Recognition - ICDAR 2026
- **Series:** Lecture Notes in Computer Science, volume 16974
- **Pages:** 387-404
- **Publisher:** Springer, Cham
- **First published online:** 24 August 2026
- **DOI:** [10.1007/978-3-032-36039-7_23](https://doi.org/10.1007/978-3-032-36039-7_23)

## Citation

If Angkorian-KSI supports your research, please cite the published paper:

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

| **Dr. Nimol Thuon** | Project lead;  nimol.thuon@gmail.com|


## Acknowledgements

This research was supported by the National Natural Science Foundation of China under Grant No. **U25A20409**. The authors thank the Khmer epigraphic experts who assisted with annotation verification and the contributors from Cambodia, China, and France who supported data organization, documentation, and benchmark preparation.

## Research Scope

Angkorian-KSI is part of a broader effort to advance low-resource historical document analysis and the responsible digital preservation of Khmer and Southeast Asian cultural heritage.
