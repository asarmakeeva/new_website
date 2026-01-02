---
title: "Projects"
type: "page"
draft: false
url: "/projects/"
weight: 3
---

## T-SYNTH: Synthetic Breast Imaging Dataset

**Role:** Lead Co-Contributor  
**Context:** U.S. Food and Drug Administration (FDA)

**T-SYNTH** is a large-scale, open-source dataset of **synthetic breast imaging data** designed to support training and evaluation of detection and segmentation models in medical imaging.

The dataset contains paired **2D digital mammography (DM)** and **3D digital breast tomosynthesis (DBT)** images generated using **knowledge-based, physics-driven simulations**. It complements the previously released **M-SYNTH** dataset and addresses key limitations of real-world annotated medical data.

**Key features**
- High-resolution DM and DBT image pairs  
- Pixel-level **segmentation masks** and **bounding boxes** for lesions and breast tissues  
- Designed for **detection and segmentation** tasks  
- Open-source and publicly available

**Key finding**  
In our experiments, augmenting real training data with synthetic samples from T-SYNTH **improved lesion detection performance**, particularly in scenarios with limited or imbalanced real data.

**Collaborators**  
Christopher Wiedeman, Elena Sizikova, Daniil Filienko, Miguel Lago, Jana Delfino, Aldo Badano

**Links**  
- 📄 [Paper](https://lnkd.in/gy5C7hes)  
- 🤖 [Code (GitHub)](https://lnkd.in/gmvwGFeC)  
- 🧠 [Dataset (Hugging Face)](https://lnkd.in/guPYZzGU)  
- 📚 [Related dataset: M-SYNTH](https://lnkd.in/ga8pnBfB)
