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

### 3.2 Summarizing

To explore the *vancouver_trees* dataset more in depth, I will use
various data manipulation techniques to help me complete a summarization
activity for each research question listed above.

#### 3.2.1 Question 1

For this question it would be useful to know how many tree species there
are for a given tree genus and how many Vancouver neighbourhoods there
are in the dataset. Knowing these counts, I can determine how much data
I am working with and how big my graphs will be. This will help me to
determine the best way of presenting the data and inform my decision on
whether to subset the data further or not. With that being said, I will
use the *distinct* function from the *dplyr* package to determine the
amount of observations for the *neighbourhood_name* variable and the
*species_name* variable in the *vancouver_trees* dataset.

``` r
## first call the data set that will be used 
vancouver_trees %>%
## next call the distinct function and input the variable from the dataset you wish to know the count for 
  distinct(neighbourhood_name)
```

    ## # A tibble: 22 × 1
    ##    neighbourhood_name      
    ##    <chr>                   
    ##  1 MARPOLE                 
    ##  2 KENSINGTON-CEDAR COTTAGE
    ##  3 OAKRIDGE                
    ##  4 MOUNT PLEASANT          
    ##  5 RENFREW-COLLINGWOOD     
    ##  6 RILEY PARK              
    ##  7 DOWNTOWN                
    ##  8 SUNSET                  
    ##  9 ARBUTUS-RIDGE           
    ## 10 GRANDVIEW-WOODLAND      
    ## # … with 12 more rows

``` r
## repeat as necessary for any other variables within the dataset. 
vancouver_trees %>%
## I want to know the amount of distinct species within a particular tree genus so I will first filter
## the dataset to only contain trees from that genus 
  filter(genus_name == "ULMUS") %>%
  distinct(species_name)
```

    ## # A tibble: 7 × 1
    ##   species_name  
    ##   <chr>         
    ## 1 AMERICANA     
    ## 2 GLABRA        
    ## 3 HOLLANDICA   X
    ## 4 PUMILA        
    ## 5 PROCERA       
    ## 6 CARPINIFOLIA  
    ## 7 SPECIES

Based on the tibble outputs, there are **22 distinct neighbourhoods**
within the *vancouver_trees* dataset and **7 distinct species** within
the Ulmus genus in the dataset. I think it would also be interesting to
take this analysis one step further and see the count for the amount of
trees in each of the 7 disinct species in the Ulmus genus. Since each
row in the dataset is a tree, I can compute this using the function
*count*.

``` r
## call the dataset
vancouver_trees %>%
## filter the data so it only contains trees with the genus Ulmus 
  filter(genus_name == "ULMUS") %>%
## use the count function and input the variable you want the disinct values for
  count(species_name)
```

    ## # A tibble: 7 × 2
    ##   species_name       n
    ##   <chr>          <int>
    ## 1 AMERICANA       2252
    ## 2 CARPINIFOLIA      17
    ## 3 GLABRA           273
    ## 4 HOLLANDICA   X    20
    ## 5 PROCERA           74
    ## 6 PUMILA           203
    ## 7 SPECIES           22

Based on this futher analysis, I can see that the species *AMERICANA*
has the most counts (2252) while *CARPINIFOLIA* has the least (17).

**In conclusion** : I have successfully determined the counts for the
amount of neighbourhoods within the dataset and the number of species
within the Ulmus genus while taking it a step further and determining
the amount of trees per species within the Ulmus genus. This
summarization exercise gives me a better idea at the amount of data I am
working with and how it could be further subsetted. For instance, I may
want to group the neighbourhoods into quadrants (north, west, east, and
south) and that way I can look at larger number of genus and species of
trees within each quadrant instead of each neighbourhood.

#### 3.2.2 Question 2

For this research question, instead of looking at how trees planted
changes over time, I could look at how trees planted changes depending
on the 4 seasons: summer, fall, winter, and spring. More specifically, I
can look and see if there is a specific season in which trees are
preferably planted and see if this varies by neighbourhood or type of
tree (genus or species). A useful summarization activity for this
question would be to create a new categorical variable called *season*
from the date_planted variable to determine which season each tree was
planted in. For the purposes of this activity, I will define the 4
seasons as follows:

