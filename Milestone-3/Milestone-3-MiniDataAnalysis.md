STAT545A Individual Mini Data Analysis-Milestone 3
================
Sara Kowalski
October 30, 2021

## Table of Contents

1.  Introduction

2.  Load packages

    -   2.1 Load **datateachr** and **tidyverse**
    -   2.2 My research questions

3.  Task 1: Special data types

    -   3.1 Reordering factors
    -   3.2 Modifying time

4.  Task 2: Modelling

    -   4.1 Research question and variable of interest
    -   4.2 Hypothesis testing
        -   4.2.1 P-value

5.  Task 3: Reading and writing data

    -   5.1 Write a csv file
    -   5.2 Write a R binary file

## 1 Introduction

Hello! The following document contains the work required to complete
Milestone 3 of the Mini Data Analysis Project in STAT545A. Here I will
be using R, and more specifically the tidyverse packages within R, to
explore special data types, like reordering factors and modifying time
data, model data and perform hypothesis testing, and read and write csv
and RDS files. I will be using the *vancouver_trees* dataset to complete
the tasks outlined in this milestone and more specifically, where
appropriate, I will be completing these tasks in relation to the two
research questions I chose at the end of milestone 2.

Much of the inspiration for this script comes from [Heads or
Tails](https://www.kaggle.com/headsortails/tidy-titarnic) and from the
[instructions](https://stat545.stat.ubc.ca/mini-project/mini-project-3/)
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

### 2.2 My research questions

1.  Question 1: What is the distribution for tree species (from a
    specified genus (e.g. Ulmus)) within Vancouver neighbourhoods? Are
    certain tree species favored over others in specific neighbourhoods?

2.  Question 2: Within each genus, what is the distribution of the
    diameter of the trees for each species? Which species have a wider
    diameter distribution and which ones have a more narrow
    distribution?

## 3 Task 1: Special data types

### 3.1 Reordering factors

For this task I have decided to take a plot I produced in milestone 2
(to answer question 1) which involved plotting across the three groups:
*neighbourhood_name*, *number of trees*, and *species_name*, and produce
a new plot with the neighbourhoods reordered in ascending order
(i.e. the neighbourhood with the least amount of trees on the far left
with it increasing up to the neighbourhood with the most amount of trees
on the far right). I will complete this task using the tools in the
**forcats** package which was designed for working with and reordering
factors.

Here is the code and a visual of the original plot I created in
Milestone 2 which looked at how many trees in the **STYRAX** genus were
planted in each Vancouver neighbourhood. The graph was further subsetted
to show how many of the different types of **STYRAX** species were
planted in each neighbourhood by colouring the bars based on the
*species_name*.

``` r
## define a new place to hold the data to be graphed and call the dataset to be used
styraxGenus <- vancouver_trees %>%
## filter that data so it only contains trees with the STYRAX genus
  filter(genus_name == "STYRAX")

## plot the data using ggplot, set x axis to neighbourhood_name
ggplot(styraxGenus, aes(neighbourhood_name, fill = species_name)) +
## set the graph to a bar graph
  geom_bar() +
## define the x and y axis labels 
  xlab("Neighbourhood") +
  ylab("Number of Trees") +
## declutter the bottom axis by switching x and y 
  coord_flip()
```

![](Milestone-3-MiniDataAnalysis_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

To more clearly see how the number of trees planted changes by
neighbourhood, I am going to reorder the neighbourhoodds so they appear
in ascending order from left to right based on the number of **STYRAX**
trees planted. I can achieve this by using the **forcats** function
*fct_infreq()* which reorders a factor (neighbourhood) by its number of
values (trees planted).

``` r
## define a new place to hold the data to be graphed and call the dataset to be used
styraxGenus <- vancouver_trees %>%
## filter that data so it only contains trees with the STYRAX genus
  filter(genus_name == "STYRAX") %>%
## use the mutate function to reorder the neighbourhoods from least amount of trees planted to most
  mutate(neighbourhood_name = fct_infreq(neighbourhood_name))


## plot the data using ggplot, set x axis to neighbourhood_name
ggplot(styraxGenus, aes(neighbourhood_name, fill = species_name)) +
## set the graph to a bar graph
  geom_bar() +
## define the x and y axis labels 
  xlab("Neighbourhood") +
  ylab("Number of Trees") +
## declutter the bottom axis by switching x and y 
  coord_flip() 
```

![](Milestone-3-MiniDataAnalysis_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

By reordering the neighbourhood_name variable in ascending order based
on the number of trees planted, you can more clearly see which
neighbourhoods have a high number of **STYRAX** trees planted and which
have low numbers. Further, you can actually distinguish and be confident
as to which neighbourhood has the most **STYRAX** trees (Kitsilano) and
which neighbourdhood has the least (West End); whereas, before
reordering, it was very difficult to be certain since Shauhnessy and
Kitsilano are very close in number of trees as are West End and
Strathcona. Finally, by reordering the data like this, if there is a
trend for a certain area or quadrant of the city to have more **STYRAX**
trees, then it would become more apparent and could lead to more
research questions.

### 3.2 Modifying time

In the dataset *vancouver_trees* there is a date column which holds the
date of when a tree was planted. For this exercise of modifying a
time-based column, I am going to use the **format()** function from the
**lubridate** package to modify the *date_planted* column by extracting
only the year the tree was planted.

``` r
## define the new data subset and call the dataset to be modified 
van_trees_year_planted <- vancouver_trees %>%
## use the format function within the mutate function to modify the existing date_planted column 
## and extract only the year
  mutate(date_planted = format(date_planted, format = "%Y"))
## visualize the new dataset
glimpse(van_trees_year_planted)
```

    ## Rows: 146,611
    ## Columns: 20
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
    ## $ date_planted       <chr> "1999", "1996", "1993", "1996", "1993", NA, "1993",…
    ## $ longitude          <dbl> -123.1161, -123.1147, -123.0846, -123.0870, -123.08…
    ## $ latitude           <dbl> 49.21776, 49.21776, 49.23938, 49.23469, 49.23894, 4…

Modifying the time data to only hold the year the tree was planted might
allow for interesting research questions such as investigating which
year had the most trees planted or looking to see if there are specific
years where certain types of trees were preferentially planted over
others, etc. Further, it may be interesting to see if there is a
relationship between the year the tree was planted and its diameter (i.e
do older trees have larger diameters? Is the diameter of a tree
influenced by how old it is?).

## 4 Task 2: Modelling

### 4.1 Research question and variable of interest

-   Research Question: Within each genus, what is the distribution of
    the diameter of the trees for each species? Which species have a
    wider diameter distribution and which ones have a more narrow
    distribution?

-   Variable of Interest: diameter

### 4.2 Hypothesis testing

To complete Task 2 for milestone 3, I have decided to run a hypothesis
test on the diameter variable. I have chosen to run a one-way anova as I
will be looking at the diameter across multiple tree genus’ (multiple
groups). By running this anova, I will be able to see whether there is a
difference in the mean diameter across the multiple tree groups which is
a good start to answering my original research question; however,
further post hoc tests would need to be run in order to determine which
groups are different from one another.

In order to perform this hypothesis test, I will use the **aov()**
function, which takes in the two variables of interest and performs the
one-way anova analysis.

``` r
## use the aov function (one-way_anova) to test if the mean diameter across the genus groups are different
## store the results somewhere new
aov_van_trees <- aov(diameter~genus_name, data = vancouver_trees)
## visualize the outcome
summary(aov_van_trees)
```

    ##                 Df  Sum Sq Mean Sq F value Pr(>F)    
    ## genus_name      96 2758757   28737     435 <2e-16 ***
    ## Residuals   146514 9679647      66                   
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

#### 4.2.1 P-value

After performing the one-way anova on my data, I will now produce the P
value from the analysis which will tell me whether my results are
significant or not (i.e. whether there is a difference in the mean
diameter across the different tree genus groups or not).

To accomplish this task, I will try using the **tidy()** function from
the broom package.

``` r
## load the broom package
library(broom)
## use the tidy function to clean up the output from the one-way anova results into a tabular format 
tidy_aov_van_trees <- tidy(aov_van_trees)
print(tidy_aov_van_trees)
```

    ## # A tibble: 2 × 6
    ##   term           df    sumsq  meansq statistic p.value
    ##   <chr>       <dbl>    <dbl>   <dbl>     <dbl>   <dbl>
    ## 1 genus_name     96 2758757. 28737.       435.       0
    ## 2 Residuals  146514 9679647.    66.1       NA       NA

``` r
## use the summary to show an alternative way of accessing the P value
summary(aov_van_trees)
```

    ##                 Df  Sum Sq Mean Sq F value Pr(>F)    
    ## genus_name      96 2758757   28737     435 <2e-16 ***
    ## Residuals   146514 9679647      66                   
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

**For the tidy function approach, the P value is located under the
p.value column, and for the summary function approach, the P value is
located under the Pr(>F) column in the genus row.**

Although both options show the P value (tidy produces a P value of 0;
summary produces a P value of \<2e-16), I would prefer to use the
summary function to obtain the P value as not only does it give the
exact calculated number, it also produces three stars (\*\*\*) next to
the P value to denote how significant the result is.

**Final Results** - based on the P value obtained from the one-way
anova, I can conclude that there is a difference in the mean diameter
across the different tree genus groups.

## 5 Task 3: Reading and writing data

For the final task in milestone 3, we are asked to write a csv and RDS
file which will both be saved in the **output** folder within my
repository.

### 5.1 Write a csv file

For the csv file writing part of the task, I will take a summary table I
wrote in milestone 2 and use the **write_csv()** and **here::here()**
function to write the table as a csv file into my output folder.

The summary table I have chosen to use for this task holds data from the
*vancouver_trees* dataset that has been filtered to only the **ULMUS**
genus and then further counted for the amount of trees each species
within the **ULMUS** genus has. When I made this table in milestone 2, I
did not save it as a new dataset. Therefore, I will re-run that code
here and call that new dataset *ulmusGenus_numSpecies_trees*.

``` r
## call the dataset and give a name to the new dataset where the subsetted data will be held
ulmusGenus_numSpecies_trees <- vancouver_trees %>%
## filter the data so it only contains trees with the genus ULMUS 
  filter(genus_name == "ULMUS") %>%
## use the count function and input the variable you want the distinct values for
  count(species_name)
## look at the new dataset
summary(ulmusGenus_numSpecies_trees)
```

    ##  species_name             n         
    ##  Length:7           Min.   :  17.0  
    ##  Class :character   1st Qu.:  21.0  
    ##  Mode  :character   Median :  74.0  
    ##                     Mean   : 408.7  
    ##                     3rd Qu.: 238.0  
    ##                     Max.   :2252.0

Now I can use this summary table and write it into a csv file to be
stored in the output folder.

``` r
## use write_csv to write the summary table into a csv file and use the here::here function to
## put it in the output folder 
## specify the data set to be written into a csv - ulmusGenus_numSpecies_trees
## name the new csv file - exported_ulmusGenus_numSpecies_trees.csv
write_csv(ulmusGenus_numSpecies_trees, here::here("output", "exported_ulmusGenus_numSpecies_trees.csv"))
```

### 5.2 Write a R binary file

For the RDS file writing part of the task, I will take the anova
hypothesis test data I produced in task 2 of this milestone and use the
**saveRDS()** and **here::here()** function to write it as an RDS file
into my output folder. Following that, I will use the **readRDS()**
function to read the file back into r.

``` r
## create an RDS file and put it in the output folder using the saveRDS function
saveRDS(aov_van_trees, here::here("output", "aov_van_trees.RDS"))

## read in the created RDS file to r using the readRDS function
readRDS(here::here("output", "aov_van_trees.RDS"))
```

    ## Call:
    ##    aov(formula = diameter ~ genus_name, data = vancouver_trees)
    ## 
    ## Terms:
    ##                 genus_name Residuals
    ## Sum of Squares     2758757   9679647
    ## Deg. of Freedom         96    146514
    ## 
    ## Residual standard error: 8.128122
    ## Estimated effects may be unbalanced
