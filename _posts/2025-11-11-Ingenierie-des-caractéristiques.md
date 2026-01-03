---
layout: post
title: Ingénierie des caractéristiques I
---


## Ingénierie des caractéristiques I

Dans cette étape, notre objectif est de mieux comprendre la relation entre la position d’un tir et sa probabilité de devenir un but. Nous utilisons pour cela les données de **tous les matchs de saison régulière entre 2016/17 et 2019/20**, obtenues via l’API officielle de la LNH et prétraitées à l’étape 1.


---

## Construction du jeu de données de tirs

À partir du play-by-play, nous filtrons tous les événements de type **SHOT** ou **GOAL** et nous calculons, pour chaque tir, les caractéristiques suivantes :

* **Distance par rapport au filet** (`shot_distance`)
* **Angle par rapport au filet** (`shot_angle`)
* **Label binaire “est un but”** (`is_goal` = 1 si `eventType == GOAL`, 0 sinon)
* **Filet vide** (`empty_net` = 1 si `emptyNet == True`, sinon 0)

Pour calculer la distance et l’angle, nous supposons que les actions sont exprimées dans un repère où le tir est dirigé vers la droite (axe +x) et nous approchons le filet adverse par un point fixe `(x_net, y_net)`. Les coordonnées `(x, y)` des tirs situés dans la moitié gauche de la patinoire sont reflétées afin d’obtenir un repère cohérent : tous les tirs “regardent” le même but.

Le résultat est un fichier de type CSV, que nous noterons `shots_train_v2.csv`, contenant une ligne par tir et au moins les colonnes :

* `shot_distance`, `shot_angle`,
* `is_goal`, `empty_net`,
* ainsi que quelques métadonnées (période, force numérique, type de tir, etc.).

Ce jeu de données sera utilisé pour les visualisations ci-dessous et comme entrée principale de nos modèles xG.

---

## 1. Distribution des tirs en fonction de la distance et de l’angle

### 1.1 Histogramme des tirs par distance (buts vs non-buts)

![Histogramme des tirs par distance]({{ site.baseurl }}/assets/hist_distance_goals_vs_nongoals.png)


Dans cette figure, nous représentons le **nombre de tirs** en fonction de la distance au filet, en séparant les tirs qui se terminent par un but (`is_goal = 1`) de ceux qui ne marquent pas (`is_goal = 0`).
 
**Observations :**

* La grande majorité des tirs proviennent de **distances relativement courtes** devant le filet.
* Les **buts** sont beaucoup plus concentrés dans cette zone proche : la courbe des buts décroît plus rapidement avec la distance que celle des tirs non convertis.
* À longue distance, les tirs restent fréquents, mais la proportion de buts est nettement plus faible.

Ces observations confirment une intuition simple : **plus le tir est proche du filet, plus il a de chances d’être converti**.

---

### 1.2 Histogramme des tirs par angle (buts vs non-buts)

![Histogramme des tirs par angle]({{ site.baseurl }}/assets/hist_angle_goals_vs_nongoals.png)

Cette figure montre la distribution des tirs en fonction de l’**angle relatif au filet**. Un angle proche de 0° correspond à un tir presque en face du gardien, tandis que des angles plus élevés (en valeur absolue) correspondent à des tirs depuis des positions plus excentrées.

**Observations :**

* Les tirs sont fortement concentrés dans les angles faibles à modérés, avec une majorité des tentatives situées entre 0° et ±60° par rapport à l’axe du filet.

* En comparant les buts aux tirs totaux, on observe que la proportion de buts est légèrement plus élevée lorsque |angle| < 60°, par rapport aux tirs très excentrés.

* Pour des angles plus extrêmes (|angle| > 60°), bien que des tirs soient encore tentés, la probabilité de marquer diminue sensiblement, ce qui suggère une efficacité offensive réduite depuis des positions très fermées.

---

### 1.3 Histogramme 2D distance × angle

![Histogramme 2D distance vs angle]({{ site.baseurl }}/assets/hist2d_distance_angle.png)

Dans cet **histogramme 2D**, un axe représente la distance et l’autre l’angle. Les couleurs indiquent le nombre de tirs dans chaque “case” distance–angle.

**Observations :**

* La plus forte densité de tirs se situe à courte distance et pour des angles faibles à modérés, reflétant les positions depuis lesquelles les joueurs tentent le plus souvent de tirer.

* Lorsque la distance augmente ou que l’angle devient très fermé, la fréquence des tirs diminue nettement, indiquant que ces positions sont moins accessibles ou moins privilégiées en situation de jeu.

Cette visualisation décrit donc la géographie des tentatives de tir, mais ne permet pas à elle seule d’inférer la dangerosité réelle de ces tirs en termes de probabilité de but.

