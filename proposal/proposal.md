Proposition de projet
================
Simon Duchesne, Thibault Masson, Nicolas Dufour, Olivier Bouchard

<style>
body {
  text-align: justify
}
</style>

``` r
library(tidyverse)
library(broom)
library(skimr)
```

    ## Warning: le package 'skimr' a été compilé avec la version R 4.2.2

## 1. Introduction

Le domaine de la science des données est en pleine effervescence avec
l’intégration de l’intelligence artificiel et l’apprentisage machine. Ce
millieu croit avec un ensemble d’emplois diversifié et ce partout dans
le monde avec différent niveau d’emploi au travers d’un entreprise.
Ainsi, selon les données dans notre dataset, nous voulons répondre à la
question suivante : Est-ce que les individues travaillant au États-Unis
pour des compagnies américaines sont ceux les mieux rémunérés dans le
domaine des sciences des données?

Nous utilisons un jeu de données disponible sur le site
<https://ai-jobs.net>. Ce site à pour but premier détablire le lien
entre l’employeur et les candidats, il permet aussi d’accumuler des
données sur les salaires annuel payé. Le but étant de recueillir de
l’information sur les salaires dans le milieu de la science des données
et de l’intelligence artificielle et dans découvrir les tendances. Les
données collecté sont rendu publique a même le site.

La méthodologie pour collecté l’information nous donnes un échantillon
non probabiliste. Car le formulaire est rempli de façons volontaire sur
le site internet. Cette méthode pour receuillir les données permets
difficilement de contrôler une bonne distribution de la population.

## 2. Données

    ## Rows: 1,332
    ## Columns: 11
    ## $ work_year          <dbl> 2022, 2022, 2022, 2022, 2022, 2022, 2022, 2022, 202…
    ## $ experience_level   <chr> "MI", "MI", "MI", "MI", "MI", "MI", "SE", "SE", "SE…
    ## $ employment_type    <chr> "FT", "FT", "FT", "FT", "FT", "FT", "FT", "FT", "FT…
    ## $ job_title          <chr> "Machine Learning Engineer", "Machine Learning Engi…
    ## $ salary             <dbl> 130000, 90000, 120000, 100000, 85000, 78000, 161000…
    ## $ salary_currency    <chr> "USD", "USD", "USD", "USD", "USD", "USD", "USD", "U…
    ## $ salary_in_usd      <dbl> 130000, 90000, 120000, 100000, 85000, 78000, 161000…
    ## $ employee_residence <chr> "US", "US", "US", "US", "US", "US", "US", "US", "US…
    ## $ remote_ratio       <dbl> 0, 0, 100, 100, 100, 100, 100, 100, 100, 100, 0, 0,…
    ## $ company_location   <chr> "US", "US", "US", "US", "US", "US", "US", "US", "US…
    ## $ company_size       <chr> "M", "M", "M", "M", "M", "M", "M", "M", "M", "M", "…

|                                                  |             |
|:-------------------------------------------------|:------------|
| Name                                             | ds_salaries |
| Number of rows                                   | 1332        |
| Number of columns                                | 11          |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_   |             |
| Column type frequency:                           |             |
| character                                        | 7           |
| numeric                                          | 4           |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ |             |
| Group variables                                  | None        |

Data summary

**Variable type: character**

| skim_variable      | n_missing | complete_rate | min | max | empty | n_unique | whitespace |
|:-------------------|----------:|--------------:|----:|----:|------:|---------:|-----------:|
| experience_level   |         0 |             1 |   2 |   2 |     0 |        4 |          0 |
| employment_type    |         0 |             1 |   2 |   2 |     0 |        4 |          0 |
| job_title          |         0 |             1 |  10 |  40 |     0 |       64 |          0 |
| salary_currency    |         0 |             1 |   3 |   3 |     0 |       18 |          0 |
| employee_residence |         0 |             1 |   2 |   2 |     0 |       64 |          0 |
| company_location   |         0 |             1 |   2 |   2 |     0 |       59 |          0 |
| company_size       |         0 |             1 |   1 |   1 |     0 |        3 |          0 |

**Variable type: numeric**

