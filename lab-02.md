Lab 02 - Plastic waste
================
Maxime Lord
22 Septembre 2025

## Chargement des packages et des données

``` r
library(tidyverse) 
```

``` r
plastic_waste <- read_csv("data/plastic-waste.csv")
```

Commençons par filtrer les données pour retirer le point représenté par
Trinité et Tobago (TTO) qui est un outlier.

``` r
plastic_waste <- plastic_waste %>%
  filter(plastic_waste_per_cap < 3)
```

## Exercices

### Exercise 1

``` r
ggplot(plastic_waste, aes(plastic_waste_per_cap)) +
  geom_histogram(binwidth = 0.05) +
  facet_wrap(~continent, ncol = 3) +
  labs(title = "Distribution des pays selon la quantitée de déchets plastiques par habitant",
       x = "Quantité de déchets plastiques par habitants",
       y = "Nombre de pays")
```

![](lab-02_files/figure-gfm/plastic-waste-continent-1.png)<!-- -->

Avec l’aide de cette représentation on voit facilement que l’Afrique et
l’Asie ont moins de déchets de plastique par habitant que l’Europe et
l’Amérique du Nord alors que l’Océanie et l’Amérique du Sud se situent à
quelque part au milieu.

### Exercise 2

``` r
ggplot(plastic_waste, aes(plastic_waste_per_cap, fill = continent)) + 
  geom_density(alpha = 0.3) +
  labs(x = "Quantité de déchets plastiques par habitants",
       y = "Densité",
       title = "Densité de pays produisant une certaine quantitée de déchets plastiques",
       subtitle = "Pour chaque continent",
       fill = "Continent")
```

![](lab-02_files/figure-gfm/plastic-waste-density-1.png)<!-- -->

La couleur, soit par la fonction `color`, ou par la fonction `fill`, est
une variable. Celle-ci va varier en fonction du continent qu’elle
représente. C’est pourquoi qu’elle doit se retrouver dans
l’environnement `aes`, là où on identifie les variables. Pour ce qui est
du paramètre `alpha`, il est le même pour toute les courbes et est
propre à la courbe de densité. Dans un autre style de représentation, il
aurait une signification complètement différente. C’est donc la raison
de son identification dans l’environnement `geom_density`.

### Exercise 3

Boxplot:

``` r
ggplot(plastic_waste, aes(continent, plastic_waste_per_cap))+
  geom_boxplot() +
  labs(x = "Continent",
       y = "Quantité de déchets plastiques par habitants", 
       title = "Quantité de déchets plastiques par habitants pour chaque pays en fonction du continent")
```

![](lab-02_files/figure-gfm/plastic-waste-boxplot-1.png)<!-- -->

Violin plot:

``` r
ggplot(plastic_waste, aes(continent, plastic_waste_per_cap))+
  geom_violin() +
  labs(x = "Continent",
       y = "Quantité de déchets plastiques par habitants", 
       title = "Quantité de déchets plastiques par habitants pour chaque pays en fonction du continent")
```

![](lab-02_files/figure-gfm/plastic-waste-violin-1.png)<!-- -->

Contrairement aux box plots, les violin plots offrent une meilleure
visualisation de la distribution des données autour de la moyenne à
l’aide d’une courbe de densité combinée aux mesures de tendances
centrales des box plots.

### Exercise 4

``` r
ggplot(plastic_waste, aes(plastic_waste_per_cap, mismanaged_plastic_waste_per_cap, color = continent))+
  geom_point() +
  labs(y = "Quantitée de déchets plastiques mals 
       gérés par habitant",
       x = " Quantité de déchets plastiques par habitant", 
       title ="Quantitée de déchets plastiques mals gérés par habitant en fonction de la 
       Quantité de déchets plastiques par habitant" )
```

![](lab-02_files/figure-gfm/plastic-waste-mismanaged-1.png)<!-- -->

En général, la relation est plutôt linéaire, plus il y a de déchets
produits, plus il y en aura qui seront mal gérés. Pour ce qui est de la
répartitions selon les continents, la seule observation qu’il est
possible de faire est celle de pays Africains. Ceux-ci produisent très
peu de déchet il y en a donc très peu qui sont mal gérés. Les autres
continents sont répartis plutôt uniformément dans le graphe.

### Exercise 5

``` r
ggplot(plastic_waste, aes(total_pop, plastic_waste_per_cap))+
  geom_point() +
  labs(y = "Quantité de déchets plastiques par habitant",
       x = "Population totale",
       title = "Quantité de déchets plastiques par habitant en fonction de la population totale")
```

    ## Warning: Removed 10 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](lab-02_files/figure-gfm/plastic-waste-population-total-1.png)<!-- -->

``` r
ggplot(plastic_waste, aes(coastal_pop, plastic_waste_per_cap))+
  geom_point()+
  labs(y = "Quantité de déchets plastiques par habitant",
       x = "Population côtière",
       title = "Quantité de déchets plastiques par habitant en fonction de la population côtière")
```

![](lab-02_files/figure-gfm/plastic-waste-population-coastal-1.png)<!-- -->

Même si les deux graphes sont très semblables, il semble que la relation
entre la quantité de déchet par habitant et la population totale vivant
sur une côte soit légèrement plus forte que celle entre la quantité de
déchet par habitant et la population totale. Plus il y aurait d’habitant
sur la côte et moins il y aurait de déchet de plastique par habitant.

## Conclusion

Recréez la visualisation:

``` r
plastic_waste_coastal <- plastic_waste %>% 
  mutate(coastal_pop_prop = coastal_pop / total_pop) %>%
  filter(plastic_waste_per_cap < 3)
```

``` r
ggplot(plastic_waste_coastal, aes(x = coastal_pop_prop, y = plastic_waste_per_cap, color = continent))+
  geom_point()+
  geom_smooth(method = "loess", se = TRUE, color = "black") +
  labs(x = "Proportion de la population côtière (Coastal/total population)",
       y = "Nombre de déchets plastiques par habitant",
       color = "Continent",
       title = "Quantité de déchets plastiques vs Proportion de la population côtière",
       subtitle = "Selon le continent")
```

    ## `geom_smooth()` using formula = 'y ~ x'

    ## Warning: Removed 10 rows containing non-finite outside the scale range
    ## (`stat_smooth()`).

    ## Warning: Removed 10 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](lab-02_files/figure-gfm/unnamed-chunk-1-1.png)<!-- --> On voit avec
ce graphe qu’il y a une légère augmentation du nombre de déchets
plastiques par habitant lorsque la proportion de la population côtière
augmente.
