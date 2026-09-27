# Fiches de compréhension — SSL Vision

Sept fiches structurées de la même façon : l'idée en une phrase, les mécanismes à comprendre, les papers de référence, les pièges de compréhension, et les questions d'expertise (tu maîtrises la fiche quand tu sais y répondre avec précision).

---

## Fiche 1 · Le mécanisme d'attention et ses dérivés

**L'idée en une phrase** : chaque élément d'une séquence calcule une moyenne pondérée de tous les autres, où les poids sont appris dynamiquement en fonction du contenu — contrairement à une convolution dont les poids sont fixes après entraînement.

**Mécanismes à comprendre :**
- **Q, K, V** : chaque token produit une query ("qu'est-ce que je cherche ?"), une key ("qu'est-ce que je propose ?") et une value ("qu'est-ce que je transmets ?"). Le score d'attention = softmax(QKᵀ/√d) appliqué à V.
- **Le √d** : sans cette normalisation, les produits scalaires grandissent avec la dimension, le softmax sature, les gradients disparaissent.
- **Multi-head** : plusieurs attentions en parallèle sur des sous-espaces, chaque tête peut se spécialiser (relations locales, globales, sémantiques...).
- **Self vs cross-attention** : self = Q, K, V viennent de la même séquence ; cross = Q vient d'une séquence, K/V d'une autre (utilisé dans les décodeurs, DETR, les modèles multimodaux). Propriétés de permutation distinctes : la self-attention est *équivariante* par permutation ; la cross-attention est équivariante en Q mais *invariante* en K/V — elle traite la seconde séquence comme un ensemble non ordonné.
- **Masked self-attention** : un masque met certaines positions à −∞ *avant* le softmax, qui leur donne donc un poids nul. Cas canonique : le masque causal des décodeurs — le token t ne voit que les tokens ≤ t, ce qui permet d'entraîner toutes les positions en parallèle sans que le modèle lise la suite. Rare en vision (ViT et BERT sont non masqués), mais l'idée réapparaît dans le masking de MAE et de JEPA.
- **Complexité O(n²)** : chaque token regarde tous les autres. C'est LE goulot d'étranglement qui motive tous les dérivés.

**Les dérivés à connaître :**
- **Attention linéaire / efficiente** (Performer, Linformer) : approximations pour casser le O(n²).
- **Windowed attention** (Swin Transformer) : attention locale par fenêtres glissantes, hiérarchique — réintroduit un biais inductif de localité.
- **FlashAttention** : pas un changement mathématique mais une réorganisation mémoire (tiling, recomputation) qui rend l'attention exacte beaucoup plus rapide. Standard partout en 2026.
- **Registers** (Darcet et al., 2023) : tokens supplémentaires sans signification spatiale qui absorbent les "artefacts" d'attention — directement utilisés dans DINOv2/v3.

**Pièges de compréhension :**
- L'attention n'a *aucune* notion d'ordre : sans positional encoding, une image mélangée donne le même résultat. Comprendre pourquoi.
- "Attention map" ≠ "explication" : les cartes d'attention sont suggestives mais pas une explication causale fiable.

**Questions d'expertise :**
1. Pourquoi divise-t-on par √d dans le softmax, et que se passe-t-il concrètement si on l'enlève ?
2. Quelle est la différence de biais inductif entre une convolution et la self-attention, et quelle en est la conséquence sur les besoins en données ?
3. Que sont les register tokens et quel problème des ViT résolvent-ils dans DINOv2/v3 ?

---

## Fiche 2 · Vision Transformers (ViT)

**L'idée en une phrase** : traiter une image comme une séquence de mots — découper en patches de 16×16, projeter chaque patch en vecteur, et appliquer un Transformer standard sans quasiment aucune adaptation.

**Mécanismes à comprendre :**
- **Patchify** : image 224×224 → 196 patches de 16×16 → 196 tokens de dimension d. C'est une simple projection linéaire (ou une conv de stride 16, équivalent).
- **CLS token** : un token appris ajouté à la séquence, qui agrège l'information globale — c'est lui qu'on utilise pour la classification. Les *patch tokens* portent l'information locale/dense.
- **Positional embeddings** : appris ou sinusoïdaux, ils réinjectent la géométrie perdue. L'interpolation des positional embeddings permet de changer de résolution après entraînement.
- **Pas de biais inductif spatial** : contrairement aux CNN (localité, invariance par translation), le ViT doit tout apprendre des données. Conséquence historique : ViT < CNN sur peu de données, ViT > CNN à grande échelle.
- **Variantes de taille** : ViT-S/B/L/H (+ ViT-g, ViT-7B chez DINOv3). Le "/16" ou "/14" indique la taille des patches — patches plus petits = features plus denses = plus coûteux.

