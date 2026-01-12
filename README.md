See the full slide deck in [docs/xct_phantom_som_cnn_demo.pdf](docs/xct_phantom_som_cnn_demo.pdf).
# Label-Free XCT Phantom Defect Detection with SOM–CNN

Label-free, interpretable defect detection on X-ray computed tomography (XCT)
phantoms using a physics-aware SOM teacher and a lightweight CNN student.

This demo shows how simulated XCT phantoms can be used to learn reusable SOM
centroids, which then transfer to real XCT data without any manual voxel labels.

`docs/xct_phantom_som_cnn_demo.pdf` contains the full slide deck and figures.

---

## Problem

- Industrial XCT inspection often relies on manual interpretation or
  heavily supervised segmentation models.
- Annotated 3D voxel labels are expensive and impractical at scale.
- Goal: **detect and segment internal defects in XCT volumes without any
  manual voxel labels**, using a self-supervised SOM teacher.

---

## Data

- **Phantom setup:** synthetic XCT phantoms with circular voids/defects
  embedded in a homogeneous background.
- **Inputs:** reconstructed 2D slices or 3D volumes from simulated
  projections (FBP-style reconstruction).
- **Task:** identify and segment internal defect regions in a fully
  label-free manner.

---

## Method (SOM → CNN student)

1. **Physics-aware feature extraction**

   Each voxel (or pixel for 2D slices) is mapped into a low-dimensional,
   physics-aware feature vector capturing:

   - local intensity and contrast,
   - edge sharpness and curvature,
   - neighborhood statistics.

2. **PCA + SOM clustering**

   - PCA reduces the feature dimension while preserving the main
     variance related to material vs defect contrast.
   - A Self-Organizing Map (SOM) is trained on phantom data to form a
     small number of interpretable clusters (e.g., background, defect,
     edges).

3. **Cluster interpretation**

   - Clusters are analyzed via their centroid features and spatial
     patterns.
   - One cluster is identified as the **defect class** (round voids),
     others as background and transition zones.
   - Radar plots (see PDF) visualize how each cluster differs in terms
     of intensity, sharpness, and other features.

4. **CNN student (optional)**

   - A lightweight CNN/U-Net is trained using SOM labels as supervision
     (teacher–student).
   - The student learns a smooth, voxel-level segmentation that matches
     the SOM teacher but is faster at inference time.

Key properties:

- **Fully label-free:** no human voxel annotation is needed.
- **Interpretable:** SOM clusters and radar plots provide clear
  semantics for "defect" vs "background".
- **Transferable:** SOM centroids learned on phantoms can be reused on
  real XCT scans with similar imaging physics.

---

## Results (see PDF)

Highlights from `docs/xct_phantom_som_cnn_demo.pdf`:

- A single SOM trained on simulated phantoms cleanly separates defect
  regions as a dedicated cluster.
- The corresponding CNN student reproduces the SOM segmentation at
  voxel-level with high overlap (IoU/Dice) while being cheaper to run.
- The same SOM centroids can serve as **initial codebooks** when moving
  from simulated to real XCT data.

---

## Status

This repository currently serves as a **documentation/demo hub** for the
XCT phantom SOM–CNN pipeline. Code and runnable notebooks will be added
progressively as the framework is cleaned and released.

---
# xct-phantom-som-cnn-demo
