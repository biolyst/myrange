---
title: Aginity Inspired Generator
author: ''
date: '2020-10-23'
slug: []
categories:
  - programming
  - R
tags: []
---
## A random random gen 
This was inspired by Aginity's tab name format. 

![](/post/2020-10-23-aginity-inspired-generator/randfunction.png)

## Load 'em up 

You can thank [Gary Hutson](https://nhsrcommunity.com/blog/author/garyhutson/) for the below method of loading and or  installing packages


```r
install_or_load_pack <- function(pack){

  create.pkg <- pack[!(pack %in% installed.packages()[, "Package"])]

  if (length(create.pkg))

    install.packages(create.pkg, dependencies = TRUE)

  sapply(pack, require, character.only = TRUE)
  
}

packages <- c("randomNames","Hmisc")
install_or_load_pack(packages)
```
## The Code block
 


```r
name_generator <- function(selection_pool, final_selection) {
  col <- colours(distinct = TRUE)
  col <- gsub('[[:digit:]]+', '', col)
  col <- unique(col)
  col <- Hmisc::capitalize(col)
  my_name <-
    sub("\\s.*",
        "",
        randomNames::randomNames(
          n = length(col),
          name.sep = " "
        )
      )
  name_vector <-
    paste(sample(col, size = selection_pool),
          sample(my_name, size = selection_pool))
  print(sample(name_vector, final_selection))
}
```
## Give it a spin


```r
name_generator(selection_pool = 100,final_selection = 10)
```

```
##  [1] "Cornflowerblue Maes"    "Springgreen Lenard"     "White el-Reza"         
##  [4] "Turquoise Claudio"      "Oldlace Thomas"         "Darkslategray Bankett" 
##  [7] "Indianred Lilly"        "Papayawhip Deskin"      "Mediumseagreen Perez"  
## [10] "Lavenderblush Wotortsi"
```


***
***
***



## Breaking it down

Using the inbuilt `colours()` function in R we extract colour names

```r
col <- colours(distinct = TRUE)
```


Use regex to remove any numbers

```r
col <- gsub('[[:digit:]]+', '', col)
```
 
 
 As colours were differentiated by a number suffix, the removal of the number  will create a number of identical entries.
 
 
 So we drop those using `unique()`


```r
col <- unique(col)
```

Lets capitalise the first letter of each of the elements in `col`


```r
col <- Hmisc::capitalize(col)
```



Using `randomNames` we generate random names equal to the length of the `col` vector
That gets wrapped in a `sub()` function that extracts all characters BEFORE the first space


```r
my_name <-
    sub("\\s.*",
        "",
        randomNames::randomNames(
          n = length(col),
          name.sep = " "
        )
      )
```


Next we use `sample()` to  select a pre-defined number of elements from both vectors
and then again we use `sample()` to return a pre-defined number of names. The idea was to make it *EXTRA* random


```r
  name_vector <-
    paste(sample(col, size = selection_pool),
          sample(my_name, size = selection_pool))
  print(sample(name_vector, final_selection))
```


