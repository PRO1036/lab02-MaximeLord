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
  filter(plastic_waste_per_cap < 3.5)
```

## Exercices

### Exercise 1

``` r
ggplot(plastic_waste, aes(plastic_waste_per_cap)) +
  geom_histogram(binwidth = 0.05) +
  facet_wrap(~continent, ncol = 3)
```

![](lab-02_files/figure-gfm/plastic-waste-continent-1.png)<!-- -->

Avec l’aide de cette représentation on voit facilement que l’Afrique et
l’Asie on moins de déchets de plastique par habitant que l’Europe et
l’Amérique du Nord alors que l’Océanie et l’Amérique du Sud se situent à
quelque part au milieu.

### Exercise 2

``` r
ggplot(plastic_waste, aes(plastic_waste_per_cap,color = continent, fill = continent)) + 
  geom_density(alpha = 0.3)
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
# insert code here
```

Violin plot:

``` r
# insert code here
```

Réponse à la question…

### Exercise 4

``` r
# insert code here
```

Réponse à la question…

### Exercise 5

``` r
# insert code here
```

``` r
# insert code here
```

Réponse à la question…

## Conclusion

Recréez la visualisation:

``` r
# insert code here
```
