---
date: "2018-09-09T00:00:00Z"
draft: false
lastmod: "2018-09-09T00:00:00Z"
linktitle: Survival Analysis (LÝÐ079F)
menu:
  course:
    name: Preface
    weight: 1
summary: Course notes in SurvivaL Analysis
title: Survival Analysis
toc: true
type: docs
weight: 1
math: true
markup: mmark
---


# Preface 

***

## The Book 

These lecture notes are based on the book [Modelling Survival Data in Medical Research by David Collett](https://www.crcpress.com/Modelling-Survival-Data-in-Medical-Research/Collett/p/book/9781439856789)

![](https://images.tandf.co.uk/common/jackets/amazon/978143985/9781439856789.jpg)

## Software 

We will primarily be writing R code using the RStudio GUI.

## Setup in RStudio 

This is our base setup. Further packages will be loaded in the individual lectures respectively.




```r
library(tidyverse); library(kableExtra); library(cowplot); library(scales); library(janitor);
theme_set(theme_cowplot() +  
              background_grid(major = "xy", 
                              minor = "xy",
                              size.major = 0.25,
                              size.minor = 0.1) +
              theme(legend.position = "top",
                    plot.margin = margin(10, 10, 10, 10)))
```
