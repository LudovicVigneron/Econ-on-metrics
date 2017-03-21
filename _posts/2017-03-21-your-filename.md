---
published: false
---
---
title: "Exercice_4_3"
author: "Ludovic Vigneron"
date: "17 mars 2017"
output:
  html_document: default
  pdf_document: default
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

<p align="justify">Dans le cadre de cet exercice, il vous est demandé de télécharger les cours journalier sur les trois dernières années des actions faisant actuellement (au 17 mars 2017 dans cet exemple) partie de l'indice CAC 40  en utilisant le wrapper API **BatchGetSymbols**.</p>

<p align="justify">Celui-ci est composé des 40 plus grosses capitalisations boursières flottantes (le volume des actions détenu dans une optique d'échange et non de contrôle) des entreprises cotées sur la place de Paris. Sa composition est régulièrement corrigée de manière à tenir compte de l'évolution des valorisations, de l'arrivé de nouveaux titres important et de la disparition de certains. Les décisions en la matière sont prises par le conseil scientifique des indices qui se réunit au moins 4 fois par an. Les réunions de ce conseil ne sont pas annoncées à l'avance.</p>

<p align="justify">Il a été créé le 31 décembre 1987. Date à laquelle, il prend la valeur 1000. Son code ISIN est FR0003500008 et son ticker ^FCHI. L'évolution de sa valeur ne prend pas en compte le réinvestissement des dividendes.</p>

## Obtenir les tickers des entreprises composant le CAC 40. 

<p align="justify">Dans la mesure où l'API que nous allons utiliser fonctionne à partir des Tickers, la première étape du travail consiste donc à obtenir l'ensemble des Tickers pour les titres composant l'indice à notre date de référence (pour moi le 17 mars 2017).</p> 

<p align="justify">Pour cela, nous chargerons un fichier Excel les contenant à partir du site https://www.abcbourse.com/download/libelles.aspx. Chargez la page dans votre navigateur, puis cochez CAC40 dans la rubrique "Composition des indices". Terminez l'opération en cliquant sur télécharger.</p>

<p align="justify">Vous pouvez alors Copier le fichier obtenu "libelles.csv" dans votre dossier de travail et charger son contenu dans R grâce à **read.csv()** en précisant que le séparateur retenu est le ';'. Nommez la data frame obtenue **libelles** et visualisez en les 5 premières lignes.</p>

```{r chunck_1}
libelles<-read.csv('libelles.csv',sep=';')
head(libelles)
```

<p align="justify">**libelles** contient les code ISIN, nom et ticker des 40 entreprises composant l'indice. Nous ne sommes intéressés que par le dernier élément. Créons donc un vecteur de type 'character' nommé tickers qui le reprendra seul. Ajoutons à ce vecteur le suffixe '.PA' afin d'être sûr que l'extraction se fera bien sur les données associées à la place de Paris grâce à la fonction **paste()**. Veillons à ce que, ce faisant, aucun espace ne se glisse dans l'expression du ticker. Pour cela, utilisons la fonction **gsub()**.</p>

```{r chunck_2}
tickers<-gsub(" ","",paste(as.character(libelles$ticker),'.PA'))
tickers
```

## Charger les cours des actions sur trois ans

<p align="justify">Maintenant que nous avons notre liste de ticker nous pouvons utiliser notre wrapper d'API pour obtenir les cours de bourses associés à chaque titre entre aujourd'hui (hiers le 16 mars 2017) et il y a trois ans (16 mars 2014).Pour cela commençons par chager le package **BatchGetSymbols**. Puis utilisons notre vecteur tickers dans la fonction d'extraction **BatchGetSymbols()**.</p>

```{r chunk_3}
library(BatchGetSymbols)
Extraction_1<-BatchGetSymbols(tickers=tickers,first.date = Sys.Date()-(3*365),last.date = Sys.Date())

```

Enter text in [Markdown](http://daringfireball.net/projects/markdown/). Use the toolbar above, or click the **?** button for formatting help.
