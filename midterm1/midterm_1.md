---
title: "Midterm 1"
author: "John Abram Flores"
date: "2022-01-26"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your code should be organized, clean, and run free from errors. Remember, you must remove the `#` for any included code chunks to run. Be sure to add your name to the author header above. You may use any resources to answer these questions (including each other), but you may not post questions to Open Stacks or external help sites. There are 15 total questions, each is worth 2 points.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

This exam is due by 12:00p on Thursday, January 27.  

## Load the tidyverse
If you plan to use any other libraries to complete this assignment then you should load them here.

```r
library("tidyverse")
```

```
## -- Attaching packages --------------------------------------- tidyverse 1.3.1 --
```

```
## v ggplot2 3.3.5     v purrr   0.3.4
## v tibble  3.1.6     v dplyr   1.0.7
## v tidyr   1.1.4     v stringr 1.4.0
## v readr   2.1.1     v forcats 0.5.1
```

```
## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
library("dplyr")
library("janitor")
```

```
## 
## Attaching package: 'janitor'
```

```
## The following objects are masked from 'package:stats':
## 
##     chisq.test, fisher.test
```

```r
library("remotes")
```

## Questions  
Wikipedia's definition of [data science](https://en.wikipedia.org/wiki/Data_science): "Data science is an interdisciplinary field that uses scientific methods, processes, algorithms and systems to extract knowledge and insights from noisy, structured and unstructured data, and apply knowledge and actionable insights from data across a broad range of application domains."  

1. (2 points) Consider the definition of data science above. Although we are only part-way through the quarter, what specific elements of data science do you feel we have practiced? Provide at least one specific example.  

**Throughout the first 3 weeks of BIS015L, some specific elements of data science that we've practiced are the idea of extract knowledge and insights from noisy, unstructured or structured data, as well as touched a little into the actionable insights (e.g. data analysis in a time span) from data. One specific example that encapsulates applying knowledge and actionable insights would be with Lab 6's homework, as we were asked to do some simple clean up of the UN's Food and Agriculture Organization's fishery data. In Lab 6, we sorted the data to find countries with the most overall catch of a species or in total, within in a set timespan (e.g. 2008-2012).**

2. (2 points) What is the most helpful or interesting thing you have learned so far in BIS 15L? What is something that you think needs more work or practice?  

**The most interesting thing I've learned so far in BIS 015L would be the different libraries we've used so far to arrange, sort, and mutate data, such as janitor, tidyverse, and dplyr. One thing I may need more practice on is the pipe (%>%) function, as I take the most time to code with these functions.**

In the midterm 1 folder there is a second folder called `data`. Inside the `data` folder, there is a .csv file called `ElephantsMF`. These data are from Phyllis Lee, Stirling University, and are related to Lee, P., et al. (2013), "Enduring consequences of early experiences: 40-year effects on survival and success among African elephants (Loxodonta africana)," Biology Letters, 9: 20130011. [kaggle](https://www.kaggle.com/mostafaelseidy/elephantsmf).  

3. (2 points) Please load these data as a new object called `elephants`. Use the function(s) of your choice to get an idea of the structure of the data. Be sure to show the class of each variable.


```r
elephants <- readr::read_csv("data/ElephantsMF.csv")
```

```
## Rows: 288 Columns: 3
```

```
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr (1): Sex
## dbl (2): Age, Height
```

```
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```


**The glimpse function shows us 288 rows (entries), and 3 columns, which are age, height, and sex. The classes are shown with the function as well, as age and height are dbl and sex is character.**

```r
glimpse(elephants)
```

```
## Rows: 288
## Columns: 3
## $ Age    <dbl> 1.40, 17.50, 12.75, 11.17, 12.67, 12.67, 12.25, 12.17, 28.17, 1~
## $ Height <dbl> 120.00, 227.00, 235.00, 210.00, 220.00, 189.00, 225.00, 204.00,~
## $ Sex    <chr> "M", "M", "M", "M", "M", "M", "M", "M", "M", "M", "M", "M", "M"~
```

4. (2 points) Change the names of the variables to lower case and change the class of the variable `sex` to a factor.


```r
elephants_c <- clean_names(elephants)
```


```r
elephants_c$sex <- as.factor(elephants_c$sex)
glimpse(elephants_c)
```

```
## Rows: 288
## Columns: 3
## $ age    <dbl> 1.40, 17.50, 12.75, 11.17, 12.67, 12.67, 12.25, 12.17, 28.17, 1~
## $ height <dbl> 120.00, 227.00, 235.00, 210.00, 220.00, 189.00, 225.00, 204.00,~
## $ sex    <fct> M, M, M, M, M, M, M, M, M, M, M, M, M, M, M, M, M, M, M, M, M, ~
```

5. (2 points) How many male and female elephants are represented in the data?

**By piping into a count(df$var) command, we see that there are 150 female elephants and 138 male elephants represented in the data.**

```r
elephants_c %>%
  group_by(sex) %>% #Group counting data by sex
  count(sex) #Count the entries of data by sex
```

```
## # A tibble: 2 x 2
## # Groups:   sex [2]
##   sex       n
##   <fct> <int>
## 1 F       150
## 2 M       138
```

6. (2 points) What is the average age all elephants in the data?
**Of all the elephants, the average age is around 10.87132 years.**


```r
elephants_c %>%
  summarize(average_age = mean(age))
```

```
## # A tibble: 1 x 1
##   average_age
##         <dbl>
## 1        11.0
```


7. (2 points) How does the average age and height of elephants compare by sex?
**By sex, the average age of a female elephant is 12.8354 years old, and the average age of a male elephant is 8.945145 years  By height, the average (shoulder) height of a female elephant is 190.0307 cm, while the average height of a male elephant is 185.1312 cm.**

```r
elephants_c %>%
  group_by(sex) %>% #Group by sex
  summarize(average_age_by_sex = mean(age), average_height_by_sex=mean(height)) #Calculate the average age and height by sex
```

```
## # A tibble: 2 x 3
##   sex   average_age_by_sex average_height_by_sex
##   <fct>              <dbl>                 <dbl>
## 1 F                  12.8                   190.
## 2 M                   8.95                  185.
```

8. (2 points) How does the average height of elephants compare by sex for individuals over 20 years old. Include the min and max height as well as the number of individuals in the sample as part of your analysis.  
**The average height of a male over 20 years old is 269.5931 cm, while the average height of a female over 20 years old is 232.2014 cm. On average, males over 20 years old are taller than females over 20 years old, compared to all females and males, where females were taller on average. There are a total of 37 females and 13 males over 20 years old. The minimum heights are 192.54 cm for females and 228.69 cm for males, and the maximum heights are 277.80 cm for females and 304.06 cm for males.**

```r
elephants_c %>%
  filter(age>20) %>% #Filter entries to elepahnts over 20 years old
  group_by(sex) %>% #Group by sex
  summarize(average_height_over_20=mean(height), min_height=min(height), max_height=max(height), individuals=n())
```

```
## # A tibble: 2 x 5
##   sex   average_height_over_20 min_height max_height individuals
##   <fct>                  <dbl>      <dbl>      <dbl>       <int>
## 1 F                       232.       193.       278.          37
## 2 M                       270.       229.       304.          13
```

For the next series of questions, we will use data from a study on vertebrate community composition and impacts from defaunation in [Gabon, Africa](https://en.wikipedia.org/wiki/Gabon). One thing to notice is that the data include 24 separate transects. Each transect represents a path through different forest management areas.  

Reference: Koerner SE, Poulsen JR, Blanchard EJ, Okouyi J, Clark CJ. Vertebrate community composition and diversity declines along a defaunation gradient radiating from rural villages in Gabon. _Journal of Applied Ecology_. 2016. This paper, along with a description of the variables is included inside the midterm 1 folder.  

9. (2 points) Load `IvindoData_DryadVersion.csv` and use the function(s) of your choice to get an idea of the overall structure. Change the variables `HuntCat` and `LandUse` to factors.

```r
gabon_defaunation <- readr::read_csv("data/IvindoData_DryadVersion.csv")
```

```
## Rows: 24 Columns: 26
```

```
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr  (2): HuntCat, LandUse
## dbl (24): TransectID, Distance, NumHouseholds, Veg_Rich, Veg_Stems, Veg_lian...
```

```
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```


```r
glimpse(gabon_defaunation)
```

```
## Rows: 24
## Columns: 26
## $ TransectID              <dbl> 1, 2, 2, 3, 4, 5, 6, 7, 8, 9, 13, 14, 15, 16, ~
## $ Distance                <dbl> 7.14, 17.31, 18.32, 20.85, 15.95, 17.47, 24.06~
## $ HuntCat                 <chr> "Moderate", "None", "None", "None", "None", "N~
## $ NumHouseholds           <dbl> 54, 54, 29, 29, 29, 29, 29, 54, 25, 73, 46, 56~
## $ LandUse                 <chr> "Park", "Park", "Park", "Logging", "Park", "Pa~
## $ Veg_Rich                <dbl> 16.67, 15.75, 16.88, 12.44, 17.13, 16.50, 14.7~
## $ Veg_Stems               <dbl> 31.20, 37.44, 32.33, 29.39, 36.00, 29.22, 31.2~
## $ Veg_liana               <dbl> 5.78, 13.25, 4.75, 9.78, 13.25, 12.88, 8.38, 8~
## $ Veg_DBH                 <dbl> 49.57, 34.59, 42.82, 36.62, 41.52, 44.07, 51.2~
## $ Veg_Canopy              <dbl> 3.78, 3.75, 3.43, 3.75, 3.88, 2.50, 4.00, 4.00~
## $ Veg_Understory          <dbl> 2.89, 3.88, 3.00, 2.75, 3.25, 3.00, 2.38, 2.71~
## $ RA_Apes                 <dbl> 1.87, 0.00, 4.49, 12.93, 0.00, 2.48, 3.78, 6.1~
## $ RA_Birds                <dbl> 52.66, 52.17, 37.44, 59.29, 52.62, 38.64, 42.6~
## $ RA_Elephant             <dbl> 0.00, 0.86, 1.33, 0.56, 1.00, 0.00, 1.11, 0.43~
## $ RA_Monkeys              <dbl> 38.59, 28.53, 41.82, 19.85, 41.34, 43.29, 46.2~
## $ RA_Rodent               <dbl> 4.22, 6.04, 1.06, 3.66, 2.52, 1.83, 3.10, 1.26~
## $ RA_Ungulate             <dbl> 2.66, 12.41, 13.86, 3.71, 2.53, 13.75, 3.10, 8~
## $ Rich_AllSpecies         <dbl> 22, 20, 22, 19, 20, 22, 23, 19, 19, 19, 21, 22~
## $ Evenness_AllSpecies     <dbl> 0.793, 0.773, 0.740, 0.681, 0.811, 0.786, 0.81~
## $ Diversity_AllSpecies    <dbl> 2.452, 2.314, 2.288, 2.006, 2.431, 2.429, 2.56~
## $ Rich_BirdSpecies        <dbl> 11, 10, 11, 8, 8, 10, 11, 11, 11, 9, 11, 11, 1~
## $ Evenness_BirdSpecies    <dbl> 0.732, 0.704, 0.688, 0.559, 0.799, 0.771, 0.80~
## $ Diversity_BirdSpecies   <dbl> 1.756, 1.620, 1.649, 1.162, 1.660, 1.775, 1.92~
## $ Rich_MammalSpecies      <dbl> 11, 10, 11, 11, 12, 12, 12, 8, 8, 10, 10, 11, ~
## $ Evenness_MammalSpecies  <dbl> 0.736, 0.705, 0.650, 0.619, 0.736, 0.694, 0.77~
## $ Diversity_MammalSpecies <dbl> 1.764, 1.624, 1.558, 1.484, 1.829, 1.725, 1.92~
```


```r
gabon_defaunation$HuntCat <- as.factor(gabon_defaunation$HuntCat) #Set HuntCat and LandUse as factors
gabon_defaunation$LandUse <- as.factor(gabon_defaunation$LandUse)
```

10. (4 points) For the transects with high and moderate hunting intensity, how does the average diversity of birds and mammals compare?
**For transects that have moderate or high hunting intensities, both average bird and mammal diversity is higher in higher intensity transects than lower intensity transects. For bird diversity, high areas have an average diversity of 1.6609, compared to the moderate areas of 1.6215. For mammal species, high areas have an average density of 1.737, while the moderate areas have an average density 1.68375.**

```r
gabon_defaunation %>%
  filter(HuntCat=="Moderate" | HuntCat=="High") %>% #Filter by moderate OR high hunting intensity
  group_by(HuntCat) %>% #Group by intensity
  summarize(avg_bird_diversity=mean(Diversity_BirdSpecies), avg_mammal_diversity=mean(Diversity_MammalSpecies))
```

```
## # A tibble: 2 x 3
##   HuntCat  avg_bird_diversity avg_mammal_diversity
##   <fct>                 <dbl>                <dbl>
## 1 High                   1.66                 1.74
## 2 Moderate               1.62                 1.68
```

11. (4 points) One of the conclusions in the study is that the relative abundance of animals drops off the closer you get to a village. Let's try to reconstruct this (without the statistics). How does the relative abundance (RA) of apes, birds, elephants, monkeys, rodents, and ungulates compare between sites that are less than 3km from a village to sites that are greater than 25km from a village? The variable `Distance` measures the distance of the transect from the nearest village. Hint: try using the `across` operator.

**After running the two code chunks below, we see that there are two transects less than 3km away from a village, and transect more than 25km away from a village. On average, the transects less than 3km from a village have a higher abundance of birds, elephants, and rodents compared to the transects greater than 25km, while the transects greater than 25km from a village have higher abundance of apes, monkeys, and ungulates.**

```r
gabon_defaunation %>%
  filter(Distance<3) %>% #Filter out transects for less than 3km
  summarize(number_of_transects_3km=n_distinct(TransectID), across(contains("RA"), mean, na.rm=TRUE)) %>% #Calculate averages of all RA's for selected transects
  select(!TransectID) #Remove TransectID Variable
```

```
## # A tibble: 1 x 7
##   number_of_trans~ RA_Apes RA_Birds RA_Elephant RA_Monkeys RA_Rodent RA_Ungulate
##              <dbl>   <dbl>    <dbl>       <dbl>      <dbl>     <dbl>       <dbl>
## 1                2    0.12     76.6       0.145       17.3      3.90        1.87
```


```r
gabon_defaunation %>%
  filter(Distance>25) %>% #Filter out transects for greather than 25km
  summarize(number_of_transects_25km=n_distinct(TransectID), across(contains("RA"), mean, na.rm=TRUE)) %>%
  select(!TransectID) 
```

```
## # A tibble: 1 x 7
##   number_of_trans~ RA_Apes RA_Birds RA_Elephant RA_Monkeys RA_Rodent RA_Ungulate
##              <dbl>   <dbl>    <dbl>       <dbl>      <dbl>     <dbl>       <dbl>
## 1                1    4.91     31.6           0       54.1      1.29        8.12
```

12. (4 points) Based on your interest, do one exploratory analysis on the `gabon` data of your choice. This analysis needs to include a minimum of two functions in `dplyr.`

**In this data, I will compare overall richness, evenness, and diversity of animals by land uses.**

```r
gabon_defaunation$LandUse <- as.factor(gabon_defaunation$LandUse)
is.factor(gabon_defaunation$LandUse) #Double checked that the first line worked (without using glimpse, str, etc.)
```

```
## [1] TRUE
```
**With the modified data frame below, we see 13 transects under logging, 6 for parkuse, and 4 with no designation. For species richness, park use had greater richness than logging, which had greater richness than no use. In species evenness, no use land has the greatest evenness, followed by park use then logging use at last. For overall species diversity, park use had the highest diversity, followed by no use then logging use.**

```r
gabon_defaunation %>%
  group_by(LandUse) %>% #Group by land use
  summarize(across(contains("All"), mean, na.rm=TRUE), n_transects=n_distinct(TransectID))
```

```
## # A tibble: 3 x 5
##   LandUse Rich_AllSpecies Evenness_AllSpecies Diversity_AllSpecies n_transects
##   <fct>             <dbl>               <dbl>                <dbl>       <int>
## 1 Logging            19.6               0.753                 2.23          13
## 2 Neither            19.2               0.797                 2.36           4
## 3 Park               21.9               0.787                 2.43           6
```

