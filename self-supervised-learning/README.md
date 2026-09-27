# Self-supervised learning (vision)

Curriculum et fiches de référence sur le self-supervised learning en vision
(DINOv3, JEPA, MAE/iBOT, contrastif...).

> **📋 [Cheatsheet — référence rapide](https://arnaudbru.github.io/resources/self-supervised-learning/cheatsheet.html)**
> Taxonomie des 7 familles SSL, tableau des mécanismes anti-collapse, composants récurrents,
> protocoles d'éval, timeline.

## Contents

| Page | Rendered | What it is |
|---|---|---|
| Fiches de compréhension | [open](https://arnaudbru.github.io/resources/self-supervised-learning/) | 7 fiches interactives (attention, ViT, lignée DINO, lignée JEPA, MAE/iBOT, contrastif, bases historiques), avec cases à cocher sur 23 questions d'expertise + 38 items de mécanismes/dérivés/généalogie, progression persistée via `localStorage`. |
| Cheatsheet | [open](https://arnaudbru.github.io/resources/self-supervised-learning/cheatsheet.html) | Page de référence rapide : taxonomie des 7 familles SSL, tableau des mécanismes anti-collapse, composants récurrents, protocoles d'éval, timeline. Page de consultation, pas de suivi de progression. |

## How to read these

Le lien "Fiches de compréhension" rend la page interactive directement dans le navigateur :
cocher les questions et mécanismes au fur et à mesure enregistre la progression localement
(par origine — utiliser le lien hébergé plutôt qu'une copie locale du fichier pour que la
progression survive aux mises à jour de contenu). La cheatsheet est une page de consultation
pure, sans suivi.

Le curriculum ci-dessous détaille l'ordre dans lequel ce contenu a été construit : les modèles
état de l'art d'abord (DINOv3, JEPA), les fondations ensuite (self-distillation, masked
modeling, contrastif), l'historique en dernier. Le référentiel des questions d'expertise (23
dans les fiches, 13 dans la version condensée ci-dessous) sert de définition du "acquis" pour
chaque module : le savoir y répondre par écrit, sans relire la fiche.

---

## Approche du curriculum

Ordre inversé par rapport à un cursus académique classique : les modèles état de l'art
utilisés en production sont traités en premier, les fondations théoriques et historiques
sont creusées ensuite — dans la mesure où une question d'expertise l'exige. Chaque module
s'ouvre sur ses questions d'expertise ; un module est considéré comme acquis quand on sait y
répondre en profondeur, avec exemples et contre-exemples.

---

## Partie 1 — Modèles état de l'art

### DINOv3 en pratique

**Questions d'expertise :**
1. Pourquoi un backbone *frozen* DINOv3 bat-il des modèles fine-tunés spécialisés en segmentation/détection ?
2. Quelle est la différence entre features globales (CLS token) et features denses (patch tokens), et quelles tâches utilisent lesquelles ?
3. Quand choisir DINOv3 vs CLIP/SigLIP 2 en production ?

**Contenu :**
- Le paper DINOv3 (arXiv 2508.10104), en particulier les sections résultats et Gram anchoring.
- Prise en main des checkpoints officiels (ViT-S → ViT-L) : extraction de features, k-NN classification, visualisation des patch features en PCA.
- Le pattern industriel de référence : **backbone frozen + adapter léger** (linear probe, puis tête de segmentation type linear/Mask2Former simplifiée).
- Comparaison empirique DINOv3 vs CLIP vs ResNet supervisé sur un même linear probe.

**Exercice associé** : un projet applicatif (détection, segmentation ou retrieval) sur backbone DINOv3 frozen, avec benchmark comparatif.

### La famille JEPA et la vidéo

**Questions d'expertise :**
4. Pourquoi prédire dans l'espace *latent* (JEPA) plutôt que dans l'espace *pixel* (MAE) ? Quel problème des pixels cela évite-t-il ?
5. En quoi V-JEPA 2 est-il un "world model" et pourquoi ça intéresse la robotique ?

**Contenu :**
- I-JEPA (2023) puis V-JEPA 2 (2025) — la vision de LeCun sur le futur de l'IA.
- Un checkpoint V-JEPA : features vidéo, éval sur un benchmark réduit (UCF101 ou similaire).
- VideoMAE v2 en survol, pour situer l'alternative MIM en vidéo.

**Exercice associé** : notebook comparatif image vs vidéo SSL, et une note de synthèse "JEPA vs MIM vs distillation — que prédit-on, dans quel espace, et pourquoi ça change tout".

---

## Partie 2 — Mécanismes et fondations

Objectif : comprendre pourquoi les modèles de la partie 1 sont conçus ainsi, en ne remontant
que ce qui alimente directement l'état de l'art.

### Self-distillation : la lignée DINO

**Questions d'expertise :**
6. Pourquoi BYOL/DINO ne collapsent-ils pas sans exemples négatifs ? (EMA, stop-gradient, centering+sharpening — être précis sur chaque mécanisme.)
7. Quel est le vrai facteur différenciant de DINOv2/v3 par rapport aux méthodes académiques ? (indice : la curation de données)
8. Que résout le Gram anchoring de DINOv3, et pourquoi les features denses se dégradaient-elles sur les longs entraînements ?

**Contenu, dans cet ordre (généalogie inversée) :**
- DINOv2 (2023) : curation LVD-142M, loss iBOT+DINO, distillation vers petits modèles.
- DINO (2021) : self-distillation, attention maps émergentes — recoder ou adapter la repo sur un petit dataset et visualiser les attention maps.
- BYOL (2020) + SimSiam (2021) : les mécanismes anti-collapse à nu.

**Exercice associé** : un mini-DINO entraîné, et une note de synthèse sur les 4 familles de mécanismes anti-collapse.

### Masked Image Modeling : la lignée MAE/iBOT

**Questions d'expertise :**
9. Pourquoi MAE excelle en fine-tuning mais est moyen en linear probe, et inversement pour DINO ? Qu'est-ce que ça dit de la nature des features apprises ?
10. Comment iBOT fusionne-t-il MIM et self-distillation, et pourquoi cette fusion est-elle au cœur de DINOv2 ?

**Contenu :**
- MAE (2021) : masking 75%, encodeur/décodeur asymétrique.
- iBOT (2021) : le chaînon manquant entre DINO et DINOv2.
- data2vec (2022) en survol : la prédiction latente avant JEPA.

**Exercice associé** : un tableau comparatif linear probe vs fine-tuning sur les méthodes ci-dessus, sur un même benchmark.

### Le contrastif : SimCLR/MoCo et la théorie

**Questions d'expertise :**
11. Pourquoi le batch size est-il critique en contrastif et pas en self-distillation ?
12. Que capturent alignment et uniformity (Wang & Isola), et comment ça éclaire toutes les méthodes vues avant ?

**Contenu :**
- SimCLR (2020) : implémenter la loss NT-Xent from scratch.
- MoCo v3 : momentum encoder (qui survit dans BYOL/DINO).
- InfoNCE (CPC, 2018) + alignment/uniformity pour la théorie.

**Exercice associé** : SimCLR from scratch sur STL-10, avec 2-3 ablations (température, augmentations).

---

## Partie 3 — Bases historiques et culture (optionnelle)

À traiter en lecture légère, en parallèle du reste :

- Pretext tasks : RotNet, Jigsaw, Colorization (2015-2018) — comprendre pourquoi elles ont plafonné.
- SwAV, Barlow Twins, VICReg — les branches alternatives (clustering, redondance).
- BEiT — le tokenizer discret.

**Question d'expertise 13 :** pourquoi les pretext tasks "artisanales" ont-elles plafonné alors que le masking et la distillation scalent ?

---

## Veille

- **Sources** : arXiv cs.CV (self-supervised, JEPA, representation learning), blog Meta AI/FAIR, auteurs clés (Caron, Oquab, Assran, Bojanowski, Siméoni, LeCun).
- **Conférences** : orals SSL de CVPR/ICCV/NeurIPS/ICLR.

## Ressources

**Papers clés (lignées SOTA)**
- [I-JEPA](https://arxiv.org/abs/2301.08243)
- [V-JEPA 2](https://ai.meta.com/vjepa/)
- DINOv3 — arXiv 2508.10104

**Mécanisme d'attention — pour approfondir √d_k et le multi-head**
- [DEV Community — Scaling Is All You Need: Understanding sqrt(d_k) in Self-Attention](https://dev.to/samyak112/scaling-is-all-you-need-understanding-sqrtd-in-self-attention-29pk)
- [Chiffres concrets sur l'attention scaled dot-product](https://www.abhik.ai/concepts/attention/scaled-dot-product)
- [Distribution des scores d'attention (arXiv 2311.09406, section 2)](https://arxiv.org/pdf/2311.09406)
- [AI Summer — Why multi-head self attention works](https://theaisummer.com/self-attention/)
- [DigitalOcean — Multi-head attention, simple explained](https://www.digitalocean.com/community/tutorials/multi-head-attention-simple-explained)
- [Ketan Doshi — Transformers Explained Visually, Part 3: Multi-Head Attention Deep Dive](https://medium.com/data-science/transformers-explained-visually-part-3-multi-head-attention-deep-dive-1c1ff1024853)

**Ressources transverses**
- Repos officiels Meta : dinov3, dinov2, dino, mae, ijepa, jepa.
- Librairie `lightly` et `solo-learn` : implémentations propres de toutes les méthodes.
- Blog de Lilian Weng (Contrastive Representation Learning, Self-Supervised Learning).
- Cours Berkeley CS294 (Pieter Abbeel).

---

## Référentiel : les 13 questions d'expertise

Version condensée du référentiel à 23 questions couvert par les fiches interactives.
L'expertise se mesure à la capacité de répondre à ces 13 questions avec précision, exemples
et contre-exemples.

1. Pourquoi un backbone frozen DINOv3 bat-il des modèles fine-tunés spécialisés ?
2. Features globales vs denses : différence et usages ?
3. DINOv3 vs CLIP/SigLIP en production : quand, pourquoi ?
4. Pourquoi prédire en latent (JEPA) plutôt qu'en pixels (MAE) ?
5. En quoi V-JEPA 2 est-il un world model ?
6. Pourquoi BYOL/DINO ne collapsent pas sans négatifs ?
7. Quel est le vrai différenciant de DINOv2/v3 ? (données)
8. Que résout le Gram anchoring ?
9. MAE fort en fine-tuning / DINO fort en linear probe : pourquoi ?
10. Comment iBOT fusionne MIM et distillation ?
11. Pourquoi le batch size est critique en contrastif ?
12. Que disent alignment et uniformity de toutes ces méthodes ?
13. Pourquoi les pretext tasks ont plafonné alors que masking/distillation scalent ?
