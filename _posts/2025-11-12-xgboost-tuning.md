---
layout: post
title: Tuning et Visualisation XGBoost
---

## Objectif : Amélioration des prédictions de buts

L'objectif de cette analyse est de tester et d'optimiser un modèle XGBoost pour prédire la probabilité qu'un tir devienne un but.  
Les figures générées permettent de visualiser la performance du modèle et sa calibration par rapport aux données réelles.

## Méthodologie

1. Prétraitement des données (suppression des valeurs manquantes et conversion des colonnes catégorielles)
2. Séparation des données en train/validation
3. Recherche des meilleurs hyperparamètres avec GridSearchCV
4. Evaluation du modèle sur l'ensemble de validation
5. Visualisation des performances via ROC, taux de buts par percentile, proportion cumulée de buts et courbe de calibration

## Résultats du tuning XGBoost

Les meilleurs paramètres obtenus pour le modèle sont :  
`{'colsample_bytree': 0.8, 'learning_rate': 0.05, 'max_depth': 3, 'n_estimators': 100, 'subsample': 0.8}`  
Avec un AUC de validation de **1.0**, indiquant une excellente séparation entre les tirs qui qui finisse par un but et ceux qui ne le deviennent pas.

### Courbe ROC
La figure ci-dessous montre la capacité du modèle à distinguer les tirs réussis des tirs ratés.  
On compare également avec une ligne de base aléatoire.

![ROC XGBoost Tuned](assets/roc_xgboost_tuned.png)

### Taux de buts vs percentile
Cette figure montre la proportion de tirs transformés en buts dans chaque percentile de probabilité prédite.  
On voit que les percentiles élevés correspondent bien aux tirs les plus probables de finir en but.

![Goal Rate vs Percentile XGBoost Tuned](assets/goal_rate_percentile_XGBoost_tuned.png)

### Proportion cumulée de buts
La figure illustre la proportion cumulée de buts en fonction des percentiles de probabilité.  
Elle permet de visualiser à quel point le modèle concentre les vrais buts dans les probabilités les plus élevées.

![Cumulative Goals XGBoost Tuned](assets/cumulative_goals_XGBoost_tuned.png)

### Courbe de calibration
Cette figure compare la probabilité prédite par le modèle avec la probabilité observée.  
Une courbe proche de la diagonale indique un bon calibrage.

![Calibration Curve XGBoost Tuned](assets/calibration_XGBoost_tuned.png)

## Analyse

On observe que le modèle XGBoost_tuned identifie très bien les tirs qui deviennent buts.  
Le taux de buts augmente progressivement avec le percentile, et la proportion cumulée montre que les tirs les plus probables sont correctement mis en avant.  
La courbe de calibration indique que les probabilités prédites sont fiables, avec seulement de légères déviations par rapport à la diagonale parfaite.

## Conclusion

Le tuning des hyperparamètres a permis d'obtenir un modèle performant, capable de prédire efficacement les tirs transformés en buts.  
Les visualisations générées permettent non seulement d'évaluer la performance globale du modèle, mais aussi d'examiner comment il se comporte sur différents segments de probabilité.
