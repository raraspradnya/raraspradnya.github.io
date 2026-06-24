---
layout: page
title: GAN-Based Makeup Transfer 💄
description: Fine-tuning and retraining BeautyGAN for realistic makeup transfer across diverse skin tones
img: /assets/img/3_gan.jpg
importance: 3
category: work
---

Most published makeup transfer systems only work optimally for fair-skinned faces — a significant gap given how makeup interacts differently with varying skin tones, undertones, and facial features. This project addresses that gap through a full retraining of **BeautyGAN** using warm weights, a new dataset, custom ground truth generation, modified loss functions, and updated generator architectures.

---

### The Problem with Existing Approaches

Standard makeup transfer models are trained on datasets skewed toward lighter skin, and their ground-truth generation pipelines don't account for how pigmentation, contrast, and texture vary across skin tones. The result is models that fail or produce unnatural outputs on darker skin.

---

### What We Built

This is not a fine-tune-and-done adaptation — we rebuilt the training pipeline from the ground up:

**New dataset** — Collected and preprocessed a new dataset covering a wider range of skin tones, with image scraping, face alignment, and normalization to ensure training quality.

**Custom ground truth generation** — Built a ground truth pipeline using **warping** and **Poisson blending** (seamless cloning), producing natural makeup boundaries that generalize across skin tones — replacing the original pipeline which failed on darker complexions.

**Modified loss functions** — Reworked the loss formulation to better optimize for perceptual quality across diverse skin tone distributions, not just the narrow range the original model was trained on.

**Updated architecture** — Experimented with **ResNet** and **Dilated ResNet** generators. Dilated convolutions allow the model to capture broader context without losing spatial resolution, improving blending quality at skin level.

**Warm weight retraining** — Used BeautyGAN's pretrained weights as a starting point, then retrained the full model end-to-end on the new dataset — preserving learned representations while adapting to the expanded skin tone distribution.

---

### Pipeline

| Step      | Script       | What it does                                        |
| --------- | ------------ | --------------------------------------------------- |
| Data prep | `convert.py` | Dataset format conversion, scraping, face alignment |
| Training  | `train.py`   | Full model retraining with warm weights             |
| Export    | `export.py`  | Exports trained model to `.h5`                      |
| Demo      | `demo.py`    | Runs makeup transfer on new images                  |

---

### Tech Stack

- **Framework:** PyTorch
- **Image processing:** OpenCV
- **Base model:** BeautyGAN (retrained from warm weights)
- **Generators:** ResNet, Dilated ResNet
- **Ground truth:** Warping + Poisson blending

---

<a href="https://github.com/raraspradnya/GANMakeupTransferVariousSkin" target="_blank">View on GitHub →</a>
&nbsp;&nbsp;·&nbsp;&nbsp;
<a href="https://bit.ly/GANMakeupTransferVariousSkin" target="_blank">Read the Paper →</a>
