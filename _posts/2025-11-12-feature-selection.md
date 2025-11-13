---
layout: post
title: Selection de Caracteristiques et XGBoost
---

## Objectif : Simplification des features pour XGBoost

L'objectif de cette analyse est d'explorer quelles caractéristiques sont réellement importantes pour prédire la probabilité qu'un tir devienne un but.  
Nous utilisons XGBoost et des techniques de sélection de caractéristiques pour identifier les features les plus pertinentes.

## Méthodologie

1. Prétraitement des données : suppression des valeurs manquantes et conversion des colonnes catégorielles en type category.
2. Séparation des données en train/validation.
3. Sélection des features importantes avec XGBoost (`SelectFromModel` basé sur l'importance des variables).
4. Recherche des meilleurs hyperparamètres avec GridSearchCV sur l'ensemble de features sélectionnées.
5. Évaluation du modèle sur l'ensemble de validation et visualisation via ROC.

## Features retenues

Les caractéristiques sélectionnées après le fit de XGBoost sont :  

`['game_id', 'event_idx', 'period', 'game_seconds', 'x', 'y', 'shotDistance', 'shotAngle', 'shotType', 'empty_net', 'eventType']`

Ces features représentent les informations les plus pertinentes pour prédire un but. Certaines colonnes initialement présentes étaient redondantes ou peu informatives et ont été exclues automatiquement par la sélection.

## Résultats

Le modèle entraîné sur ces features a obtenu :  

- Meilleurs hyperparamètres :  
`{'colsample_bytree': 0.8, 'learning_rate': 0.05, 'max_depth': 3, 'n_estimators': 100, 'subsample': 0.8}`
- AUC sur validation : **1.0**  
Indiquant une excellente séparation entre tirs réussis et ratés.

### Courbe ROC
La figure ci-dessous montre la capacité du modèle à distinguer les tirs qui deviennent but et ceux qui ne le deviennent pas.  

![ROC XGBoost Features Selection](assets/roc_xgboost_features_sel.png)

## Analyse

- Les features sélectionnées couvrent les dimensions temporelles (`game_seconds`, `period`), spatiales (`x`, `y`), de type de tir (`shotType`, `eventType`) et de contexte (`empty_net`).
- L'AUC de 1.0 montre que la combinaison de ces features permet au modèle de séparer parfaitement les tirs réussis des autres.
- Cette sélection réduit la complexité du modèle tout en conservant (voire améliorant) la performance par rapport à l'utilisation de toutes les features.

## Référence Wandb

Toutes les métriques et le modèle sont loggés sur Wandb :  
[XGBoost Feature Selection Run](https://wandb.ai/haoran-sun-universite-de-montreal-/IFT6758.2025-A-3700/runs/6m6fbhfs)

## Conclusion

La sélection de caractéristiques permet de simplifier l'entrée du modèle tout en conservant une très haute performance.  
Le modèle est désormais plus interprétable, plus léger et prêt à être utilisé pour des prédictions fiables.
