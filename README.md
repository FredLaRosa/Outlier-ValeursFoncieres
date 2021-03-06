# Outlier-ValeursFoncieres
Analysis of abnormal real estate transactions in France.


```{r setup, include=FALSE}
knitr::opts_chunk$set
library(dplyr)
library(ggplot2)
```

## Importation Database

Nous allons importer notre dataset.

```{r, eval=TRUE}
valeur_foncieres2020 <- read.delim2(
  file = "data/valeursfoncieres-2020 - Copie.txt",
  sep = "|",
  header = TRUE,
  dec = ",",
  encoding = "UTF-8"
)
```

On présente le tableau ici:

```{r eval=FALSE}
str(valeur_foncieres2020)
```


J'analyse le nombre de ventes effectuées en 2020:

count sert à compter

```{r}
valeur_foncieres2020 %>%
  filter(
    Nature.mutation == "Vente"
  ) %>% 
  count()
```


J'analyse le nombre de ventes effectuées en 2020 par villes:

group_by sert à regrouper
arrange sert à classer par ordre `asc` sauf si on lui précise descending `desc`

```{r}
valeur_foncieres2020 %>%
  filter(
    Nature.mutation == "Vente"
  ) %>%
  group_by(Commune) %>% 
  count() %>% 
  arrange(desc(n))
```


J'analyse le nombre de ventes effectuées en 2020 par villes, ainsi que le volume représenté en Euro

summarise sert à faire une fonction d'agrégation :

- `max` = retourne la valeur maximum de la colonne
- `sum` = retourne la somme des valeurs de la colonne
- `mean` = retourne la moyenne
- `median` = retourne la médianne

```{r}
valeur_foncieres2020 %>%
  filter(
    Nature.mutation == "Vente"
  ) %>%
  group_by(Commune) %>% 
  summarise(sum = sum(Valeur.fonciere), n = n()) %>% 
  arrange(desc(n), desc(sum))
```


# Analyse par m²

- Surface carrez

```{r}
valeur_foncieres2020 %>% 
  filter(
    Nature.mutation == "Vente"
  ) %>% 
  group_by(Surface.Carrez.du.1er.lot) %>% 
  summarise(
    prix = sum(Valeur.fonciere),
    prixmax = max(Valeur.fonciere)
  )
```

- Surface réelle

```{r}
valeur_foncieres2020 %>% 
  filter(
    Nature.mutation == "Vente"
  ) %>% 
  group_by(Surface.reelle.bati) %>% 
  summarise(
    prix = sum(Valeur.fonciere),
    prixmax = max(Valeur.fonciere)
  )
```



