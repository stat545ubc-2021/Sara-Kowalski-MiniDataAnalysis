STAT545A Individual Mini Data Analysis - Milestone 2
================
Sara Kowalski
October 16, 2021

## Table of Contents

1.  Introduction

2.  Load packages

    -   2.1 Load **datateachr** and **tidyverse**

3.  Task 1: Process and summarize *vancouver_trees*

    -   3.1 My 4 research questions
    -   3.2 Summarizing
        -   3.2.1 Question 1  
        -   3.2.2 Question 2
        -   3.2.3 Question 3
        -   3.2.4 Question 4
    -   3.3 Graphing
        -   3.3.1 Question 1
        -   3.3.2 Question 2
        -   3.3.3 Question 3
        -   3.3.4 Question 4
    -   3.4 Reflect on results

4.  Task 2: Tidy *vancouver_trees* dataset

    -   4.1 Is *vancouver_trees* tidy?
    -   4.2 untidy, then tidy back *vancouver_trees*
    -   4.3 Choose 2 research questions
    -   4.4. Choose a version of *vancouver_trees*

## 1 Introduction

Hello! The following document contains the work required to complete
Milestone 2 of the Mini Data Analysis Project in STAT545A. Here I will
be using R, and more specifically the tidyverse packages within R, to
summarize and visualize/graph the data within the *vancouver_trees*
dataset. By the end of this document I will have chosen 2 research
questions to continue to explore in Milestone 3 and generated a version
of my data that will help me to best answer those questions.

Much of the inspiration for this script comes from [Heads or
Tails](https://www.kaggle.com/headsortails/tidy-titarnic) and from the
[instructions](https://stat545.stat.ubc.ca/mini-project/mini-project-2/)
provided for the project.

## 2 Load Packages

### 2.1 Load **datateachr** and **tidyverse**

``` r
## Load dataset package - datateachr
library(datateachr)

## Load tidyverse - a collection of R packages used for data analysis
library(tidyverse) 
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──

    ## ✓ ggplot2 3.3.5     ✓ purrr   0.3.4
    ## ✓ tibble  3.1.4     ✓ dplyr   1.0.7
    ## ✓ tidyr   1.1.3     ✓ stringr 1.4.0
    ## ✓ readr   2.0.1     ✓ forcats 0.5.1

    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

## 3 Task 1: Process and summarize *vancouver_trees*

### 3.1 My 4 research questions

-   Question 1: What is the distribution for tree species (from a
    specified genus (e.g. Ulmus)) within Vancouver neighbourhoods? Are
    certain tree species favoured over others in specific
    neighbourhoods?

-   Question 2: How does the amount of trees planted change over time?
    Can go more in depth and look at how this changes within the
    different Vancouver neighbourhoods listed in the dataset. Or can
    look at how the number of trees planted changes for each genus or
    species over time. Can also look at a specific genus or species and
    see how the amount of trees planted changes over time within a
    specific neighbourhood or all the neighbourhoods in Vancouver.

-   Question 3: Within each genus, what is the distribution of the
    diameter of the trees for each species? Which species have a wider
    diameter distribution and which ones have a more narrow
    distribution?

-   Question 4: Which tree species are found planted on curbs? Does this
    vary by neighbourhood? Does the diameter of the tree determine
    whether it is planted on a curb (Y) or not (N) (i.e. large diameter
    trees would not be planted on curbs and smaller diameter trees would
    be)? Will have to group the trees by genus in order to pair down the
    dataset because for this question, it is too vast to look at in its
    entirety.
