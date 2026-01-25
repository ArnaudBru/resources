# Distillation

**Paper**: Hinton et al. (2015) — [*Distilling the Knowledge in a Neural Network*](https://arxiv.org/pdf/1503.02531)

Distillation's main idea is to transfer knowledge from a bigger, more cumbersome model (the teacher) to a smaller model (the student).

## Motivation - Why

Large neural networks or ensembles often perform much better than individual small models, especially when trained on big datasets, but are too slow or expensive for deployment.
Instead of deploying the large model directly, the paper proposes compressing its learned knowledge into a smaller, efficient model that runs faster with minimal loss in accuracy.

## How it works

The teacher model's output logits are softened using a softmax with a high temperature. Using a high temperature retains more information about class similarities in the output probabilities. The student model is trained to match these soft targets distribution (probabilities) as they provide more information on the class distribution (e.g. classes that are similar or often confused)

In practice, the student model is trained using the same high temperature used by the teacher model for prediction. However, for prediction, the student model uses a temperature of 1

$$
\frac{\partial C}{\partial z_i} = \frac{1}{T}\, (q_i - p_i)
= \frac{1}{T} \left(\frac{e^{z_i/T}}{\sum_j e^{z_j/T}} -\frac{e^{v_i/T}}{\sum_j e^{v_j/T}}\right)
$$

where:

* $z_i$ → logit of the student model for class $i$
* $v_i$ → logit of the cumbersome/teacher model for class $i$
* $p_i$ → soft target probability from the teacher:
  $p_i = \frac{e^{v_i/T}}{\sum_j e^{v_j/T}}$
* $q_i$ → student softmax probability at temperature $T$:
  $q_i = \frac{e^{z_i/T}}{\sum_j e^{z_j/T}}$
* $T$ → distillation temperature used in softmax
* $C = - \sum_i p_i \log q_i$ → cross-entropy distillation loss with soft targets

### Example of temperature impact

Assume a model outputs the following logits for 4 classes:

$$
\mathbf{z} = [3.0; 1.5; 0.0; -1.0]
$$

Softmax with temperature $T$ is defined as:

$$
\text{softmax}_i(T) = \frac{\exp(z_i / T)}{\sum_j \exp(z_j / T)}
$$

**Softmax outputs for different temperatures**

| Class | Logit | $T = 1$ | $T = 2$ | $T = 5$ |
|------|-------|---------|---------|---------|
| A | 3.0  | 0.71 | 0.45 | 0.31 |
| B | 1.5  | 0.20 | 0.26 | 0.26 |
| C | 0.0  | 0.07 | 0.17 | 0.23 |
| D | -1.0 | 0.02 | 0.12 | 0.20 |

**Key takeaway**: Increasing the temperature softens the output distribution without changing class ranking, exposing richer information about class relationships that is crucial for knowledge distillation.

## Extension: Using the ground truth

Learning from the teacher's logits will provide the student with the teacher's "view of the world", however, the teacher can be wrong. If you posess the correct labels a.k.a the ground truth labels, it is possible to extend the training.

The student is trained with a weighted sum of two cross-entropies:

**Distillation loss (soft targets)**
Cross-entropy between:
- teacher’s soft probabilities
- student’s soft probabilities
Both computed with a high temperature $T$

This transfers class similarities

**Standard classification loss (hard labels)**
Cross-entropy between:
- student’s predictions
- true one-hot labels
Computed with temperature $T = 1$

This enforces correctness on labeled data

### Distillation with hard + soft targets (final combined objective)

The student loss is a weighted sum of two cross-entropy losses:

$$
\mathcal{L} = \alpha T^2 \mathcal{L}_{\text{soft}} + (1-\alpha)\,\mathcal{L}_{\text{hard}}
$$

where the **soft-target (distillation) loss** is:

$$
\mathcal{L}_{\text{soft}} = - \sum_{i=1}^{K} p_i^{(T)} \log q_i^{(T)}
$$

and the **hard-label loss** is:

$$
\mathcal{L}_{\text{hard}} = - \sum_{i=1}^{K} y_i \log q_i^{(1)}
$$

**Definitions**

- $\mathcal{L}$ → total student training loss  
- $\alpha \in [0,1]$ → weight controlling the balance between soft and hard supervision  
- $T$ → distillation temperature (typically $T>1$ makes the distribution softer)  
- $K$ → number of classes  
- $y_i$ → hard (ground-truth) label as a one-hot vector ($y_i=1$ for the true class, else $0$)  

Teacher / student probabilities:

- $p_i^{(T)}$ → teacher soft target probability at temperature $T$:
  $p_i^{(T)} = \frac{\exp(v_i/T)}{\sum_{j=1}^{K}\exp(v_j/T)}$
- $q_i^{(T)}$ → student probability at temperature $T$:
  $q_i^{(T)} = \frac{\exp(z_i/T)}{\sum_{j=1}^{K}\exp(z_j/T)}$
- $q_i^{(1)}$ → student probability at temperature $1$ (standard softmax used for hard labels):
  $q_i^{(1)} = \frac{\exp(z_i)}{\sum_{j=1}^{K}\exp(z_j)}$

Logits:

- $v_i$ → teacher (cumbersome model) logit for class $i$  
- $z_i$ → student (distilled model) logit for class $i$  

**Why the $T^2$ factor?**: Gradients from the soft-target term scale roughly like $1/T^2$, so multiplying $\mathcal{L}_{\text{soft}}$ by $T^2$ keeps the contribution of the distillation signal comparable when changing $T$.

**The $\alpha$ factor**: No alpha value is explicitely mentioned, however: "best results were generally obtained by using a condiderably lower weight on the second, objective function → $\alpha$ close to 1"
