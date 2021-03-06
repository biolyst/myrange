---
title: 'R Script Automation on Windows '
author: ''
date: '2020-12-19'
slug: []
categories:
  - programming
  - R
  - Automation
tags: []
---

## Automation made simple  

Automation of R scripts is an incredibly powerful tool to have at your disposal. 
In its simplest form, the ease by which you can automate an R script is almost laughable.

You do not need much to start. You will need an R script of your choice, here I give a fun yet largely useless example. 
You will need a text editor (notepad will do fine) to create your batch file. 

*Also you must have administrator privileges to execute this file as it executes in command prompt.*



```r
setwd("C:\\Location\\of\\Choice")


# Thank you to Gary Hutson for the below function
install_or_load_pack <- function(pack){

  create.pkg <- pack[!(pack %in% installed.packages()[, "Package"])]

  if (length(create.pkg))

    install.packages(create.pkg, dependencies = TRUE)

  sapply(pack, require, character.only = TRUE)
  
}

# Next you need to bring in the relevant libraries. 

packages <-c("dplyr",'wikipediatrend',"lubridate", "ggplot2")

install_or_load_pack(packages)

options(scipen = 999)

# Create Dynamic Date Ranges
today <- Sys.Date()
week_from_today <-  Sys.Date()-10
# Create search term for page
cyberpunk <- 'Cyberpunk_2077'
# create the query
wiki_search <- wp_trend(page = cyberpunk ,
         from = week_from_today,
         to = today)
wiki_search <-  as_tibble(wiki_search)
 wiki_search <- wiki_search %>% 
  mutate(date = date(date),day = day(date))
views_plot <- wiki_search %>% 
  ggplot(aes(date,views,col=article)) +
  geom_point(size=4,col='orange',alpha=0.3)+
  geom_line(size=1,show.legend=FALSE)+
  theme_minimal() +
  scale_y_continuous(labels = scales::comma)+
  scale_color_viridis_d() +
  labs(title = 'Cyberpunk_2077 Wikipedia Page Last 10 Days')


ggsave(filename = paste("C:\\Location\\of\\Choice\\",Sys.Date(),'cyberpunk_views.png',sep = "_"))
```

Once you have done this, you will then also save your R script in the same folder. 


## The Batch File
The batch file will be saved in the same folder as where you have saved your R script and your ggplot2 output using ggsave

The batch file it self will only contain the following lines of code and must be saved as `.bat`

![Code Snippet](C:\\Users\\ald04\\Documents\\myrange\\content\\post\\2020-12-19-r-script-automation-on-windows\\code.PNG)


```[r,eval=FALSE]

@echo off
C:
PATH "C:\Program Files\R\R-3.6.2\bin\x64"
cd C:\Location\of\Choice
Rscript NameOfScript.R

pause

```
The `PATH` is directed to where R is installed


The change directory (cd) command takes you to where you saved your R script and the output. 



### Windows Task Scheduler 

Now that we have the R script and the batch file ready, its time to get into windows task scheduler.


On the right hand side, under `Actions` select `Create Task`


Under `General` give it an appropriate name 


Under `Triggers` select `New` and choose your time frame


Under `Actions` select `New`, choose the `Start a Program` option and then browse to find the `.bat` file you created


Simple as that, you've automated your first R script. 






