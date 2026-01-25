# Vision Transformers

## Image preprocessing: From an image to a series of tokens

The following equation turns an image into a sequence of position-aware (via $E_{pos}) tokens - one per patch, plus a class token $x_{\text{class}}$

$$
z_0 = \big[ x_{\text{class}};; x_p^1 E;; x_p^2 E;; \dots;; x_p^N E \big] + E_{\text{pos}}
$$

where:

* $z_0$ → input token sequence to the Vision Transformer encoder
* $x_{\text{class}} \in \mathbb{R}^D$ → **learnable** class token used for global image representation
* $x_p^i \in \mathbb{R}^{P^2 \cdot C}$ → flattened vector of the $i$-th image patch
* $E \in \mathbb{R}^{(P^2 \cdot C) \times D}$ → **learnable** linear projection from patch space to embedding space
* $x_p^i E \in \mathbb{R}^D$ → token embedding corresponding to the $i$-th patch
* $E_{\text{pos}} \in \mathbb{R}^{(N+1) \times D}$ → **learnable** positional embeddings added to each token

* $P$ → patch size (patches are $P \times P$ pixels)
* $C$ → number of image channels
* $D$ → embedding dimension of the transformer
* $N = \frac{HW}{P^2}$ → number of patches for an image of size $H \times W$

#### Paper: Dosovitskiy et al. (2020) — [*An Image is Worth 16×16 Words*](https://arxiv.org/pdf/2010.11929) (ViT)

## (WIP) DeiT (Data-efficient Image Transformers) 

#### Paper: Touvron et al. (2021) — [*DeiT*](https://arxiv.org/pdf/2012.12877)
