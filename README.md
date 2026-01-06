# Learning Phenotypic Representations from Cell Images

![Cell Image Example](https://via.placeholder.com/800x400?text=Fluorescence+Microscopy+Example)
## Project Overview
[cite_start]This project explores the application of self-supervised and weakly-supervised learning to extract phenotypic representations from high-content fluorescent microscopy images[cite: 2, 6, 24]. [cite_start]Building upon the **WS-DINO** framework, we leverage weak label information (such as treatment and compound metadata) to improve mechanism of action (MoA) prediction without requiring single-cell cropping as a pre-processing step[cite: 24, 26, 27].

[cite_start]We benchmark the original DINO Vision Transformer (ViT) against a Weakly-Supervised DINO (WS-DINO) approach and demonstrate state-of-the-art performance using an improved **DINOv3** architecture[cite: 25, 26, 199].

## Dataset: BBBC021
[cite_start]The project utilizes the **BBBC021 Compound Profiling Dataset**[cite: 28].
* [cite_start]**Cell Line:** MCF-7 breast cancer cells treated for 24 hours[cite: 29, 30].
* [cite_start]**Classes:** 13 Mechanisms of Action (MoA) including Epithelial, Protein synthesis, Aurora kinase inhibitors, and DNA damage[cite: 31, 32, 33, 34, 40].
* [cite_start]**Data Volume:** A subset of 2,528 images is used for training and testing[cite: 46].

### Image Channels
[cite_start]The data consists of 3-channel fluorescence microscopy images[cite: 52]:

| Channel | Stain / Marker | Target Structure | Biological Meaning |
| :--- | :--- | :--- | :--- |
| **DAPI** | Hoechst 33342 | Nuclei | [cite_start]Reveals shape, size, and position of the nucleus[cite: 54]. |
| **Tubulin** | Alexa Fluor 488 | Microtubules | [cite_start]Stains internal fibrous cytoskeleton structures[cite: 54]. |
| **Actin** | Alexa Fluor 594 | Actin filaments | [cite_start]Highlights the boundary and outline of the cell[cite: 54]. |

## Methodology

### Pre-processing
[cite_start]We apply a rigorous CellProfiler and Python pipeline[cite: 72, 73]:
1.  [cite_start]**Illumination Correction:** Smoothing function with a filter size of 320 pixels[cite: 74].
2.  [cite_start]**Resize:** Bicubic interpolation to $640 \times 512$ pixels[cite: 75].
3.  [cite_start]**Normalization:** Pixel value cut-off at 10,000, scaled to mean=0 and sd=1[cite: 77].
4.  [cite_start]**Batch Correction:** Typical Variation Normalization (TVN) applied to correct batch effects[cite: 82].

### Model Architectures
1.  [cite_start]**DINO (Baseline):** A self-supervised distillation approach using a Vision Transformer (ViT-Small) backbone initialized with ImageNet weights[cite: 25, 88].
2.  **WS-DINO:** Incorporates "weak labels" (Unique Compounds or Treatments). [cite_start]Crops from images sharing the same metadata are treated as positive pairs[cite: 188, 189].
3.  [cite_start]**DINOv3:** An improved architecture incorporating Gram Anchoring to enhance segmentation and representation learning[cite: 199].

![ViT Architecture](https://via.placeholder.com/800x400?text=Vision+Transformer+Architecture)
## Experimental Results
[cite_start]Models were evaluated based on **Not-Same-Compound-and-Batch (NSCB) Accuracy**[cite: 144].

| Model | NSCB Accuracy | Improvement |
| :--- | :--- | :--- |
| **DINO (Baseline)** | 20.6% | - |
| **WS-DINO** | 30.4% | ~47% over Baseline |
| **DINOv3 (DAPI only)** | 54.3% | - |
| **DINOv3 (3 Channels)** | **59.8%** | **+189.47% over Baseline** |

[cite_start]*(Source: [cite: 446, 448])*

### Key Findings
[cite_start]The final **DINOv3 model (3 channels)** achieved the highest NSCB accuracy of **0.5978**[cite: 449]. [cite_start]This represents a **96.43% improvement** over the interim weak-label baseline[cite: 448].

## Visualization
[cite_start]We utilized t-SNE clustering and RGB feature map visualizations (Layer-3 vs Layer-6) to demonstrate that the DINOv3 model learns more structurally meaningful phenotypic profiles compared to the vanilla DINO baseline[cite: 206, 229].

![t-SNE Clustering](https://via.placeholder.com/800x400?text=t-SNE+Feature+Clustering)
## Citation
If you find this work useful, please consider citing our paper:
