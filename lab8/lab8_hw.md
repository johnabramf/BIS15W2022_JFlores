---
title: "Lab 8 Homework"
author: "John Abram Flores"
date: "2022-02-03"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
```

## Install `here`
The package `here` is a nice option for keeping directories clear when loading files. I will demonstrate below and let you decide if it is something you want to use.  

```r
#install.packages("here")
```

## Data
For this homework, we will use a data set compiled by the Office of Environment and Heritage in New South Whales, Australia. It contains the enterococci counts in water samples obtained from Sydney beaches as part of the Beachwatch Water Quality Program. Enterococci are bacteria common in the intestines of mammals; they are rarely present in clean water. So, enterococci values are a measurement of pollution. `cfu` stands for colony forming units and measures the number of viable bacteria in a sample [cfu](https://en.wikipedia.org/wiki/Colony-forming_unit).   

This homework loosely follows the tutorial of [R Ladies Sydney](https://rladiessydney.org/). If you get stuck, check it out!  

1. Start by loading the data `sydneybeaches`. Do some exploratory analysis to get an idea of the data structure.

```r
sydneybeaches <- readr::read_csv("data/sydneybeaches.csv")
```

```
## Rows: 3690 Columns: 8
```

```
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr (4): Region, Council, Site, Date
## dbl (4): BeachId, Longitude, Latitude, Enterococci (cfu/100ml)
```

```
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
sydneybeaches
```

```
## # A tibble: 3,690 x 8
##    BeachId Region    Council   Site   Longitude Latitude Date  `Enterococci (cf~
##      <dbl> <chr>     <chr>     <chr>      <dbl>    <dbl> <chr>             <dbl>
##  1      25 Sydney C~ Randwick~ Clove~      151.    -33.9 02/0~                19
##  2      25 Sydney C~ Randwick~ Clove~      151.    -33.9 06/0~                 3
##  3      25 Sydney C~ Randwick~ Clove~      151.    -33.9 12/0~                 2
##  4      25 Sydney C~ Randwick~ Clove~      151.    -33.9 18/0~                13
##  5      25 Sydney C~ Randwick~ Clove~      151.    -33.9 30/0~                 8
##  6      25 Sydney C~ Randwick~ Clove~      151.    -33.9 05/0~                 7
##  7      25 Sydney C~ Randwick~ Clove~      151.    -33.9 11/0~                11
##  8      25 Sydney C~ Randwick~ Clove~      151.    -33.9 23/0~                97
##  9      25 Sydney C~ Randwick~ Clove~      151.    -33.9 07/0~                 3
## 10      25 Sydney C~ Randwick~ Clove~      151.    -33.9 25/0~                 0
## # ... with 3,680 more rows
```

If you want to try `here`, first notice the output when you load the `here` library. It gives you information on the current working directory. You can then use it to easily and intuitively load files.

```r
library(here)
```

```
## here() starts at C:/Users/Default.DESKTOP-KQBUCHH/Desktop/BIS15W2022_JFlores
```

The quotes show the folder structure from the root directory.

```r
sydneybeaches <-read_csv(here("lab8", "data", "sydneybeaches.csv")) %>% janitor::clean_names()
```

```
## Rows: 3690 Columns: 8
```

```
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr (4): Region, Council, Site, Date
## dbl (4): BeachId, Longitude, Latitude, Enterococci (cfu/100ml)
```

```
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

2. Are these data "tidy" per the definitions of the tidyverse? How do you know? Are they in wide or long format?

**It is not tidy, as by the definition from tidyverse, we see that each variable does not have its own column, as although date, council, etc. fit, the site of where the data is taken is in one column. However, each observation does indeed have its own row, and each value does have its own cell.**

3. We are only interested in the variables site, date, and enterococci_cfu_100ml. Make a new object focused on these variables only. Name the object `sydneybeaches_long`


```r
sydneybeaches_long <- sydneybeaches %>%
  select(site, date, enterococci_cfu_100ml)
sydneybeaches_long
```