**Pourquoi c'est central pour le SSL** : le ViT est le backbone de tout le SSL moderne. La séparation CLS token (global) / patch tokens (dense) structure toutes les évaluations : classification via CLS, segmentation/profondeur via patchs.

**Pièges de compréhension :**
- Un ViT n'est pas "meilleur" qu'un CNN dans l'absolu : il est meilleur *à grande échelle de données*, ce qui est exactement le régime du SSL.
- Le CLS token n'a rien de magique : certaines méthodes utilisent le mean pooling des patch tokens à la place, avec des résultats similaires.

**Questions d'expertise :**
4. Features globales (CLS) vs denses (patch tokens) : quelle différence, et quelles tâches utilisent lesquelles ?
5. Pourquoi les ViT ont-ils besoin de plus de données que les CNN, et pourquoi est-ce paradoxalement un avantage en SSL ?
6. Que se passe-t-il quand on passe un ViT entraîné en 224×224 sur une image en 518×518 ? Quel mécanisme le permet ?

---

## Fiche 3 · La lignée DINO (self-distillation)

**L'idée en une phrase** : un réseau *student* apprend à prédire les sorties d'un réseau *teacher* qui est une copie retardée de lui-même (moyenne mobile exponentielle de ses poids), sur des vues différentes de la même image — pas de labels, pas de négatifs.

**La généalogie :**
- **BYOL (2020)** : le choc conceptuel — apprendre sans négatifs sans collapse, grâce au trio predictor + EMA teacher + stop-gradient.
- **SimSiam (2021)** : la version minimaliste — montre que le couple **stop-gradient + predictor** est l'ingrédient essentiel, et que l'EMA est optionnel. Retirer le predictor fait collapser le modèle même avec l'EMA ; le retirer *lui* ne casse rien.
- **DINO (2021)** : self-distillation avec softmax de sortie ; anti-collapse par **centering** (soustraire la moyenne des sorties du teacher, empêche la domination d'une dimension) + **sharpening** (température basse côté teacher, empêche la sortie uniforme). Découverte majeure : des cartes d'attention qui segmentent les objets sans supervision.
- **DINOv2 (2023)** : le passage à l'échelle industrielle — curation automatique du dataset LVD-142M (le vrai différenciant), loss combinée DINO (global) + iBOT (dense, cf. fiche 5), régularisateur KoLeo, distillation des gros modèles vers les petits.
- **DINOv3 (2025)** : scaling à 7B de paramètres et 1,7B d'images ; **Gram anchoring** — régularisation qui contraint la matrice de Gram des patch features à rester proche de celle d'un teacher antérieur, résolvant la dégradation des features denses sur les longs entraînements ; premier backbone frozen qui bat des modèles spécialisés en dense prediction.

**Mécanismes anti-collapse (le cœur de la fiche) :**
Le collapse = le réseau sort la même chose pour toute image (solution triviale de "prédire soi-même"). Quatre familles de parades : les négatifs (contrastif), l'asymétrie architecturale (predictor + stop-gradient, BYOL/SimSiam ; l'EMA améliore mais n'est pas nécessaire), la manipulation statistique des sorties (centering+sharpening, DINO), les contraintes de covariance (Barlow Twins/VICReg).

**Pièges de compréhension :**
- Le teacher EMA n'est *pas* entraîné par gradient : il est une moyenne mobile du student. C'est le student qui court après une cible qui se stabilise.
- Le succès de DINOv2/v3 doit plus à la *curation des données* qu'à l'innovation algorithmique — c'est contre-intuitif mais central.

**Questions d'expertise :**
7. Pourquoi BYOL/DINO ne collapsent-ils pas sans négatifs ? Sois précis sur le rôle de chaque mécanisme (EMA, stop-gradient, centering, sharpening).
8. Quel est le vrai facteur différenciant de DINOv2/v3 par rapport aux méthodes académiques ?
9. Que résout le Gram anchoring de DINOv3, et pourquoi les features denses se dégradaient-elles avant ?
10. Pourquoi un backbone frozen DINOv3 bat-il des modèles fine-tunés spécialisés ?

---

## Fiche 4 · La lignée JEPA (prédiction latente)

**L'idée en une phrase** : au lieu de reconstruire les pixels manquants (MAE) ou de comparer des vues (contrastif), prédire la *représentation latente* des parties masquées — on prédit le sens, pas l'apparence.

**La généalogie :**
- **data2vec (2022)** : le précurseur — prédire les représentations d'un teacher EMA sur les parties masquées, unifié pour vision/audio/texte.
- **I-JEPA (2023)** : l'architecture canonique — un context encoder voit une partie de l'image, un predictor prédit les représentations (produites par un target encoder EMA) de blocs cibles, conditionné par leur position. Pas d'augmentations artisanales, pas de reconstruction pixel.
- **V-JEPA (2024)** : extension vidéo — prédire les représentations latentes de tubes spatio-temporels masqués. Apprend le mouvement et la dynamique.
- **V-JEPA 2 (2025)** : scaling massif + volet "world model" — le modèle prédit les conséquences d'actions, utilisé pour la planification robotique zero-shot.

**Pourquoi prédire en latent plutôt qu'en pixels :**
Les pixels contiennent une énorme part d'information imprévisible et sans intérêt sémantique (texture exacte de l'herbe, bruit). Un modèle qui reconstruit les pixels dépense sa capacité sur ces détails. En prédisant en latent, le modèle peut *ignorer l'imprévisible* et se concentrer sur ce qui est structurellement prédictible — la sémantique. C'est l'argument central de LeCun contre les modèles génératifs pour la perception.

