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

## Library of Names 

You can thank [Gary Hutson](https://nhsrcommunity.com/blog/author/garyhutson/) for the below method of loading and or  installing packages


```r
install_or_load_pack <- function(pack){

  create.pkg <- pack[!(pack %in% installed.packages()[, "Package"])]

  if (length(create.pkg))

    install.packages(create.pkg, dependencies = TRUE)

  sapply(pack, require, character.only = TRUE)
  
}

packages <- c("randomNames")
install_or_load_pack(packages)
```

```
## randomNames 
##        TRUE
```
## The Code block
 


```r
my_rand_gen <-  function() {
  col <-
    sample(gsub('[[:digit:]]+', '',  unique(colours(distinct = TRUE))), 1)
  my_name <- sub("\\s.*",
                 "",
                 randomNames::randomNames(
                   n = length(col),
                   ethnicity = 5,
                   name.sep = " "
                 ))
  for (i in col) {
    for (j in my_name)
      random_name <- paste(i, j)
  }
  return(random_name)
}
```
## Give it a spin


```r
my_rand_gen()
```

```
## [1] "darkorange Bly"
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
 
 
 As colours were differentiated by a number suffix, the removal of the number  will create a number of identical entries so we drop those using `unique()`


```r
col <- unique(col)
```

`length()` gives us the number of elements in `col`


```r
length(col)
```

```
## [1] 128
```



Using `randomNames` we generate random names equal to the length of the `col` vector
That gets wrapped in a `sub()` function that extracts all characters BEFORE the first space


```r
my_name <- sub("\\s.*","", randomNames::randomNames(n=length(col),ethnicity = 5,
                                                    name.sep = " "))
```


Next we use a nested for loop


```r
for(i in col){
  for(j in my_name)
  random_name <- paste(i,j)
}
print(random_name)
```

```
## [1] "yellow Wiens"
```


