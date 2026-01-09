# Convolution reminder

For an input feature map with dimensions $n_h \times n_w \times n_c$, using a convolution layer with filter size $f$, padding $p$, stride $s$, and number of filters $n_f$, the output dimensions are:

$$
\left\lfloor \frac{n_h + 2p - f}{s} \right\rfloor + 1
\quad\times\quad
\left\lfloor \frac{n_w + 2p - f}{s} \right\rfloor + 1
\quad\times\quad
n_f
$$


where:

- $n_h$ → height of the input feature map  
- $n_w$ → width of the input feature map  
- $n_c$ → number of channels in the input feature map  
- $f$ → size of the convolution filter (kernel)  
- $p$ → padding size  
- $s$ → stride length  
- $n_f$ → number of filters (output channels)  


## Interesting notions

### Aliasing


### Feature hierarchy intuition

CNNs naturally learn a hierarchy of representations due to receptive field growth + depth:
- Early layers: edges, corners, simple textures
- Middle layers: motifs, repeated patterns, object parts
- Deep layers: object-level concepts (faces, wheels, etc.)
