# CT–CT Liver Registration

**Medical Image Processing and AI Research Internship | Perfint Healthcare, Chennai**

Developed a literature-grounded, intensity-based deformable registration pipeline for aligning contrast-enhanced CT (CECT) with non-contrast CT for liver image-guided ablation planning.

> **Source code:** Confidential — not included in this repository.  
> **Technical report:** Available upon request, subject to confidentiality approval. 📄 [Request access here](YOUR_DRIVE_LINK)

---

## The Problem

During CT-guided liver tumour ablation, the procedure is commonly performed using non-contrast CT, while diagnostic contrast-enhanced CT provides clearer visualization of lesions and hepatic vasculature.

Registering the CECT to the non-contrast CT allows diagnostically useful anatomy to be mapped into the image space used for procedural guidance.

CT–CT registration is challenging because:

1. **Contrast variation** — vessels and lesions can have substantially different appearances between contrast-enhanced and non-contrast scans.
2. **Respiratory and positional deformation** — the liver changes position and shape between acquisitions.
3. **Large-volume deformable registration** — full abdominal CT volumes can make dense deformable optimization computationally and memory intensive.

---

## My Contribution

- Conducted a **focused literature review** of liver CT registration methods to identify appropriate similarity metrics, transformation models, and validation practices.
- Designed a **single, phase-independent intensity-based pipeline** using Mattes Mutual Information, avoiding phase-specific branching and segmentation masks.
- Implemented a staged **rigid → affine → B-spline deformable registration** pipeline using multi-resolution optimization.
- Diagnosed and addressed a **memory bottleneck in full-volume B-spline registration** by using a bounded control grid and a lighter deformable-stage pyramid.
- Implemented an optional **GPU diffeomorphic deformable stage** using a stationary velocity field and local normalized cross-correlation.
- Built an integrated **GUI-based desktop application** with live stage progress, metric visualization, result inspection, and landmark-based validation.
- Implemented **Target Registration Error (TRE)** validation using corresponding vessel-bifurcation landmarks in physical coordinates.

---

## Pipeline

**CT inputs → Preprocessing → Rigid → Affine → Deformable → Fusion / Inspection → Landmark TRE Validation**

### Preprocessing
- Hounsfield-unit windowing
- Common isotropic resampling
- Full-volume registration without segmentation masks

### Linear Registration
- 3D Euler rigid alignment
- Affine refinement for scaling and shear
- Multi-resolution optimization

### Deformable Registration

Two deformable engines are available:

- **B-spline + Mattes Mutual Information** — CPU-based default and literature-grounded path
- **GPU diffeomorphic + local NCC** — optional experimental path

The GPU implementation uses a stationary velocity field integrated through scaling-and-squaring. The CPU B-spline implementation remains the default validated path.

---

## Technologies

**Python** · **SimpleITK** · **PyQt6** · **NumPy** · **PyTorch (CUDA)** · **Mattes Mutual Information** · **B-spline Deformable Registration** · **Diffeomorphic Registration** · **Target Registration Error (TRE)**

---

## Results

- The pipeline successfully completed the full **rigid → affine → deformable** workflow on an initial abdominal CT pair.
- Registration converged monotonically across the staged optimization.
- The resulting registration preserved **sub-4 mm landmark agreement** on the initial validation case.
- The deformable stage was substantially more computationally expensive than the preceding linear stages, motivating the memory-bounded implementation.
- The initial evaluation used only a small number of landmarks and therefore **does not support a generalized accuracy claim**.
- The experimental GPU deformable stage was **not used for the reported clinical result** and remains subject to further validation.

---

## Key Engineering Insight

> **A deformable registration pipeline is only useful if its optimization is computationally practical and its accuracy is independently validated.**

The project therefore combined three components:

**literature-grounded similarity metric → memory-bounded deformable optimization → landmark-based TRE validation**

rather than treating successful optimizer convergence alone as evidence of registration accuracy.

---

## Validation

Registration accuracy is evaluated using **Target Registration Error (TRE)** on corresponding anatomical vessel-bifurcation landmarks.

Landmarks are stored in physical coordinates, and TRE is computed from the dense displacement field in millimetres.

The validation interface reports:

- Mean TRE
- Median TRE
- Maximum TRE
- TRE before registration
- TRE after registration

This follows the literature's use of vessel-bifurcation landmarks as a reference accuracy measure for liver deformable image registration.

---

## Confidentiality

Source code, clinical data, patient images, proprietary algorithms, and internal implementation details are not included.

This repository is a **documentation-only portfolio artifact** describing work conducted during my internship at Perfint Healthcare.

---

## Technical Report

A detailed technical report describing the literature review, methodology, implementation, experiments, and validation is available upon request, subject to confidentiality approval.

📄 **[Request access to the technical report](YOUR_DRIVE_LINK)**

---

## Disclaimer

This repository is a portfolio-level technical overview and is **not a clinical software release or medical device**.
