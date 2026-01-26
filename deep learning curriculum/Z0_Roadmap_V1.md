## Core breakthroughs to master (foundational)

### 1) Backpropagation + SGD (1986–1990s) — the training engine behind almost everything

* **Key concepts that enabled it**

  * Chain rule on computation graphs; gradients for layered functions
  * Stochastic optimization (mini-batches), momentum, weight decay
* **What to master**

  * Autodiff, gradient flow, exploding/vanishing gradients
  * Optimizers: SGD+momentum vs Adam; LR schedules; regularization
* **Seminal paper**

  * Rumelhart, Hinton, Williams (1986) — *Learning representations by back-propagating errors*
* **Emblematic models**

  * LeNet-5 (trained with backprop/SGD), later all CNN/Transformer training



### 4) ReLU and modern activations (2010–2017) — stable gradients and faster optimization

* **Key concepts that enabled it**

  * Avoiding saturation (vs sigmoid/tanh)
  * Piecewise linearity; sparse activation
* **What to master**

  * ReLU variants (LeakyReLU, ELU), GELU (common in Transformers)
  * How activations interact with initialization and normalization
* **Seminal references**

  * Nair & Hinton (2010) — ReLU in RBMs (popularized in deep nets)
* **Emblematic models**

  * AlexNet (ReLU), Transformers (GELU)

---

### 5) Dropout (2012) — practical regularization for large models

* **Key concepts that enabled it**

  * Stochastic feature “thinning” approximating model ensembles
* **What to master**

  * When it helps (FC layers, small data) vs when it’s redundant (with strong aug/BN)
  * Dropout placement and rates; inference scaling
* **Seminal paper**

  * Srivastava et al. (2014) — *Dropout: A Simple Way to Prevent Neural Networks from Overfitting* (introduced earlier, formalized here)
* **Emblematic models**

  * AlexNet / early deep CNNs

---

### 6) Better initialization (2010–2015) — makes deep nets trainable

* **Key concepts that enabled it**

  * Variance-preserving init (Xavier/Glorot, He/Kaiming)
* **What to master**

  * Fan-in/fan-out; interaction with ReLU/BN
* **Seminal papers**

  * Glorot & Bengio (2010); He et al. (2015)
* **Emblematic models**

  * VGG/ResNet-era CNNs

---

### 7) Batch Normalization (2015) — acceleration + stability + implicit regularization

* **Key concepts that enabled it**

  * Normalizing intermediate activations; learnable scale/shift
  * Allows higher learning rates and easier optimization
* **What to master**

  * Train vs eval behavior; batch size sensitivity; SyncBN
  * Alternatives: LayerNorm/GroupNorm; why Transformers use LayerNorm
* **Seminal paper**

  * Ioffe & Szegedy (2015) — *Batch Normalization: Accelerating Deep Network Training…*
* **Emblematic models**

  * Inception v2/v3, ResNet

---

### 8) Autoencoders (2006–2010) — representation learning without labels

* **Key concepts that enabled it**

  * Encoder–decoder architectures
  * Bottleneck representations
  * Reconstruction loss as supervision

* **What problem it solved**

  * Learning meaningful representations without labels
  * Dimensionality reduction beyond PCA

* **What it enabled**

  * Unsupervised feature learning
  * Pretraining for deep networks (early deep learning revival)
  * Encoder–decoder patterns used everywhere later

* **Seminal papers / models**

  * Hinton & Salakhutdinov (2006) — Reducing the Dimensionality of Data with Neural Networks
  * Sparse / Denoising Autoencoders

### 9) Variational Autoencoders (2013–2014) — probabilistic latent spaces

* **Key concepts that enabled it**

  * Latent variable models
  * Variational inference
  * Reparameterization trick

* **What problem it solved**

  * Autoencoders learned representations, but not smooth, generative latent spaces 
  * Sampling and interpolation were poorly defined

* **What it enabled**

  * Principled generative modeling 
  * Continuous, structured latent spaces 
  * The mathematical foundation behind modern diffusion and latent generative models

