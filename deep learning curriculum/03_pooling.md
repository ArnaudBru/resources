# Pooling reminder

For a feature map with dimensions $n_h \times n_w \times n_c$, the dimensions of the output after a pooling layer are:

$$
\left\lfloor \frac{n_h - f}{s} \right\rfloor + 1 \quad\times\quad \left\lfloor \frac{n_w - f}{s} \right\rfloor + 1 \quad\times\quad n_c
$$

where:

- $n_h$ → height of the feature map  
- $n_w$ → width of the feature map  
- $n_c$ → number of channels in the feature map  
- $f$ → size of the pooling filter  
- $s$ → stride length  


Types of pooling layer to know:
- Average pooling
- Max pooling
- Global pooling (Max or Average)
