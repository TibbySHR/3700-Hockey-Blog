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

[
\textbf{Les buts proviennent en moyenne de distances plus courtes.}
]

![fig1\_hist\_shot\_distance]({{ site.baseurl }}/assets/fig1_hist_shot_distance.png)

---

### Figure 2 — Angle vs Distance du tir (couleur = but / non-but)

[
\textbf{Zone dangereuse = faible distance + petit angle.}
]

![fig2\_scatter\_angle\_distance]({{ site.baseurl }}/assets/fig2_scatter_angle_distance.png)

---

### Figure 3 — Carte de densité des tirs (hexbin, coordonnées x–y)

[
\textbf{Concentration nette dans l’enclave devant le filet.}
]

![fig3\_hexbin\_xy]({{ site.baseurl }}/assets/fig3_hexbin_xy.png)

---

### Figure 4 — Distribution de la vitesse du jeu (rebond vs non-rebond)

[
\textbf{Les rebonds sont souvent associés à des phases de jeu plus rapides.}
]

![fig4\_hist\_play\_speed]({{ site.baseurl }}/assets/fig4_hist_play_speed.png)

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

### 📎 Lien vers l’artefact (à insérer ici)

👉 [https://wandb.ai/](https://wandb.ai/haoran-sun-universite-de-montreal-/ift6758-stage2/runs/i5uth3g5)


Ce dataset contient **toutes les caractéristiques listées ci-dessus** et servira de base à l’entraînement des modèles xG à l’étape suivante.

---

##  Conclusion

Cette étape nous a permis de passer d’une représentation isolée du tir à une représentation **contextuelle et dynamique**, ce qui est essentiel pour modéliser efficacement la probabilité qu’un tir aboutisse à un but.
Ces caractéristiques seront utilisées dans la prochaine étape pour **entraîner des modèles de prédiction xG**.