**Le cadre conceptuel** : JEPA s'inscrit dans la vision "world models" de LeCun (paper *A Path Towards Autonomous Machine Intelligence*, 2022) — une IA qui apprend un modèle prédictif du monde par observation, comme un enfant.

**Pièges de compréhension :**
- JEPA n'est pas génératif : le predictor prédit des vecteurs, il ne peut pas générer d'image. C'est un choix, pas une limitation.
- Le target encoder EMA joue le même rôle anti-collapse que dans BYOL — la lignée JEPA hérite directement de la self-distillation.

**Questions d'expertise :**
11. Pourquoi prédire dans l'espace latent plutôt qu'en pixels ? Quel problème fondamental des pixels cela évite-t-il ?
12. En quoi V-JEPA 2 est-il un "world model", et pourquoi ça intéresse la robotique ?
13. Qu'est-ce qui empêche I-JEPA de collapser vers des représentations constantes ?

---

## Fiche 5 · La lignée MAE / iBOT (masked image modeling)

**L'idée en une phrase** : le BERT des images — masquer une grande partie des patches et entraîner le modèle à reconstruire ce qui manque.

**La généalogie :**
- **BEiT (2021)** : première transposition sérieuse de BERT — prédit des tokens visuels *discrets* produits par un tokenizer (dVAE) pré-entraîné.
- **MAE (2021)** : la version élégante — masquer 75% des patches, encoder *uniquement* les patches visibles (gain de compute massif), reconstruire les pixels avec un décodeur léger jeté après entraînement.
- **iBOT (2021)** : la fusion — masked modeling où la cible n'est ni les pixels ni des tokens discrets, mais la sortie d'un teacher EMA (comme DINO) sur les patches masqués. C'est du "MIM en latent", chaînon direct vers DINOv2.
- **VideoMAE / v2 (2022-2023)** : extension vidéo avec masking très agressif (90%+), possible car la vidéo est très redondante temporellement.

**Le débat central — MIM vs distillation :**
- MAE apprend d'excellentes features pour le **fine-tuning** mais médiocres en **linear probe** : la reconstruction pixel force des features locales, texturales, peu linéairement séparables — mais qui constituent une excellente initialisation.
- DINO, à l'inverse, apprend des features globales très séparables (excellent linear probe/k-NN) mais son avantage fond en fine-tuning complet.
- iBOT/DINOv2 combinent les deux losses précisément pour avoir le meilleur des deux mondes : global (DINO) + dense (iBOT).

**Pièges de compréhension :**
- Le ratio de masking élevé (75%) n'est pas un détail : avec 15% comme BERT, la tâche est trop facile en vision (interpolation locale suffit). Il faut masquer beaucoup pour forcer la compréhension sémantique.
- Le décodeur de MAE est jeté : seul l'encodeur sert en aval. L'asymétrie encodeur lourd / décodeur léger est un choix d'efficacité.

**Questions d'expertise :**
14. Pourquoi MAE excelle en fine-tuning mais est moyen en linear probe, et inversement pour DINO ? Qu'est-ce que ça dit de la nature des features ?
15. Comment iBOT fusionne MIM et self-distillation, et pourquoi cette fusion est au cœur de DINOv2 ?
16. Pourquoi masque-t-on 75% en vision alors que BERT masque 15% en texte ?

---

## Fiche 6 · Le contrastif

**L'idée en une phrase** : rapprocher dans l'espace des représentations deux vues augmentées de la même image (paire positive) et éloigner toutes les autres images (négatifs).

