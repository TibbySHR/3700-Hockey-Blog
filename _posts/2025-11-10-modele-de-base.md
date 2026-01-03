---
layout: post
title: Modèles de base
---


# Modèles de base avec régression logistique

Dans cette section, nous évaluons des modèles de base entièrement **non réglés** afin d’obtenir une intuition sur le problème avant d’ajouter des caractéristiques et des modèles plus riches. Nous utilisons **uniquement l’ensemble d’entraînement/validation (saisons 2016/17–2019/20)** ; la saison 2020/21 reste intacte comme **test final**.

# Modèles de base avec régression logistique

Dans cette section, nous évaluons des modèles de base entièrement **non réglés** afin d’obtenir une intuition sur le problème avant d’ajouter des caractéristiques et des modèles plus riches. Nous utilisons **uniquement l’ensemble d’entraînement/validation (saisons 2016/17–2019/20)** ; la saison 2020/21 reste intacte comme **test final**.

## 1.Découpage entraînement/validation et modèle distance-only

Nous divisons les tirs en un ensemble d’entraînement et un ensemble de validation (stratifié). Nous entraînons d’abord une **régression logistique par défaut** sur la seule caractéristique `distance`.

- Code : `clf = LogisticRegression(); clf.fit(X, y)`  
- **Métrique Accuracy (validation)** : *[collez la valeur imprimée]*  
- **Taux de positifs (validation)** : *[collez la valeur imprimée]*

**Remarques.**  
L’accuracy peut paraître trompeuse si les classes sont déséquilibrées. Par exemple, un classifieur “naïf” prédisant toujours la classe majoritaire peut obtenir une accuracy élevée sans aucune capacité de discrimination. En regardant les probabilités (voir ci-dessous), on obtient une mesure bien plus pertinente pour notre objectif xG.

## 2.Probabilités et quatre diagnostics

Nous examinons les **probabilités** `predict_proba` sur la validation et traçons quatre figures. Chaque figure contient **4 courbes** :  
- Régression logistique (distance)  
- Régression logistique (angle)  
- Régression logistique (distance + angle)  
- Baseline aléatoire (probas ~ U(0,1))

### a.Courbes ROC (avec AUC)
![ROC]({{ site.baseurl }}/baseline_roc.png)

**Remarques.**  
Les courbes ROC montrent que le modèle de régression logistique combinant **distance et angle** offre la **meilleure capacité de discrimination** (AUC ≈ 0,71), surpassant nettement les modèles utilisant une seule variable et le baseline aléatoire.

### b.Taux de but vs centile de probabilité
![Goal rate vs percentile]({{ site.baseurl }}/baseline_goal_rate_vs_percentile.png)

**Remarques.**  
Le taux de but augmente clairement avec le centile de probabilité prédit pour les modèles utilisant la **distance**, et encore davantage pour le modèle **distance + angle**, indiquant une **bonne capacité de calibration et de classement**, contrairement au baseline aléatoire qui reste quasi constant.

### c.Proportion cumulée de buts vs centile de probabilité
![Cumulative goals vs percentile]({{ site.baseurl }}/baseline_cumulative_goals_vs_percentile.png)

**Remarques.**  
Les courbes montrent que le modèle distance + angle concentre une proportion plus élevée de buts dans les centiles de probabilité les plus élevés, indiquant une meilleure capacité de priorisation des tirs dangereux que les modèles utilisant une seule variable ou le baseline aléatoire.

### d.Diagramme de fiabilité (calibration)
![Calibration]({{ site.baseurl }}/baseline_calibration.png)

**Remarques.**  
Le diagramme de fiabilité montre que le modèle distance + angle est globalement bien calibré, avec des probabilités prédites qui correspondent étroitement aux fréquences observées de buts sur l’ensemble des bins considérés.
## 3.Comparaison distance vs angle vs distance+angle

**Résumé.**  
- *Distance-only* : baseline raisonnable ;  
- *Angle-only* : utile mais moins discriminant que la distance (souvent) ;  
- *Distance+angle* : amélioration significative par rapport aux modèles mono-caractéristique.

Ces constats justifient l’emploi conjoint de **distance** et **angle** comme caractéristiques de base pour les modèles xG.

## 4.Suivi d’expériences (WandB)

Nous enregistrons chaque entraînement dans **WandB** avec le modèle sauvegardé comme **artifact** :

- Distance seulement : *(https://wandb.ai/haoran-sun-universite-de-montreal-/IFT6758.2025-A-3700/runs/315a2dz3?nw=nwuserhaoransun)*  
- Angle seulement : *(https://wandb.ai/haoran-sun-universite-de-montreal-/IFT6758.2025-A-3700/runs/ihymne1q?nw=nwuserhaoransun)*  
- Distance + angle : *(https://wandb.ai/haoran-sun-universite-de-montreal-/IFT6758.2025-A-3700/runs/t0h8jscy?nw=nwuserhaoransun)*  

Des **tags** explicites (ex. `baseline`, `logreg`, `distance`, `angle`) facilitent la navigation pour les étapes suivantes.