---

## 2. Taux de but en fonction de la distance et de l’angle

Jusqu’ici, nous ne regardions que des **comptes de tirs**. Pour la modélisation xG, ce qui nous intéresse vraiment, c’est le **taux de but** associé à chaque type de tir, que l’on peut estimer empiriquement comme :

[
\text{taux de but} = \frac{#\text{buts}}{#\text{buts} + #\text{non-buts}}.
]

Nous calculons ce taux dans des **intervalles (bins)** de distance ou d’angle, puis traçons la courbe correspondante.

### 2.1 Taux de but vs distance

![Taux de but en fonction de la distance]({{ site.baseurl }}/assets/goal_rate_vs_distance.png)

**Observations :**

* Le taux de but est maximal à très courte distance, ce qui reflète une probabilité de conversion élevée pour les tirs pris près du filet.

* À mesure que la distance augmente, le taux de but diminue globalement, bien que la courbe présente des fluctuations dues au bruit statistique, en particulier dans les bins peu peuplés.

* Pour les longues distances, le taux de but devient très faible et relativement stable, suggérant que ces tirs ont une contribution marginale à la probabilité globale de marquer.

Cette relation confirme que la distance est un facteur explicatif majeur dans la modélisation xG, mais qu’elle doit être combinée à d’autres variables (angle, type de tir, situation de jeu) pour capturer pleinement la qualité d’une occasion.
---

### 2.2 Taux de but vs angle

![Taux de but en fonction de l’angle]({{ site.baseurl }}/assets/goal_rate_vs_angle.png)

**Observations :**

* Le taux de but est le plus élevé pour des angles **proches de 0°** (face au filet).
* Lorsque l’angle s’écarte de l’axe central, le taux de but **diminue progressivement**.
* Aux angles très extrêmes, la probabilité de marquer devient très faible, même si des buts restent possibles.

En combinant ces résultats avec ceux de la section précédente, on voit que **distance et angle capturent deux dimensions complémentaires de la qualité d’un tir**. Ces deux caractéristiques seront donc des entrées naturelles pour nos modèles xG.

---

## 3. Vérification des données : filet vide vs filet non vide

Avant d’aller plus loin dans la modélisation, il est important de vérifier que les données ne contiennent pas d’erreurs grossières. Nous disposons d’une certaine **connaissance du domaine**, notamment le fait suivant :

> Il est extrêmement rare de marquer un but sur un filet **non vide** depuis l’intérieur de sa propre zone défensive.

Pour exploiter cette information, nous regardons uniquement les **buts** (`is_goal = 1`), et nous traçons un histogramme des distances en séparant les **filets vides** (`empty_net = 1`) et **non vides** (`empty_net = 0`).

![Buts par distance (filet vide vs non vide)]({{ site.baseurl }}/assets/goals_by_distance_empty_vs_nonempty.png)

**Observations :**

* Le taux de but est maximal en moyenne pour des angles proches de 0°, correspondant à des tirs pris face au filet.

* À mesure que l’angle s’écarte de l’axe central, le taux de but tend globalement à diminuer, malgré certaines fluctuations dues au bruit statistique et au découpage en intervalles (binning).

* Pour des angles très extrêmes, la probabilité de marquer devient faible, même si des buts restent possibles de manière occasionnelle.

En combinant ces résultats avec ceux obtenus pour la distance, on observe que l’angle et la distance capturent deux dimensions complémentaires de la qualité d’un tir. Ces deux variables constituent donc des entrées naturelles et pertinentes pour la modélisation des buts attendus (xG).

---

## 4. Conclusion de cette étape d’ingénierie des caractéristiques

Dans cette première phase d’ingénierie des caractéristiques, nous avons :

* construit un **jeu de données propre au niveau du tir** pour les saisons régulières 2016/17–2019/20 ;
* mis en évidence, à l’aide d’**histogrammes**, la distribution des tirs et des buts en fonction de la distance et de l’angle ;
* montré que le **taux de but** décroît fortement avec la distance et l’angle, ce qui justifie l’utilisation de ces variables comme caractéristiques de base pour un modèle xG ;
* réalisé quelques **vérifications de qualité des données** en exploitant la distinction entre filets vides et non vides.

Ce jeu de données (`shots_train.csv`) servira de base à la suite du projet, où nous testerons différents modèles statistiques et de machine learning (régression logistique, modèles d’arbres, etc.) pour prédire la probabilité qu’un tir donné se transforme en but. Les figures présentées ici constituent le socle intuitif et empirique de ces modèles : elles montrent comment la notion de “qualité du tir” émerge naturellement à partir de la distance et de l’angle.


