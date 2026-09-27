# Self-supervised learning (vision) — fast-track curriculum

Parcours + fiches de référence sur le SSL en vision (DINOv3, JEPA, MAE/iBOT, contrastif...),
construits progressivement sur plusieurs sessions.

## Contents

| Page | Rendered | What it is |
|---|---|---|
| Fiches de compréhension | [open](https://arnaudbru.github.io/resources/ssl/) | 7 fiches interactives (attention, ViT, lignée DINO, lignée JEPA, MAE/iBOT, contrastif, bases historiques), avec cases à cocher sur 23 questions d'expertise + 38 items de mécanismes/dérivés/généalogie, progression persistée via `localStorage`. Aussi disponible en [texte brut](fiches.md). |
| Cheatsheet | [open](https://arnaudbru.github.io/resources/ssl/cheatsheet.html) | Page de référence rapide : taxonomie des 7 familles SSL, tableau des mécanismes anti-collapse, composants récurrents, protocoles d'éval, timeline. Page de consultation, pas de suivi de progression. |

## How to read these

Le lien "Fiches de compréhension" rend la page interactive directement dans le navigateur —
coche les questions et mécanismes au fur et à mesure, la progression est sauvegardée localement
(par origine : utilise le lien hébergé plutôt qu'une copie locale du fichier pour que la
progression survive aux mises à jour de contenu). La cheatsheet est une page de consultation pure.

Le curriculum ci-dessous est le parcours pédagogique suivi pour produire ces fiches : SOTA
d'abord (DINOv3, JEPA), backfill des fondations ensuite (DINO/BYOL/SimSiam, MAE/iBOT,
SimCLR/contrastif), historique en dernier. Le référentiel des 23 questions d'expertise
(13 dans la version courte ci-dessous, 23 dans les fiches) est la colonne vertébrale : un
module n'est acquis que quand on sait y répondre par écrit sans relire la fiche.

---

# Parcours SSL Vision — Fast-track vers l'état de l'art

**Philosophie** : inversée par rapport à un cursus académique. Tu commences par les modèles que tout le monde utilise en 2026, tu deviens opérationnel dessus en quelques semaines, puis tu creuses rétroactivement les fondations — uniquement quand une question d'expertise t'y oblige. Chaque module s'ouvre par ses **questions d'expertise** : tant que tu ne sais pas y répondre en profondeur, le module n'est pas terminé.

**Durée** : opérationnel sur le SOTA en ~1 mois, expertise complète en 3-4 mois.

---

## Phase 1 — Opérationnel sur le SOTA (semaines 1-4)

### Module 1 · DINOv3 en pratique (semaines 1-2)

**Questions d'expertise :**
1. Pourquoi un backbone *frozen* DINOv3 bat-il des modèles fine-tunés spécialisés en segmentation/détection ?
2. Quelle est la différence entre features globales (CLS token) et features denses (patch tokens), et quelles tâches utilisent lesquelles ?
3. Quand choisir DINOv3 vs CLIP/SigLIP 2 en production ?

**À faire :**
- Lire le paper DINOv3 (arXiv 2508.10104) en diagonale d'abord, puis les sections résultats et Gram anchoring en profondeur.
- Prendre en main les checkpoints officiels (ViT-S → ViT-L suffisent) : extraction de features, k-NN classification, visualisation des patch features en PCA (les fameuses cartes colorées).
- Construire le pattern industriel de référence : **backbone frozen + adapter léger** (linear probe, puis tête de segmentation type linear/Mask2Former simplifiée) sur un dataset de ton domaine.
- Comparer empiriquement DINOv3 vs CLIP vs un ResNet supervisé sur le même linear probe.

**Livrable** : un projet applicatif complet (détection, segmentation ou retrieval) sur backbone DINOv3 frozen, avec benchmark comparatif. C'est directement valorisable en entreprise.

### Module 2 · La famille JEPA et la vidéo (semaines 3-4)

**Questions d'expertise :**
4. Pourquoi prédire dans l'espace *latent* (JEPA) plutôt que dans l'espace *pixel* (MAE) ? Quel problème des pixels cela évite-t-il ?
5. En quoi V-JEPA 2 est-il un "world model" et pourquoi ça intéresse la robotique ?

**À faire :**
- Lire I-JEPA (2023) puis V-JEPA 2 (2025) — c'est la vision de LeCun sur le futur de l'IA, très présente dans le débat actuel.
- Manipuler un checkpoint V-JEPA : features vidéo, éval sur un benchmark réduit (UCF101 ou similaire).
- Lire en survol VideoMAE v2 pour situer l'alternative MIM en vidéo.

**Livrable** : notebook comparatif image vs vidéo SSL + une note d'une page : "JEPA vs MIM vs distillation — que prédit-on, dans quel espace, et pourquoi ça change tout".

**→ Fin de la phase 1 : tu sais utiliser, évaluer et choisir les modèles SSL de 2026. Tu peux déjà en parler crédiblement en entretien ou en projet.**

---

## Phase 2 — Comprendre les mécanismes (semaines 5-9)

Maintenant que tu utilises ces modèles, tu remontes leur lignée pour comprendre *pourquoi* ils sont conçus ainsi. On ne remonte que ce qui alimente directement le SOTA.

### Module 3 · Self-distillation : la lignée DINO (2 semaines)

**Questions d'expertise :**
6. Pourquoi BYOL/DINO ne collapsent-ils pas sans exemples négatifs ? (EMA, stop-gradient, centering+sharpening — sois précis sur chaque mécanisme.)
7. Quel est le vrai facteur différenciant de DINOv2/v3 par rapport aux méthodes académiques ? (indice : la curation de données)
8. Que résout le Gram anchoring de DINOv3, et pourquoi les features denses se dégradaient-elles sur les longs entraînements ?

**À faire, dans cet ordre (généalogie inversée) :**
- DINOv2 (2023) : curation LVD-142M, loss iBOT+DINO, distillation vers petits modèles.
- DINO (2021) : self-distillation, attention maps émergentes — recoder ou adapter la repo sur un petit dataset et **visualiser les attention maps**.
- BYOL (2020) + SimSiam (2021) : les mécanismes anti-collapse à nu.

**Livrable** : mini-DINO entraîné + note de synthèse "les 4 familles de mécanismes anti-collapse".

### Module 4 · Masked Image Modeling : la lignée MAE/iBOT (1,5 semaine)

**Questions d'expertise :**
9. Pourquoi MAE excelle en fine-tuning mais est moyen en linear probe, et inversement pour DINO ? Qu'est-ce que ça dit de la nature des features apprises ?
10. Comment iBOT fusionne-t-il MIM et self-distillation, et pourquoi cette fusion est-elle au cœur de DINOv2 ?

**À faire :**
- MAE (2021) : masking 75%, encodeur/décodeur asymétrique. Entraîner un petit MAE.
- iBOT (2021) : le chaînon manquant entre DINO et DINOv2.
- data2vec (2022) en survol : la prédiction latente avant JEPA.

**Livrable** : tableau comparatif personnel linear probe vs fine-tuning sur les méthodes des modules 3-4, même benchmark.

### Module 5 · Le contrastif : SimCLR/MoCo et la théorie (1,5 semaine)

**Questions d'expertise :**
11. Pourquoi le batch size est-il critique en contrastif et pas en self-distillation ?
12. Que capturent alignment et uniformity (Wang & Isola), et comment ça éclaire toutes les méthodes vues avant ?

**À faire :**
- SimCLR (2020) : implémenter la loss NT-Xent from scratch — c'est l'exercice de code le plus formateur du parcours, même si la méthode n'est plus SOTA.
- MoCo v3 en lecture : momentum encoder (qui survit dans BYOL/DINO !).
- InfoNCE (CPC, 2018) + alignment/uniformity pour la théorie.

**Livrable** : SimCLR from scratch sur STL-10 avec 2-3 ablations (température, augmentations).

---

## Phase 3 — Bases historiques et culture (optionnelle, en fond, semaines 10+)

À traiter en lecture légère, un paper par semaine en parallèle d'autre chose :

- Pretext tasks : RotNet, Jigsaw, Colorization (2015-2018) — comprendre pourquoi elles ont plafonné.
- SwAV, Barlow Twins, VICReg — les branches alternatives (clustering, redondance).
- BEiT — le tokenizer discret.

**Question d'expertise 13 :** pourquoi les pretext tasks "artisanales" ont-elles plafonné alors que le masking et la distillation scalent ?

---

## Veille continue (dès la semaine 1)

- **Sources** : arXiv cs.CV (self-supervised, JEPA, representation learning), blog Meta AI/FAIR, auteurs clés (Caron, Oquab, Assran, Bojanowski, Siméoni, LeCun).
- **Rythme** : 1 paper récent/semaine dès le début — la phase 1 te donne déjà le vocabulaire pour les lire.
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
- Cours Berkeley CS294 (Pieter Abbeel) — en support pour les phases 2-3.
- Compute : checkpoints frozen = quasi gratuit (phase 1) ; petits entraînements sur Colab/Kaggle (phases 2-3).

---

## Le référentiel : les 13 questions d'expertise

C'est ton tableau de bord. Coche-les au fur et à mesure ; l'expertise, c'est de savoir répondre aux 13 avec précision, exemples et contre-exemples. (Les fiches interactives en couvrent une version étendue à 23 questions, réparties sur les 7 fiches.)

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