-   summer = July (7) - September (9)
-   fall = October (10) - December (12)
-   winter = January (1) - March (3)
-   spring = April (4) - June (6)

In order to do this I must first add a variable to the dataset called
*month* that will hold numerical data corresponding to the month that
each tree was planted based on the trees *date_planted* value. To ensure
reproducibility of the code and ease of comprehension, I will create a
new dataset called *numericMonth* which will store this added variable
*months* along with the original data from the *vancouver_trees*
dataset.

``` r
## define where the data created will be stored and which dataset will be used to create the new data
numericMonth <- vancouver_trees %>%
## add the new variable with the mutate function
## to access the month of each date use the months.Date function
## to further change the month into a numerical variable use the as.numeric and as.factor functions
  mutate(month = as.numeric(as.factor(months.Date(date_planted))))
## use the glimpse function to confirm the addition of the new variable - month
glimpse(numericMonth)
```

    ## Rows: 146,611
    ## Columns: 21
    ## $ tree_id            <dbl> 149556, 149563, 149579, 149590, 149604, 149616, 149…
    ## $ civic_number       <dbl> 494, 450, 4994, 858, 5032, 585, 4909, 4925, 4969, 7…
    ## $ std_street         <chr> "W 58TH AV", "W 58TH AV", "WINDSOR ST", "E 39TH AV"…
    ## $ genus_name         <chr> "ULMUS", "ZELKOVA", "STYRAX", "FRAXINUS", "ACER", "…
    ## $ species_name       <chr> "AMERICANA", "SERRATA", "JAPONICA", "AMERICANA", "C…
    ## $ cultivar_name      <chr> "BRANDON", NA, NA, "AUTUMN APPLAUSE", NA, "CHANTICL…
    ## $ common_name        <chr> "BRANDON ELM", "JAPANESE ZELKOVA", "JAPANESE SNOWBE…
    ## $ assigned           <chr> "N", "N", "N", "Y", "N", "N", "N", "N", "N", "N", "…
    ## $ root_barrier       <chr> "N", "N", "N", "N", "N", "N", "N", "N", "N", "N", "…
    ## $ plant_area         <chr> "N", "N", "4", "4", "4", "B", "6", "6", "3", "3", "…
    ## $ on_street_block    <dbl> 400, 400, 4900, 800, 5000, 500, 4900, 4900, 4900, 7…
    ## $ on_street          <chr> "W 58TH AV", "W 58TH AV", "WINDSOR ST", "E 39TH AV"…
    ## $ neighbourhood_name <chr> "MARPOLE", "MARPOLE", "KENSINGTON-CEDAR COTTAGE", "…
    ## $ street_side_name   <chr> "EVEN", "EVEN", "EVEN", "EVEN", "EVEN", "ODD", "ODD…
    ## $ height_range_id    <dbl> 2, 4, 3, 4, 2, 2, 3, 3, 2, 2, 2, 5, 3, 2, 2, 2, 2, …
    ## $ diameter           <dbl> 10.00, 10.00, 4.00, 18.00, 9.00, 5.00, 15.00, 14.00…
    ## $ curb               <chr> "N", "N", "Y", "Y", "Y", "Y", "Y", "Y", "Y", "Y", "…
    ## $ date_planted       <date> 1999-01-13, 1996-05-31, 1993-11-22, 1996-04-29, 19…
    ## $ longitude          <dbl> -123.1161, -123.1147, -123.0846, -123.0870, -123.08…
    ## $ latitude           <dbl> 49.21776, 49.21776, 49.23938, 49.23469, 49.23894, 4…
    ## $ month              <dbl> 5, 9, 10, 1, 3, NA, 3, 3, 3, 3, 3, 3, 3, 10, 10, 3,…

Now I can change the numeric variable *month* into a categorical
variable *season* that holds the 4 seasons (groups): summer, fall,
winter, and spring. I will store this in a new dataset called
*season_van_trees_planted*.

