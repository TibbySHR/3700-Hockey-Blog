---
layout: post
title: "Modèle XGBoost Baseline"
---

## Objectif : Évaluation d’un modèle XGBoost de base

Cette partie présente l’entraînement d’un modèle XGBoost de base pour prédire si un tir se transformera en but.  
on veut fournir une référence de performance avant d’appliquer l’optimisation des hyperparamètres.

## Méthodologie

- Sélection des features principales : **distance** et **angle** du tir  
- Conversion des colonnes catégorielles en type approprié  
- Séparation des données en train/validation  
- Entraînement d’un XGBoost de base (`n_estimators=100`, `max_depth=3`, `learning_rate=0.1`)  
- Évaluation sur l’ensemble de validation avec AUC et courbe ROC

## Résultats

- **AUC (distance + angle)** : 0.718  
- **Figure ROC** : ![ROC XGBoost Baseline](assets/roc_xgboost_baseline.png)

La courbe ROC montre que le modèle de base distingue correctement les tirs qui deviennent buts des tirs ratés, mais elle est moins précise que le modèle tuné avec toutes les features.

## Analyse

On remarque que l’AUC est correcte pour un modèle simple utilisant seulement deux variables, mais il reste un potentiel important d’amélioration.  
Les erreurs de classification proviennent principalement des tirs à distances intermédiaires ou des angles particuliers, où le modèle ne dispose pas d’informations supplémentaires pour différencier les tirs efficaces des tirs ratés.

## Conclusion

Le modèle XGBoost baseline sert de référence pour évaluer les gains obtenus après le tuning des hyperparamètres et l’utilisation de l’ensemble complet des features.  
Il permet également de valider la qualité des données et la méthodologie de prétraitement avant d’appliquer des modèles plus complexes.