* **Seminal papers / models**

  * Kingma & Welling (2013) — Auto-Encoding Variational Bayes 
  * Rezende et al. (2014)

### 10) “Small kernels + depth” design (2014) — simple scalable CNN blueprints

* **Key concepts that enabled it**

  * Stacking 3×3 convs approximates larger receptive fields with more nonlinearity
  * Uniform architecture makes transfer learning easy
* **What to master**

  * Architecture reading: blocks/stages, downsampling schedules
* **Seminal paper**

  * Simonyan & Zisserman (2014) — *Very Deep Convolutional Networks for Large-Scale Image Recognition*
* **Emblematic models**

  * VGG-16/19

---

### 11) Inception / multi-branch multi-scale processing (2014–2016)

* **Key concepts that enabled it**

  * Parallel paths for multiple receptive fields
  * 1×1 conv for channel mixing and compute reduction (bottlenecks)
* **What to master**

  * Compute budgeting (FLOPs, memory); bottleneck patterns
* **Seminal papers**

  * Szegedy et al. (2015) — *Going Deeper with Convolutions* (GoogLeNet/Inception v1)
* **Emblematic models**

  * GoogLeNet, Inception v3

---

### 12) Residual / skip connections (2015) — deep networks without degradation

* **Key concepts that enabled it**

  * Identity mappings; learning residual functions improves gradient flow
* **What to master**

  * Pre-activation vs post-activation ResNets
  * Why residuals help optimization; relation to dynamical systems intuition
* **Seminal paper**

  * He et al. (2015) — *Deep Residual Learning for Image Recognition*
* **Emblematic models**

  * ResNet-50/101/152 (also the backbone template for many tasks)

---

### 13) Fully Convolutional Networks for dense prediction (2015) — segmentation becomes “natural”

* **Key concepts that enabled it**

  * Replace fully-connected layers with conv layers
  * Upsampling / deconvolution; skip connections for spatial detail
* **What to master**

  * Encoder-decoder patterns; stride vs dilation; output resolution tradeoffs
* **Seminal paper**

  * Long, Shelhamer, Darrell (2015) — *Fully Convolutional Networks for Semantic Segmentation*
* **Emblematic models**

  * FCN-8s and descendants

---

### 14) U-Net (2015) — the canonical encoder–decoder with skip fusion

* **Key concepts that enabled it**

  * Symmetric skip connections that concatenate features across scales
  * Strong augmentation when data is limited
* **What to master**

  * Skip fusion strategies (concat vs add), decoder design, loss functions (Dice/CE)
* **Seminal paper**

  * Ronneberger, Fischer, Brox (2015) — *U-Net: Convolutional Networks for Biomedical Image Segmentation*
* **Emblematic models**

  * U-Net (and countless variants for medical/industrial vision)

---

### 15) Two-stage detection (2014–2017) — region proposals + classification/refinement

* **Key concepts that enabled it**

  * Region proposals; ROI pooling/align
  * Multi-task heads (cls + bbox regression); shared backbone features
* **What to master**

  * IoU; bbox parameterization; NMS; mAP evaluation
* **Seminal papers**

  * Girshick et al. (2014) — *R-CNN*
  * Ren et al. (2015) — *Faster R-CNN* (RPN)
* **Emblematic models**

  * Faster R-CNN (+ ResNet backbone)

---

### 16) One-stage detection (2016–2018) — speed by removing proposals

* **Key concepts that enabled it**

  * Direct dense prediction over grids/feature maps
  * Anchors (or anchor-free later), focal loss for class imbalance
* **What to master**

  * Anchor design; label assignment; focal loss; feature pyramids
* **Seminal papers**

  * Redmon et al. (2016) — *YOLO*
  * Lin et al. (2017) — *Focal Loss for Dense Object Detection* (RetinaNet)
* **Emblematic models**

  * YOLO family, RetinaNet

---

### 17) Feature Pyramid Networks (2017) — multi-scale done “right” for detection/segmentation

* **Key concepts that enabled it**

  * Top-down pathway + lateral connections for semantic + high-res features