``` r
## define where the data created will be stored and which dataset will be used to create the new data
season_van_trees_planted <- numericMonth %>%
## use the mutate function to add the new variable season 
## use the cut function to create the categorical variable season with 4 groups (summer, fall, winter,
## spring) from the numeric variable month 
## winter(1-3), spring(4-6), summer(7-9), fall(10-12)
  mutate(season = cut(month, breaks = c(1,3,6,9,12), labels = c("winter", "spring", "summer", "fall")))

## use the glimpse function to confirm the addition of the new variable - season 
glimpse(season_van_trees_planted)
```

    ## Rows: 146,611
    ## Columns: 22
    ## $ tree_id            <dbl> 149556, 149563, 149579, 149590, 149604, 149616, 149…
    ## $ civic_number       <dbl> 494, 450, 4994, 858, 5032, 585, 4909, 4925, 4969, 7…
    ## $ std_street         <chr> "W 58TH AV", "W 58TH AV", "WINDSOR ST", "E 39TH AV"…
    ## $ genus_name         <chr> "ULMUS", "ZELKOVA", "STYRAX", "FRAXINUS", "ACER", "…
    ## $ species_name       <chr> "AMERICANA", "SERRATA", "JAPONICA", "AMERICANA", "C…
    ## $ cultivar_name      <chr> "BRANDON", NA, NA, "AUTUMN APPLAUSE", NA, "CHANTICL…
    ## $ common_name        <chr> "BRANDON ELM", "JAPANESE ZELKOVA", "JAPANESE SNOWBE…
    ## $ assigned           <chr> "N", "N", "N", "Y", "N", "N", "N", "N", "N", "N", "…
    ## $ root_barrier       <chr> "N", "N", "N", "N", "N", "N", "N", "N", "N", "N", "…
    ## $ plant_area         <chr> "N", "N", "4", "4", "4", "B", "6", "6", "3", "3", "…
    ## $ on_street_block    <dbl> 400, 400, 4900, 800, 5000, 500, 4900, 4900, 4900, 7…
    ## $ on_street          <chr> "W 58TH AV", "W 58TH AV", "WINDSOR ST", "E 39TH AV"…
    ## $ neighbourhood_name <chr> "MARPOLE", "MARPOLE", "KENSINGTON-CEDAR COTTAGE", "…
    ## $ street_side_name   <chr> "EVEN", "EVEN", "EVEN", "EVEN", "EVEN", "ODD", "ODD…
    ## $ height_range_id    <dbl> 2, 4, 3, 4, 2, 2, 3, 3, 2, 2, 2, 5, 3, 2, 2, 2, 2, …
    ## $ diameter           <dbl> 10.00, 10.00, 4.00, 18.00, 9.00, 5.00, 15.00, 14.00…
    ## $ curb               <chr> "N", "N", "Y", "Y", "Y", "Y", "Y", "Y", "Y", "Y", "…
    ## $ date_planted       <date> 1999-01-13, 1996-05-31, 1993-11-22, 1996-04-29, 19…
    ## $ longitude          <dbl> -123.1161, -123.1147, -123.0846, -123.0870, -123.08…
    ## $ latitude           <dbl> 49.21776, 49.21776, 49.23938, 49.23469, 49.23894, 4…
    ## $ month              <dbl> 5, 9, 10, 1, 3, NA, 3, 3, 3, 3, 3, 3, 3, 10, 10, 3,…
    ## $ season             <fct> spring, summer, fall, NA, winter, NA, winter, winte…

**In conclusion** I have successfully created a new categorical variable
called *season* with 4 groups: winter, spring, summer, and fall, out of
transformed date data. In further analysis, I would likely filter out
all the NA values from the data to clean it up or graph it with NA as
it’s own group. This summarization exercise will help to graph the data
when I explore the relationship between species of trees and the month
they were planted in.

#### 3.2.3 Question 3

For this research question, it may be helpful to look at a handful of
summary statistics for the diameter of the trees across the species of a
specific genus. For this exercise I will be calculating the **range**,
**mean**, **median**, and **standard deviation** of tree diameters for
each species within the Ulmus genus. Computing these summary statistics
will help me understand how the distribution of tree diameter might vary
between different species, and could help me to ask deeper questions to
develop a more complex analysis about the relationship between tree
diameter and species.

I will compute these statistics by utilizing the *summarize* function
within the *dplyr* package.

