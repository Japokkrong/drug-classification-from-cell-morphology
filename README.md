# Learning Phenotypic Representations from Cell Images

![Cell Image Example](https://via.placeholder.com/800x400?text=Fluorescence+Microscopy+Example)
## Project Overview
This project explores the application of self-supervised and weakly-supervised learning to extract phenotypic representations from high-content fluorescent microscopy images. Building upon the **WS-DINO** framework, we leverage weak label information (such as treatment and compound metadata) to improve mechanism of action (MoA) prediction without requiring single-cell cropping as a pre-processing step.

We benchmark the original DINO Vision Transformer (ViT) against a Weakly-Supervised DINO (WS-DINO) approach and demonstrate state-of-the-art performance using an improved **DINOv3** architecture.

## Dataset: BBBC021
The project utilizes the **BBBC021 Compound Profiling Dataset**.
* **Cell Line:** MCF-7 breast cancer cells treated for 24 hours.
* **Classes:** 13 Mechanisms of Action (MoA) including Epithelial, Protein synthesis, Aurora kinase inhibitors, and DNA damage.
* **Data Volume:** A subset of 2,528 images is used for training and testing.

### Image Channels
The data consists of 3-channel fluorescence microscopy images:

| Channel | Stain / Marker | Target Structure | Biological Meaning |
| :--- | :--- | :--- | :--- |
| **DAPI** | Hoechst 33342 | Nuclei | Reveals shape, size, and position of the nucleus. |
| **Tubulin** | Alexa Fluor 488 | Microtubules | Stains internal fibrous cytoskeleton structures. |
| **Actin** | Alexa Fluor 594 | Actin filaments | Highlights the boundary and outline of the cell. |

## Methodology

### Pre-processing
We apply a rigorous CellProfiler and Python pipeline:
1.  **Illumination Correction:** Smoothing function with a filter size of 320 pixels.
2.  **Resize:** Bicubic interpolation to $640 \times 512$ pixels.
3.  **Normalization:** Pixel value cut-off at 10,000, scaled to mean=0 and sd=1.
4.  **Batch Correction:** Typical Variation Normalization (TVN) applied to correct batch effects.

### Model Architectures
1.  **DINO (Baseline):** A self-supervised distillation approach using a Vision Transformer (ViT-Small) backbone initialized with ImageNet weights.
2.  **WS-DINO:** Incorporates "weak labels" (Unique Compounds or Treatments). Crops from images sharing the same metadata are treated as positive pairs.
3.  **DINOv3:** An improved architecture incorporating Gram Anchoring to enhance segmentation and representation learning.

![ViT Architecture](https://via.placeholder.com/800x400?text=Vision+Transformer+Architecture)
## Experimental Results
Models were evaluated based on **Not-Same-Compound-and-Batch (NSCB) Accuracy**.

| Model | NSCB Accuracy | Improvement |
| :--- | :--- | :--- |
| **DINO (Baseline)** | 20.6% | - |
| **WS-DINO** | 30.4% | ~47% over Baseline |
| **DINOv3 (DAPI only)** | 54.3% | - |
| **DINOv3 (3 Channels)** | **59.8%** | **+189.47% over Baseline** |

### Key Findings
The final **DINOv3 model (3 channels)** achieved the highest NSCB accuracy of **0.5978**. This represents a **96.43% improvement** over the interim weak-label baseline and a substantial gain over the original DINO baseline.

## Visualization
We utilized t-SNE clustering and RGB feature map visualizations (Layer-3 vs Layer-6) to demonstrate that the DINOv3 model learns more structurally meaningful phenotypic profiles compared to the vanilla DINO baseline.

![t-SNE Clustering](https://via.placeholder.com/800x400?text=t-SNE+Feature+Clustering)
## Citation

```

@article {10.48550/arXiv.2209.07819,

author = {Cross-Zamirski, Jan and Mouchet, Elizabeth and Williams, Guy and Sch{\"o}nlieb, Carola-Bibiane and Turkki, Riku and Wang, Yinhai},

title = {Self-Supervised Learning of Phenotypic Representations from Cell Images with Weak Labels},

year = {2022},

doi = {https://doi.org/10.48550/arXiv.2209.07819},

journal = {arXiv Preprint arXiv:2209.07819}

}