* **What to master**

  * Multi-scale training/inference; pyramid level assignment
* **Seminal paper**

  * Lin et al. (2017) — *Feature Pyramid Networks for Object Detection*
* **Emblematic models**

  * Faster R-CNN + FPN, RetinaNet

---

### 18) Instance segmentation (2017) — detection + per-instance masks

* **Key concepts that enabled it**

  * Parallel mask head; ROIAlign to avoid quantization artifacts
* **What to master**

  * ROIAlign vs ROIPool; mask losses; evaluation (APmask)
* **Seminal paper**

  * He et al. (2017) — *Mask R-CNN*
* **Emblematic models**

  * Mask R-CNN

---

### 19) Dilated/Atrous convolutions + ASPP (2016–2018) — context without losing resolution

* **Key concepts that enabled it**

  * Increasing receptive field via dilation while keeping feature map resolution
  * Multi-scale context aggregation (ASPP)
* **What to master**

  * Dilation gridding artifacts; output stride; segmentation backbones
* **Seminal papers**

  * Chen et al. (2017/2018) — *DeepLab* family
* **Emblematic models**

  * DeepLabv3(+)

---

### 20) Attention / Transformer core (2017) — global interactions as a primitive

* **Key concepts that enabled it**

  * Self-attention, multi-head attention, positional encoding
  * Scale-friendly training (parallelism) and large data regimes
* **What to master**

  * Q/K/V mechanics; attention complexity; positional embeddings
  * LayerNorm, residual+MLP blocks, warmup schedules
* **Seminal paper**

  * Vaswani et al. (2017) — *Attention Is All You Need*
* **Emblematic models**

  * Transformer encoder/decoder; later ViT/Swin/DETR

---

### 21) Vision Transformers (2020–2021) — patch tokens replace conv features

* **Key concepts that enabled it**

  * Patchification + linear embedding; transformer encoder as backbone
  * Heavy pretraining (data scale) and strong augmentation/distillation
* **What to master**

  * Patch size tradeoffs; tokenization; class token vs pooling
  * Training recipes (AdamW, warmup, cosine decay, augmentation)
* **Seminal papers**

  * Dosovitskiy et al. (2020) — *An Image is Worth 16×16 Words* (ViT)
  * Touvron et al. (2021) — *DeiT* (data-efficient training)
* **Emblematic models**

  * ViT-B/16, DeiT, Swin Transformer

---

### 22) Hierarchical / windowed transformers (2021) — make Transformers work for detection/segmentation

* **Key concepts that enabled it**

  * Local attention windows + shifting to exchange information efficiently
  * Multi-scale feature hierarchy like CNN stages
* **What to master**

  * Computational scaling; window size; patch merging; feature pyramids
* **Seminal paper**

  * Liu et al. (2021) — *Swin Transformer*
* **Emblematic models**

  * Swin-T/B as backbones for detection/segmentation

---

### 23) End-to-end detection with set prediction (2020) — no anchors, no NMS

* **Key concepts that enabled it**

  * Hungarian matching; object queries; transformer decoder for detection
* **What to master**

  * Bipartite matching loss; query-based decoding; why it simplifies pipelines
* **Seminal paper**

  * Carion et al. (2020) — *DETR: End-to-End Object Detection with Transformers*
* **Emblematic models**

  * DETR and its faster variants (Deformable DETR)

---

### 24) Self-supervised representation learning (2019–2021) — pretrain without labels

* **Key concepts that enabled it**

  * Contrastive learning / instance discrimination
  * Strong augmentations; large batch / memory banks / momentum encoders
* **What to master**

  * Contrastive losses (InfoNCE), negative sampling, collapse avoidance
  * Augmentations as “task definition”
* **Seminal papers**

  * He et al. (2020) — *MoCo*
  * Chen et al. (2020) — *SimCLR*
* **Emblematic models**

  * ResNet backbones pretrained with MoCo/SimCLR, then fine-tuned

---

### 25) Vision–language pretraining (2021) — semantics via text supervision

* **Key concepts that enabled it**

  * Contrastive alignment between image and text embeddings
  * Web-scale data; prompt engineering; zero-shot transfer