```
## # A tibble: 3,690 x 3
##    site           date       enterococci_cfu_100ml
##    <chr>          <chr>                      <dbl>
##  1 Clovelly Beach 02/01/2013                    19
##  2 Clovelly Beach 06/01/2013                     3
##  3 Clovelly Beach 12/01/2013                     2
##  4 Clovelly Beach 18/01/2013                    13
##  5 Clovelly Beach 30/01/2013                     8
##  6 Clovelly Beach 05/02/2013                     7
##  7 Clovelly Beach 11/02/2013                    11
##  8 Clovelly Beach 23/02/2013                    97
##  9 Clovelly Beach 07/03/2013                     3
## 10 Clovelly Beach 25/03/2013                     0
## # ... with 3,680 more rows
```


4. Pivot the data such that the dates are column names and each beach only appears once. Name the object `sydneybeaches_wide`


```r
sydneybeaches_wide <- sydneybeaches_long %>%
  pivot_wider(names_from="site", values_from = "enterococci_cfu_100ml")
sydneybeaches_wide
```

```
## # A tibble: 344 x 12
##    date       `Clovelly Beach` `Coogee Beach` `Gordons Bay (Ea~ `Little Bay Bea~
##    <chr>                 <dbl>          <dbl>             <dbl>            <dbl>
##  1 02/01/2013               19             15                NA                9
##  2 06/01/2013                3              4                NA                3
##  3 12/01/2013                2             17                NA               72
##  4 18/01/2013               13             18                NA                1
##  5 30/01/2013                8             22                NA               44
##  6 05/02/2013                7              2                NA                7
##  7 11/02/2013               11            110                NA              150
##  8 23/02/2013               97            630                NA              330
##  9 07/03/2013                3             11                NA               31
## 10 25/03/2013                0             82                 4              420
## # ... with 334 more rows, and 7 more variables: Malabar Beach <dbl>,
## #   Maroubra Beach <dbl>, South Maroubra Beach <dbl>,
## #   South Maroubra Rockpool <dbl>, Bondi Beach <dbl>, Bronte Beach <dbl>,
## #   Tamarama Beach <dbl>
```


5. Pivot the data back so that the dates are data and not column names.


```r
sydneybeaches_back <- sydneybeaches_wide %>%
  pivot_longer(-date, names_to="beach", values_to="enterococci_cfu_100ml")
sydneybeaches_back
```

```
## # A tibble: 3,784 x 3
##    date       beach                   enterococci_cfu_100ml
##    <chr>      <chr>                                   <dbl>
##  1 02/01/2013 Clovelly Beach                             19
##  2 02/01/2013 Coogee Beach                               15
##  3 02/01/2013 Gordons Bay (East)                         NA
##  4 02/01/2013 Little Bay Beach                            9
##  5 02/01/2013 Malabar Beach                               2
##  6 02/01/2013 Maroubra Beach                              1
##  7 02/01/2013 South Maroubra Beach                        1
##  8 02/01/2013 South Maroubra Rockpool                    12
##  9 02/01/2013 Bondi Beach                                 3
## 10 02/01/2013 Bronte Beach                                4
## # ... with 3,774 more rows
```


6. We haven't dealt much with dates yet, but separate the date into columns day, month, and year. Do this on the `sydneybeaches_long` data.


```r
sydneybeaches_long_mmddyyyy <- sydneybeaches_long %>%
  separate(date, into=c("month", "day", "year"), sep="/")
sydneybeaches_long_mmddyyyy
```

```
## # A tibble: 3,690 x 5
##    site           month day   year  enterococci_cfu_100ml
##    <chr>          <chr> <chr> <chr>                 <dbl>
##  1 Clovelly Beach 02    01    2013                     19
##  2 Clovelly Beach 06    01    2013                      3
##  3 Clovelly Beach 12    01    2013                      2
##  4 Clovelly Beach 18    01    2013                     13
##  5 Clovelly Beach 30    01    2013                      8
##  6 Clovelly Beach 05    02    2013                      7
##  7 Clovelly Beach 11    02    2013                     11
##  8 Clovelly Beach 23    02    2013                     97
##  9 Clovelly Beach 07    03    2013                      3
## 10 Clovelly Beach 25    03    2013                      0
## # ... with 3,680 more rows
```


7. What is the average `enterococci_cfu_100ml` by year for each beach. Think about which data you will use- long or wide.

```r
sydneybeaches_long_mean <- sydneybeaches_long_mmddyyyy %>%
  group_by(site, year) %>%
  summarize(entercocci_mean=mean(enterococci_cfu_100ml, na.rm=TRUE)) %>%
  arrange(desc(entercocci_mean))
```

```
## `summarise()` has grouped output by 'site'. You can override using the `.groups` argument.
```

```r
sydneybeaches_long_mean
```

```
## # A tibble: 66 x 3
## # Groups:   site [11]
##    site                    year  entercocci_mean
##    <chr>                   <chr>           <dbl>
##  1 Little Bay Beach        2013            122. 
##  2 South Maroubra Rockpool 2018            112. 
##  3 Malabar Beach           2013            101. 
##  4 South Maroubra Rockpool 2013             96.4
##  5 Malabar Beach           2016             91.0
##  6 Malabar Beach           2015             66.9
##  7 Bronte Beach            2016             61.3
##  8 Coogee Beach            2016             59.5
##  9 South Maroubra Rockpool 2016             59.3
## 10 Little Bay Beach        2018             59.1
## # ... with 56 more rows
```


8. Make the output from question 7 easier to read by pivoting it to wide format.


```r
sydneybeaches_long_mean %>%
  pivot_wider(names_from = "site", values_from = "entercocci_mean")
```

```
## # A tibble: 6 x 12
##   year  `Little Bay Beach` `South Maroubra Rockp~ `Malabar Beach` `Bronte Beach`
##   <chr>              <dbl>                  <dbl>           <dbl>          <dbl>
## 1 2013               122.                    96.4           101.            26.8
## 2 2018                59.1                  112.             38.0           43.4
## 3 2016                31.2                   59.3            91.0           61.3
## 4 2015                25.5                   47.3            66.9           23.6
## 5 2014                19.5                   40.6            54.5           17.5
## 6 2017                18.2                   46.9            49.8           16.8
## # ... with 7 more variables: Coogee Beach <dbl>, Tamarama Beach <dbl>,
## #   Maroubra Beach <dbl>, South Maroubra Beach <dbl>, Gordons Bay (East) <dbl>,
## #   Bondi Beach <dbl>, Clovelly Beach <dbl>
```


9. What was the most polluted beach in 2018?

**South Maroubra Rockpool on average had the most entercocci per 100mL in the year 2018.**

```r
sydneybeaches_long_mmddyyyy %>%
  filter(year=="2018") %>%
  group_by(site) %>%
  summarize(twentyeighteen_entercocci_mean=mean(enterococci_cfu_100ml, na.rm=TRUE)) %>%
  arrange(desc(twentyeighteen_entercocci_mean))
```

```
## # A tibble: 11 x 2
##    site                    twentyeighteen_entercocci_mean
##    <chr>                                            <dbl>
##  1 South Maroubra Rockpool                         112.  
##  2 Little Bay Beach                                 59.1 
##  3 Bronte Beach                                     43.4 
##  4 Malabar Beach                                    38.0 
##  5 Bondi Beach                                      22.9 
##  6 Coogee Beach                                     21.6 
##  7 Gordons Bay (East)                               17.6 
##  8 Tamarama Beach                                   15.5 
##  9 South Maroubra Beach                             12.5 
## 10 Clovelly Beach                                   10.6 
## 11 Maroubra Beach                                    9.21
```


10. Please complete the class project survey at: [BIS 15L Group Project](https://forms.gle/H2j69Z3ZtbLH3efW6)


## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
