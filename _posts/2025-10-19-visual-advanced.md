---
layout: post
title: Visualisations Avancées
---

---
## Objectif  : Création des plans de tir offensifs interactifs

L’objectif de cette partie est de crée des cartes de tirs offensifs interactives pour toutes les équipes de la LNH pour chaque saison de 2016-17 à 2020-2021.  
Les graphiques produits permettra de visualiser la densité des tirs en zone offensive et de comparer la performance des équipes avec la moyenne de la ligue.

## Méthodologie

1. Calcul du taux de tir moyen par heure de la ligue
2. Regroupement par équipe et position sur la glace
3. Calcul de la différence vs moyenne ligue
4. Visualisation avec lissage gaussien

## Plans de Tir Interactifs
Les figures suivantes illustrent les cartes de tir offensif pour les saisons.  
Chaque graphique est interactif et permet de sélectionner l’équipe désirée.

### Saison 2016-2017 :
{% include plan_tir_2016_2017.html %}

### Saison 2017-2018 :
{% include plan_tir_2017_2018.html %}

### Saison 2018-2019 :
{% include plan_tir_2018_2019.html %}

### Saison 2019-2020 :
{% include plan_tir_2019_2020.html %}

### Saison 2020-2021 :
{% include plan_tir_2020_2021.html %}

## Analyses graph
Les graphiques illustrent la répartition moyenne des tirs offensifs dans la ligue, de la saison 2016-2017 à la saison 2020-2021, en fonction de la position et de la distance par rapport au filet (en pieds). On observe une concentration des tirs dans une zone spécifique à travers les saisons, principalement entre 70 et 83 pieds du filet et autour de ±10 pieds en position latérale centrale. Cette distance entre les tirs et le filet indique une fréquence élevée de tirs de longue distance. Cette tendance reste stable au fil des saisons et pourrait refléter une stratégie commune à de nombreuses équipes de la ligue.

On note également des zones de tirs moins denses à des distances plus rapprochées (entre 40 et 60 pieds du filet), ce qui suggère que les tirs de courte distance sont moins fréquents.

En examinant les équipes individuellement, on constate que la répartition de leurs tirs est généralement similaire à la moyenne de la ligue. Les légères différences observées pourraient refléter des styles de jeu ou des tactiques propres à chaque équipe.
### Colorado Avalanche : 2016-17 vs 2020-21

## Analyse de la saison 2016-2017:

 Grâce au heatmap des tirs, on voit que la concentration des tirs autour de 80 pieds du filet avec des zones de densité moins prononcées mais plus proche du filet sur les ailiers gauche et droite (± 25-30 pieds de la position latérale centrale). En général, le graphique donne une impression de tirs dispersés.

## Analyse de la saison 2020-2021:

Le graphique montre une concentration plus intense et centrée des tirs d'autour 75-83 pieds du filet en position latérale centrée. On voit aussi quelques zones concentrées à 40 pieds du filet au centre et à l'ailier droite. Cela indique une tactique de tirs à une distance où joueur peut avoir des avantages avec des tirs (rebonds, occasions secondaires, etc.). 

## Conclusion
L'Avalanche Colorado se trouve en dernière de sa ligue pour la saison 2016-2017 et l'équipe se trouve en top de sa ligue en 2020-2021. Ce classement est cohérent avec les graphiques, car comme mentionné en saison 2016-2017, les tirs sont plus dispersés, ce qui reflète une faible qualité offensive. Pour la saison 2020-2021, la concentration des tirs sur le heatmap montre les tirs de meilleurs qualité offensive avec une attaque focalisée sur le centre à partir de la ligne bleue.

### Sabres de Buffalo vs Lightning de Tampa Bay
Analyse de l'équipe Sabres de Buffalo :

Dans les 3 saisons, on remarque une concentration constante des tirs autour de 80 pieds du filet et au centre de la position latérale. Cette tactique reste inchangeé au fil des saisons. 

Cependant, on remarque à la saison 2018-2019 une concentration des tirs entre 35-45 pieds du filet sur les ailiers gauche et droite(±30 pieds de la position latérale centrale). Cela suggère une utilisation active des flancs pour les tirs de mi-distance.


Cette concentration sur les ailiers se réduit pour la saison 2019-2020, avec une présence marquée uniquement sur l'ailier gauche, suggérant un ajustement tactique privilégiant un côté spécifique


 Dans la saison, 2020-2021, les tirs sur les ailiers deviennent presque inexistants, marquant un changement de style de jeu.


 Analyse de l'équipe de Lightning Tempa Bay:

Tout comme l'équipe Sabres de Buffalo, on remarque une concentration constante des tirs de 80 pieds du filet en position latérale centrale au fil des saisons.

Pour la saison 2018-2019, on voit qu'une zone au 40 pieds du filet étendu de +25 à -25 pieds du centre de la position latérale. Cette concentration indique une dispersion des tirs. Cela indique une approche offensive diversifiée.

Pour la saison 2019-2020, on voit toujours ce style de jeu, avec moins de tendance de tirs sur l'ailier droite, suggérant un ajustement tactique.

Pour la saison 2020-2021, la tendance des tirs se réparti sur les ailiers gauche de droite avec une légère préférence sur l'ailier droite. Ce changement pourrait indique un retour à une stratégie plus équilibrée et affine sur les flancs. 

## Conclusion
On observe une dispersion des tirs au cours de la saison 2018-2019 pour l'équipe du Lightning de Tampa Bay. Son style de jeu s'est affiné au fil des saisons, avec une adaptation vers une stratégie offensive plus efficace, privilégiant l'utilisation des flancs. Chez les Sabres de Buffalo, l'utilisation des ailiers est presque inexistante dans les saisons plus récentes, ce qui pourrait expliquer les difficultés de l'équipe. Cela reflète une réduction progressive de la variété dans leur jeu, avec une dépendance croissante envers la zone centrale.

Les graphiques de répartition des tirs offensifs par saison pour les équipes offrent un aperçu limité de leurs stratégies offensives. Cependant, ils ne suffisent pas à expliquer pleinement le succès ou les difficultés d'une équipe, qui dépendent de nombreux autres facteurs, tels que la santé physique et mentale des joueurs, la chimie entre eux, et d'autres éléments.


