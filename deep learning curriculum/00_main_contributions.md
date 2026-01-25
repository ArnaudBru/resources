# Deep Learning - Main contributions

## Introduction

This page summarizes the major breakthrough ideas that shaped modern computer vision, from early CNN building blocks like convolution and activations to newer paradigms like transformers, vision-language models, and diffusion. For each invention, it explains the technique, the problem it solved, what it enabled, and the key paper or model that introduced it.

## Contributions

### 1 (TBD) - Backpropagation (1986)

Backpropagation is the core algorithm used to train neural networks by computing how each parameter contributes to the model’s error. It applies the chain rule to propagate the loss gradient from the output back through each layer, producing gradients for all weights efficiently in a single backward pass.

These gradients are then used by an optimizer (such as SGD or Adam) to update parameters and reduce the loss. Backpropagation solved the key problem of training deep, multi-layer models end-to-end, enabling networks to learn visual features directly from data instead of relying on hand-crafted pipelines.

Its main difficulty is unstable gradient flow (vanishing or exploding gradients)

Notions to master:
- Stochastic Gradient Descent (SGC)
- Momentum
- Adam
- Learning rate schedules
- Regularization

### 2 - Convolution, weight sharing (1980 - 1998)

Convolution, weight sharing, and pooling are the core building blocks that made neural networks efficient and effective for images. Convolution applies small filters across the entire image, using the same weights (weight-sharing) everywhere, which enforces locality and translation equivariance while drastically reducing the number of parameters compared to fully connected layers.

This solved the problem of scaling neural networks to high-dimensional images and enabled the learning of hierarchical features, from edges and textures to object parts and full objects.

Notions to master:
- Convolution math: padding/stride/dilation
- Aliasing
- Atrous convolution

aliasing; feature hierarchy intuition

### 3 - Pooling (1990s)

Pooling (or strided downsampling) further reduces spatial resolution, making computation cheaper and adding robustness to small translations or noise.

Together, these ideas allowed deep CNNs to become practical, generalize well, and dominate vision tasks for a decade, forming the backbone of most classical vision architectures before transformers.

- Dimensionality Reduction: less parameter, less computaion. Models are faster and more efficient.
- Invariance to small translations distortions
- Overfitting Prevention: reducing the spatial dimension prevent overfitting by providing a form of regularization.
- Feature Hierarchy: lower layers capture fine details and higher layers capture more abstract and global features.

### 4 (TBD) - Non-linear activation functions (1990s → 2012+)

Non-linear activation functions are what make neural networks more than stacked linear filters, without them, even very deep networks collapse to a single linear transformation.
By inserting a non-linearity after each layer, the model can represent complex patterns and decision boundaries

Early activations like sigmoid and tanh enabled the first neural models but often saturated (gradients ≈ 0 when input is largely positive or largely negative) , causing slow training and vanishing gradients. The shift to ReLU-style activations solved much of this by keeping gradients stable over wide ranges and making optimization far faster and more reliable.

### 5 (TBD) Dropout (2012) — practical regularization for large models

### 6 (TBD) Better initialization (2010–2015) — makes deep nets trainable

### 7 (TBD) Batch Normalization (2015) — acceleration + stability + implicit regularization

### 8 (TBD) “Small kernels + depth” design (2014) — simple scalable CNN blueprints

### 9 (TBD) Inception / multi-branch multi-scale processing (2014–2016)

### 10 (TBD) Residual / skip connections (2015) — deep networks without degradation

### 11 (TBD) Fully Convolutional Networks for dense prediction (2015) — segmentation becomes “natural”

### 12 (TBD) U-Net (2015) — the canonical encoder–decoder with skip fusion

### 13 (TBD) Two-stage detection (2014–2017) — region proposals + classification/refinement

### 14 (TBD) One-stage detection (2016–2018) — speed by removing proposals

### 15 (TBD) Feature Pyramid Networks (2017) — multi-scale done “right” for detection/segmentation

### 16 (TBD) Instance segmentation (2017) — detection + per-instance masks

### 17 (TBD) Dilated/Atrous convolutions + ASPP (2016–2018) — context without losing resolution

### 18 (TBD) Attention / Transformer core (2017) — global interactions as a primitive

### 19 (WIP) Vision Transformers (2020–2021) — patch tokens replace conv features

Until then, Vision models were CNN that would extract feature by progressively building spatial hierarchies through successive convolutions and pooling (first layer represent local feature, deeper layer represent global features).

Vision Transformers replace that with a pure transformer encoder: the image is split into fixed size patches, each patch linearly embedded into a token. A global self-attention is used on the first layer, making global context immediately available

It is often beneficial to fine-tune at higher resolution than pre-training

At first heavy training was necessary, ViTs trade architectural inductive bias for scaling capacity.
DeiT (Data-efficient Image Transformers) showed that ViT can be trained on smaller datasets, introducing the standard ViT training recipe:
- Strong augmentation (RandAugment, Mixup, CutMix)
- AdamW optimizer (decoupled weight decay)
- Learning-rate warmup + cosine decay
- [Knowledge distillation](training_techniques/distillation.md) via a distillation token

### 20 (TBD) Hierarchical / windowed transformers (2021) — make Transformers work for detection/segmentation

### 21 (TBD) End-to-end detection with set prediction (2020) — no anchors, no NMS

### 22 (TBD) Self-supervised representation learning (2019–2021) — pretrain without labels

### 23 (TBD) Vision–language pretraining (2021) — semantics via text supervision

### 24 (TBD) Diffusion models (2020–2022) — iterative denoising beats GANs for many regimes

## Impactful but less fundamental (still worth knowing)

### A) Squeeze-and-Excitation (2017) — channel attention inside CNNs

### B) Efficient scaling laws + NAS (2018–2019)

### C) Modern augmentation recipes (2017–2021): Mixup, CutMix, RandAugment

### D) Knowledge distillation (2015–2021)

### E) GAN refinements (2015–2020) — still useful ideas even if diffusion dominates

### F) Neural Radiance Fields (2020) — major for 3D, not always core for 2D pipelines
