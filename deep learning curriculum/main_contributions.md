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