| skim_variable | n_missing | complete_rate |      mean |         sd |   p0 |   p25 |    p50 |    p75 |     p100 | hist  |
|:--------------|----------:|--------------:|----------:|-----------:|-----:|------:|-------:|-------:|---------:|:------|
| work_year     |         0 |             1 |   2021.72 |       0.56 | 2020 |  2022 |   2022 |   2022 |     2022 | ▁▁▂▁▇ |
| salary        |         0 |             1 | 237712.38 | 1077368.81 | 2324 | 80000 | 130000 | 175100 | 30400000 | ▇▁▁▁▁ |
| salary_in_usd |         0 |             1 | 123374.66 |   65945.87 | 2324 | 75593 | 120000 | 164997 |   600000 | ▇▇▁▁▁ |
| remote_ratio  |         0 |             1 |     63.85 |      45.26 |    0 |     0 |    100 |    100 |      100 | ▅▁▂▁▇ |

## 3. Plan d’analyse de données

Selon notre hypothèse et les variables fournies dans notre dataset, nous
pouvons conclure que notre variable de réponse (variable cible) est le
salaire en dollars US. Les variables explicatives, c’est-à-dire les
variables influençant notre variable cible sont l’année de l’emploie,
l’expérience, le type d’emploie, le titre du travail, l’endroit de
résidence, la proportion en télétravail, la localisation de la compagnie
et la grosseur de cette dernière. En effet, ils agissent tous de
conséquence sur l’augmentation ou la diminution du salaire d’une
personne.

Selon notre hypothèse, les groupes de comparaison sont les pays, les
personnes en télétravail, les grosseurs des compagnies, le titre des
travails et l’expérience. En effet, pour que notre analyse soit efficace
et sensée, nous devons diviser ces catégories en sous-catégorie.
Dépendamment de plusieurs facteurs, une donnée salariale peut-être
avantagée par rapport aux autres.

Nous devrons utiliser la variable du salaire en devise USD pour la
comparer à l’endroit, le type d’emploi et titre de l’emploi. Nous
pourrons aussi comparer le salaire à la grosseur de l’entreprise ou bien
à la quantité de travaille à distance.

Voici une première visualisation de la répartition du salaire :

![](proposal_files/figure-gfm/visualisation%20salaire-1.png)<!-- -->

Le résultat obtenu semble logique par rapport aux données statistiques
récapitulatives obtenues plus haut. Réalisons maintenant une seconde
observation du salaire mais selon l’année :

![](proposal_files/figure-gfm/visualisation%20salaire%20par%20annee-1.png)<!-- -->

Cette visualisation montre 3 répartitions de salaire qui ont la même
forme mais on peut voir qu’il y a beaucoup plus de salaires en 2022 donc
plus d’employés. Cela nous prouve bien qu’il est nécessaire de comparer
le salaire avec les différentes variables catégoriques de jeu de données
pour en dégager des informations permettant de découvir les meilleures
opportunités d’emploi dans le milieu de la science des données.

Pour ce qui est des méthodes qui seront employées, nous avons observé
dans notre exploration préliminaire du jeu de données que la majorité
des variables sont catégorielles. Nous utiliserons donc beaucoup
d’histogrammes (bar plot) pour démontrer la comparaison entre les
différentes valeurs et variables. Ensuite, des tests de dépendance
statistique entre nos variables prédictives choisies et la variable de
résultat seront pertinents. De plus, notre valeur réponse est continue,
il serait donc intéressant d’employer les boîtes à moustache (box plot)
pour mettre en lumière nos écarts inter-quartiles ainsi que nos valeurs
abérantes tel que nos minimums et maximums et ce de façon imagé grâce au
graphique.

Pour ce qui est des histogrammes nous voulons en tirer une façon
efficace de visualiser et présenter nos données, nous aurons alors
besoin de faire du sens entre les relations de nos différentes variables
et la variable réponse afin de découvrir la meilleure combinaison pour
obtenir les meilleurs opportunités d’emplois. Par ailleurs, nos tests de
dépendance serviront à s’assurer que nos variables prédictives ont en
effet une influence directe sur notre variable résultat. Finalement, les
boîtes à moustache vont faire ressortir les valeurs abérantes comme
mentionné ci-haut, il pourrait alors être pertinent de retirer les
extrêmes afin de faire ressortir la moyenne du salaire US la plus fidèle
possible au jeu de données.
