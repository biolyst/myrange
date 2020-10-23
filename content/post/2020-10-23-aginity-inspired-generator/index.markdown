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
## [1] "brown Ash"
```