**Mécanismes à comprendre :**
- **InfoNCE / NT-Xent** : cross-entropy où la "bonne classe" est le positif parmi N candidats. La **température** contrôle la dureté : basse = focalise sur les négatifs difficiles, trop basse = instabilité.
- **SimCLR (2020)** : la recette de référence — grosses batches (les négatifs viennent du batch), composition d'augmentations critique (crop + color jitter surtout), projection head (les features *avant* la projection sont meilleures en aval — subtil et important).
- **MoCo v1/v2/v3** : découpler le nombre de négatifs de la taille du batch via une queue mémoire + un momentum encoder (l'EMA qui survivra dans BYOL/DINO). MoCo v3 : la transposition sur ViT.
- **CLIP (2021)** : contrastif *cross-modal* image-texte. Pas du SSL pur (les textes sont une supervision faible), mais le concurrent de référence de DINOv3 en production.

**La théorie — alignment & uniformity (Wang & Isola, 2020)** : la loss contrastive optimise deux choses décomposables — *alignment* (les positifs sont proches) et *uniformity* (les représentations couvrent uniformément l'hypersphère). Ce cadre éclaire toutes les méthodes : le collapse = perte d'uniformity ; les mécanismes anti-collapse de BYOL/DINO sont des manières implicites de maintenir l'uniformity sans négatifs.

**Pourquoi le contrastif a perdu du terrain** : dépendance au batch size (il faut beaucoup de négatifs), sensibilité extrême au choix des augmentations, features globales mais denses médiocres. La self-distillation et le MIM s'en sont affranchis.

**Pièges de compréhension :**
- Les augmentations ne sont pas un détail d'implémentation : elles *définissent* les invariances apprises. Invariance à la couleur = features aveugles à la couleur — problématique pour certaines tâches (fleurs, oiseaux).
- "Plus de négatifs = mieux" n'est vrai que jusqu'à un point ; les faux négatifs (deux images de la même classe traitées comme négatives) plafonnent la méthode.

**Questions d'expertise :**
17. Pourquoi le batch size est-il critique en contrastif et pas en self-distillation ?
18. Que capturent alignment et uniformity, et comment ce cadre éclaire-t-il le collapse et ses parades dans toutes les familles ?
19. Pourquoi les features avant la projection head sont-elles meilleures que celles après ?
20. Quand choisir DINOv3 vs CLIP/SigLIP en production ?

---

## Fiche 7 · Les bases historiques (pretext tasks)

**L'idée en une phrase** : avant 2020, le SSL consistait à inventer des tâches prétextes artisanales — prédire une transformation appliquée à l'image — en espérant que les résoudre force l'apprentissage de bonnes features.

**Les classiques :**
- **Context prediction (2015)** : prédire la position relative de deux patches. Premier signal que le SSL pouvait marcher.
- **Jigsaw (2016)** : remettre 9 patches dans l'ordre.
- **Colorization (2016)** : coloriser une image en niveaux de gris.
- **RotNet (2018)** : prédire la rotation appliquée (0/90/180/270°). Le plus simple et longtemps le plus efficace.

**Pourquoi elles ont plafonné :**
Chaque tâche prétexte n'exige que les features *strictement nécessaires* à sa résolution. RotNet apprend l'orientation canonique des objets — utile, mais partiel. Jigsaw apprend des indices de continuité locale — le modèle triche avec les aberrations chromatiques des objectifs photo. Aucune ne demande une compréhension sémantique complète. Le masking et la distillation, eux, définissent des tâches *ouvertes* dont la difficulté croît avec les données et la capacité du modèle — elles scalent.

**Ce qui a survécu** : l'idée que la structure de la donnée est le signal (fondation de tout le SSL), le multi-crop (SwAV → DINO), et culturellement, la prudence face aux "raccourcis" (shortcut learning) que les modèles trouvent toujours.

**Branches alternatives à connaître de nom** : SwAV (clustering en ligne + Sinkhorn), Barlow Twins et VICReg (anti-collapse par contraintes de covariance — la 4e famille anti-collapse).

**Questions d'expertise :**
21. Pourquoi les pretext tasks artisanales ont-elles plafonné alors que le masking et la distillation scalent ?
22. Qu'est-ce que le shortcut learning, avec un exemple concret tiré de Jigsaw ?
23. Comment Barlow Twins/VICReg empêchent-ils le collapse sans négatifs ni EMA ? En quoi est-ce une 4e famille distincte ?

---

## Récapitulatif : les 23 questions d'expertise

Ton tableau de bord global. Les questions 7-20 correspondent au cœur du fast-track ; 1-6 sont les fondations techniques ; 21-23 la culture historique.

**Attention & ViT (fondations)** : 1-6
**DINO** : 7-10 · **JEPA** : 11-13 · **MAE/iBOT** : 14-16 · **Contrastif** : 17-20 · **Historique** : 21-23

Méthode de travail suggérée : après chaque module du parcours, réponds par écrit aux questions correspondantes sans regarder les fiches, puis compare. Une réponse floue = un point à recreuser.
