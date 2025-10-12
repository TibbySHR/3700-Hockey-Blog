---
layout: post
title: Nettoyer les données
---


## Nettoyage des données

Nous avons créé une fonction `extract_shots_and_goals()` qui transforme les fichiers JSON bruts en un DataFrame propre contenant uniquement les tirs cadrés et les buts.

### Exemple de sortie

![DataFrame head](includes/df_head.jpg)

### Discussion

1. **Force du jeu (strength)**  
   Si cette information n’était disponible que pour les buts, on pourrait la déduire pour les tirs à partir des pénalités en cours et du nombre de joueurs sur la glace.

2. **Caractéristiques supplémentaires possibles :**  
   - Distance entre le tir et le but.  
   - Angle du tir par rapport au centre du filet.  
   - Tirs de rebond (détectés si le tir survient < 3 s après un autre tir de la même équipe).