``` r
## call the dataset
vancouver_trees %>%
## filter the data for the Ulmus genus 
  filter(genus_name == "ULMUS") %>%
## group the data by species 
  group_by(species_name) %>%
## calculate the range, mean, median, and standard deviation of tree diameter for each species with the
## summarize function
  summarize(avg_treeDiameter = mean(diameter), range_treeDiameter = range(diameter), median_treeDiameter = median(diameter), sd_treeDiameter = sd(diameter))
```

    ## `summarise()` has grouped output by 'species_name'. You can override using the `.groups` argument.

    ## # A tibble: 14 × 5
    ## # Groups:   species_name [7]
    ##    species_name   avg_treeDiameter range_treeDiameter median_treeDiameter
    ##    <chr>                     <dbl>              <dbl>               <dbl>
    ##  1 AMERICANA                  22.4               1.25                24.5
    ##  2 AMERICANA                  22.4             144                   24.5
    ##  3 CARPINIFOLIA               30.1               8.5                 29.5
    ##  4 CARPINIFOLIA               30.1              38.5                 29.5
    ##  5 GLABRA                     25.1               3                   25  
    ##  6 GLABRA                     25.1              43                   25  
    ##  7 HOLLANDICA   X             28.1              13                   27.5
    ##  8 HOLLANDICA   X             28.1              41                   27.5
    ##  9 PROCERA                    27.0              12                   27.2
    ## 10 PROCERA                    27.0              53                   27.2
    ## 11 PUMILA                     22.1               3                   24  
    ## 12 PUMILA                     22.1              46                   24  
    ## 13 SPECIES                    19.8               3                   19.5
    ## 14 SPECIES                    19.8              42                   19.5
    ## # … with 1 more variable: sd_treeDiameter <dbl>

**In conclusion** I have successfully calculated summary statistics
about the diameters of the trees within the species of the Ulmus genus.
Based on the results, I can see that the species *AMERICANA* has the
largest range of diameters with the minimum being 1.25 and the maximum
being 144.00 and thus if I were to plot this data, I would predict that
the *AMERICANA* species would have the widest distribution of tree
diameters. It would take more calculations to determine this with
certainty, however, these initial calculations give me a good start at
answering my original research question.

#### 3.2.4 Question 4

This research question would also benefit from the summarization
exercise performed above for question 3. However, to look at this in a
different light and further explore my data, I will perform similar
calculations as the ones above with a slight twist. Instead of grouping
the trees by species and then calculating diameter summary statistic, I
will group the trees by whether (Y) or not (N) they have been planted on
a curb. This will let me look at the types of diameters trees on curbs
have vs. those not planted on curbs. For this exercise I will calculate
the **range**, **mean**, **IQR**, and **standard deviation**. The trees
will be filtered by genus, specifically looking at the *ACER* genus.

``` r
## call the dataset
vancouver_trees %>%
## filter the data for the Acer genus 
  filter(genus_name == "ACER") %>%
## group the data by curb 
  group_by(curb) %>%
## calculate the range, mean, IQR, and standard deviation of tree diameter for each species with the
## summarize function
  summarize(avg_treeDiameter = mean(diameter), range_treeDiameter = range(diameter), IQR_treeDiameter = IQR(diameter), sd_treeDiameter = sd(diameter))
```

    ## `summarise()` has grouped output by 'curb'. You can override using the `.groups` argument.

    ## # A tibble: 4 × 5
    ## # Groups:   curb [2]
    ##   curb  avg_treeDiameter range_treeDiameter IQR_treeDiameter sd_treeDiameter
    ##   <chr>            <dbl>              <dbl>            <dbl>           <dbl>
    ## 1 N                 11.7                  0               15            9.74
    ## 2 N                 11.7                161               15            9.74
    ## 3 Y                 10.5                  0               12            8.66
    ## 4 Y                 10.5                317               12            8.66

**In conclusion** I have successfully calculated summary statistics
about the diameters of the trees planted on curbs (Y) vs. those not
planted on curbs (N) within the Acer genus. Based on the results, I can
see that trees planted on curbs have a slightly lower average (mean)
tree diameter compared to those not planted on curbs. However, it is
also evident that trees planted on curbs have a much larger range of
tree diameters compared to those not planted on curbs. This is a good
start to answering my full research question and could help me to subset
my data for further analysis.