* **What to master**

  * Embedding spaces; prompts/templates; domain shift robustness
* **Seminal paper**

  * Radford et al. (2021) — *Learning Transferable Visual Models From Natural Language Supervision* (CLIP)
* **Emblematic models**

  * CLIP (and CLIP-like foundation models)

---

### 26) Diffusion models (2020–2022) — iterative denoising beats GANs for many regimes

* **Key concepts that enabled it**

  * Forward noise process + learned reverse denoiser
  * Score matching / variational bounds; classifier-free guidance (later)
  * Often U-Net backbones + attention
* **What to master**

  * Noise schedules, denoising objective, guidance, sampling speedups
  * Latent diffusion idea: generate in latent space for efficiency
* **Seminal papers**

  * Ho et al. (2020) — *DDPM*
  * Nichol & Dhariwal (2021) — *Improved DDPM*
  * Rombach et al. (2022) — *High-Resolution Image Synthesis with Latent Diffusion Models*
* **Emblematic models**

  * Stable Diffusion (latent diffusion), Imagen-like systems, DALL·E 2-style pipelines

---

## Impactful but less fundamental (still worth knowing)

### A) Squeeze-and-Excitation (2017) — channel attention inside CNNs

* **Key enabling concept:** global pooling + gating to reweight channels
* **Master:** attention as reweighting; where it helps in backbones
* **Paper:** Hu et al. (2017) — *Squeeze-and-Excitation Networks*
* **Model:** SENet, EfficientNet-style blocks

### B) Efficient scaling laws + NAS (2018–2019)

* **Key enabling concept:** architecture search + compound scaling rules
* **Master:** FLOPs/latency tradeoffs; scaling depth/width/resolution
* **Paper:** Tan & Le (2019) — *EfficientNet*
* **Model:** EfficientNet, EfficientDet

### C) Modern augmentation recipes (2017–2021): Mixup, CutMix, RandAugment

* **Key enabling concept:** regularization by synthetic mixing / learned augmentation policies
* **Master:** augmentation as inductive bias; how it changes calibration/robustness
* **Models:** strong for ViT/DeiT and many CNNs

### D) Knowledge distillation (2015–2021)

* **Key enabling concept:** soft targets transfer “dark knowledge”
* **Master:** teacher–student setups; when distillation substitutes for data
* **Paper:** Hinton et al. (2015) — *Distilling the Knowledge in a Neural Network*
* **Model:** DeiT (distillation token), many production compression pipelines

### E) GAN refinements (2015–2020) — still useful ideas even if diffusion dominates

* **Key enabling concept:** adversarial losses + architectural/training tricks
* **Master:** stability tricks (spectral norm, gradient penalties), evaluation (FID)
* **Models:** DCGAN, StyleGAN2

### F) Neural Radiance Fields (2020) — major for 3D, not always core for 2D pipelines

* **Key enabling concept:** continuous scene function + volume rendering
* **Master:** coordinate networks, rendering integrals, camera pose handling
* **Paper/Model:** Mildenhall et al. (2020) — NeRF

---

## If you want a “mastery checklist” (what to be truly strong at)

1. **Optimization & training recipes:** LR schedules, regularization, normalization, batch size effects
2. **Backbone literacy:** ResNet-style stages, bottlenecks, receptive field control (stride/dilation)
3. **Detection math:** IoU, losses, assignment, NMS vs set prediction, evaluation metrics
4. **Segmentation design:** encoder-decoder, skip fusion, context modules (ASPP), resolution tradeoffs
5. **Transformer mechanics:** attention, tokens, positional encoding, scaling constraints
6. **Pretraining & transfer:** supervised vs self-supervised vs multimodal; fine-tuning strategies
7. **Generative modeling:** diffusion training objectives, guidance, sampling; U-Net+attention patterns

If you want, I can convert this into a “study path” with a recommended order, what to implement for each (small coding exercises), and a mapping from these concepts to tasks like detection of dépôts sauvages (false positives/false negatives, domain shift, low-light, occlusions).
