---
layout: post
title: Ingénierie des caractéristiques II
---


# 🏒 *Analyse des tirs NHL — Ingénierie des caractéristiques (Étape 2)*

Dans cette étape, nous enrichissons notre jeu de données afin de modéliser la probabilité qu’un tir devienne un but (*expected goals* ou xG). L’objectif est de mieux représenter le **contexte du tir**, plutôt que de le considérer comme un événement isolé.

Nous intégrons (1) des caractéristiques propres au tir, (2) des informations sur l’événement immédiatement précédent, puis (3) des caractéristiques dérivées permettant de capturer la dynamique du jeu.

---

## 1.Caractéristiques liées au tir lui-même

| Nom de colonne | Description (interprétation)                                                |
| -------------- | --------------------------------------------------------------------------- |
| `game_seconds` | Nombre total de secondes écoulées dans le match au moment du tir.           |
| `period`       | Période du jeu (1, 2, 3 ou prolongation).                                   |
| `x`, `y`       | Coordonnées du tir sur la glace (système NHL).                              |
| `shotDistance` | Distance entre le point de tir et le filet (calculée en utilisant `hypot`). |
| `shotAngle`    | Angle du tir par rapport au centre du but (calculé à l’aide de `arctan2`).  |
| `shotType`     | Type de tir (`goal` ou `shot-on-goal`).                                     |
| `isGoal`       | Variable binaire indiquant si le tir est devenu un but.                     |

---

## 2.Contexte : informations sur l’événement précédent

À chaque tir, nous associons l’événement immédiatement précédent, qu’il s’agisse ou non d’un tir.

| Nom de colonne          | Description                                                         |
| ----------------------- | ------------------------------------------------------------------- |
| `last_eventType`        | Type de l’événement précédent (ex. passe, tir, récupération, etc.). |
| `last_x`, `last_y`      | Coordonnées de l’événement précédent.                               |
| `time_since_last_event` | Temps écoulé entre les deux événements (en secondes).               |
| `dist_from_last_event`  | Distance parcourue entre l’événement précédent et le tir actuel.    |

Ces informations permettent de contextualiser la séquence offensive.

---

## 3.Caractéristiques dérivées : dynamique de jeu

Nous dérivons ensuite trois caractéristiques clés :

| Nom de colonne            | Interprétation                                                                  |
| ------------------------- | ------------------------------------------------------------------------------- |
| `isRebound`               | 1 si l’événement précédent était un tir (`shot-on-goal` ou `goal`), sinon 0.    |
| `angle_change_on_rebound` | Variation d’angle entre deux tirs consécutifs, sinon 0.                         |
| `play_speed`              | Vitesse de progression du jeu = `dist_from_last_event / time_since_last_event`. |

Ces caractéristiques traduisent trois phénomènes du hockey offensif :

* Les **rebonds** créent des occasions dangereuses.
* Un **grand changement d’angle** oblige le gardien à se déplacer latéralement.
* Un **jeu rapide** avant le tir augmente souvent la probabilité de marquer.

---

## 4.Visualisations (Exploration des données)

### Figure 1 — Distribution de la distance des tirs

Les buts sont majoritairement marqués à courte distance du filet.
On observe que la distribution des tirs menant à un but est fortement concentrée sur les faibles distances, tandis que les tirs non concluants s’étendent sur une plage beaucoup plus large. Cette différence suggère que la distance au filet constitue une variable explicative fondamentale pour la probabilité de marquer.

![fig1_hist_shot_distance]({{ site.baseurl }}/assets/hist_distance_goals_vs_nongoals.png)

---

### Figure 2 — Angle vs Distance du tir (couleur = but / non-but)

Les buts sont associés à des angles de tir plus favorables.
Les tirs qui aboutissent à un but présentent en moyenne des angles plus petits, indiquant une meilleure ouverture sur le filet. À l’inverse, les tirs pris sous des angles extrêmes sont beaucoup plus fréquemment arrêtés ou manqués.

![fig2_hist_shot_angle]({{ site.baseurl }}/assets/hist_angle_goals_vs_nongoals.png)
---

### Figure 3 — Histogramme bidimensionnel distance × angle

La majorité des tirs est concentrée dans une zone restreinte devant le filet.
La densité maximale de tirs se situe à faible distance et à angle modéré, correspondant à la zone de l’enclave. Cette concentration reflète des schémas offensifs réalistes et confirme la cohérence spatiale des données utilisées.

![fig3_hist2d_distance_angle]({{ site.baseurl }}/assets/hist2d_distance_angle.png)

---

### Figure 4 — Taux de but en fonction de la distance et de l’angle

Le taux de but décroît rapidement avec l’augmentation de la distance et de l’angle.
Le taux de conversion est élevé pour les tirs pris près du filet et chute de manière monotone lorsque la distance augmente ou que l’angle devient plus défavorable. Cette observation justifie l’utilisation conjointe de la distance et de l’angle comme caractéristiques de base dans un modèle de buts espérés (xG).

![fig4_goal_rate_distance_angle]({{ site.baseurl }}/assets/goal_rate_vs_distance.png)

---

### Figure 5 — Buts par distance : filet vide vs filet non vide

Les buts marqués à très longue distance sont presque exclusivement associés à des situations de filet vide.
Lorsque le filet n’est pas vide, les buts provenant de distances élevées sont extrêmement rares, ce qui confirme la validité des données et met en évidence l’importance de distinguer les situations de filet vide dans l’analyse.

![fig5_goals_empty_net]({{ site.baseurl }}/assets/goals_by_distance_empty_vs_nonempty.png)
---

## 5.Export d’un sous-ensemble (WandB Dataset Artifact)

Nous filtrons notre jeu final pour le match :

> **Winnipeg vs Washington — 12 mars 2018**
> `game_id = 2017021065`

Puis nous l’enregistrons en tant qu’artefact :

```
Nom de l’artefact : wpg_v_wsh_2017021065  
Type : dataset
```

### 📎 Lien vers l’artefact

👉 [https://wandb.ai/]( https://wandb.ai/haoran-sun-universite-de-montreal-/IFT6758.2025-A-3700/artifacts/dataset/wpg_v_wsh_2017021065)


Ce dataset contient **toutes les caractéristiques listées ci-dessus** et servira de base à l’entraînement des modèles xG à l’étape suivante.

---

##  Conclusion

Cette étape nous a permis de passer d’une représentation isolée du tir à une représentation **contextuelle et dynamique**, ce qui est essentiel pour modéliser efficacement la probabilité qu’un tir aboutisse à un but.
Ces caractéristiques seront utilisées dans la prochaine étape pour **entraîner des modèles de prédiction xG**.


