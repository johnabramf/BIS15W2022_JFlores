---
title: "Lab 11 Homework"
author: "John Abram Flores"
date: "2022-02-15"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above. For any included plots, make sure they are clearly labeled. You are free to use any plot type that you feel best communicates the results of your analysis.  

**In this homework, you should make use of the aesthetics you have learned. It's OK to be flashy!**

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
library(here)
library(naniar)
library(skimr)
library(ggthemes)
```


```r
options(scipen=999)
```

## Resources
The idea for this assignment came from [Rebecca Barter's](http://www.rebeccabarter.com/blog/2017-11-17-ggplot2_tutorial/) ggplot tutorial so if you get stuck this is a good place to have a look.  

## Gapminder
For this assignment, we are going to use the dataset [gapminder](https://cran.r-project.org/web/packages/gapminder/index.html). Gapminder includes information about economics, population, and life expectancy from countries all over the world. You will need to install it before use. This is the same data that we will use for midterm 2 so this is good practice.

```r
#install.packages("gapminder")
library("gapminder")
```

## Questions
The questions below are open-ended and have many possible solutions. Your approach should, where appropriate, include numerical summaries and visuals. Be creative; assume you are building an analysis that you would ultimately present to an audience of stakeholders. Feel free to try out different `geoms` if they more clearly present your results.  

**1. Use the function(s) of your choice to get an idea of the overall structure of the data frame, including its dimensions, column names, variable classes, etc. As part of this, determine how NA's are treated in the data.**  


```r
miss_var_summary(gapminder) #no NA"s found, probably as -999.0
```

```
## # A tibble: 6 x 3
##   variable  n_miss pct_miss
##   <chr>      <int>    <dbl>
## 1 country        0        0
## 2 continent      0        0
## 3 year           0        0
## 4 lifeExp        0        0
## 5 pop            0        0
## 6 gdpPercap      0        0
```


```r
skimr::skim(gapminder) # No outlandish -999.0 values, no NA's present
```


Table: Data summary

|                         |          |
|:------------------------|:---------|
|Name                     |gapminder |
|Number of rows           |1704      |
|Number of columns        |6         |
|_______________________  |          |
|Column type frequency:   |          |
|factor                   |2         |
|numeric                  |4         |
|________________________ |          |
|Group variables          |None      |


**Variable type: factor**

|skim_variable | n_missing| complete_rate|ordered | n_unique|top_counts                             |
|:-------------|---------:|-------------:|:-------|--------:|:--------------------------------------|
|country       |         0|             1|FALSE   |      142|Afg: 12, Alb: 12, Alg: 12, Ang: 12     |
|continent     |         0|             1|FALSE   |        5|Afr: 624, Asi: 396, Eur: 360, Ame: 300 |


**Variable type: numeric**

|skim_variable | n_missing| complete_rate|        mean|           sd|       p0|        p25|        p50|         p75|         p100|hist                                     |
|:-------------|---------:|-------------:|-----------:|------------:|--------:|----------:|----------:|-----------:|------------:|:----------------------------------------|
|year          |         0|             1|     1979.50|        17.27|  1952.00|    1965.75|    1979.50|     1993.25|       2007.0|▇▅▅▅▇ |
|lifeExp       |         0|             1|       59.47|        12.92|    23.60|      48.20|      60.71|       70.85|         82.6|▁▆▇▇▇ |
|pop           |         0|             1| 29601212.32| 106157896.74| 60011.00| 2793664.00| 7023595.50| 19585221.75| 1318683096.0|▇▁▁▁▁ |
|gdpPercap     |         0|             1|     7215.33|      9857.45|   241.17|    1202.06|    3531.85|     9325.46|     113523.1|▇▁▁▁▁ |

```r
glimpse(gapminder)
```

```
## Rows: 1,704
## Columns: 6
## $ country   <fct> "Afghanistan", "Afghanistan", "Afghanistan", "Afghanistan", ~
## $ continent <fct> Asia, Asia, Asia, Asia, Asia, Asia, Asia, Asia, Asia, Asia, ~
## $ year      <int> 1952, 1957, 1962, 1967, 1972, 1977, 1982, 1987, 1992, 1997, ~
## $ lifeExp   <dbl> 28.801, 30.332, 31.997, 34.020, 36.088, 38.438, 39.854, 40.8~
## $ pop       <int> 8425333, 9240934, 10267083, 11537966, 13079460, 14880372, 12~
## $ gdpPercap <dbl> 779.4453, 820.8530, 853.1007, 836.1971, 739.9811, 786.1134, ~
```

**2. Among the interesting variables in gapminder is life expectancy. How has global life expectancy changed between 1952 and 2007?**

*We see a general increase in life expectancies as time increased between 1952 and 2007.*

```r
gapminder %>%
  select(year,lifeExp) %>%
  ggplot(aes(x=year, y=lifeExp)) +geom_jitter() +geom_smooth(method=lm, se=T) +labs(title="Global Life Expectancies from 1952-2007")
```

```
## `geom_smooth()` using formula 'y ~ x'
```

![](lab11_hw_files/figure-html/unnamed-chunk-6-1.png)<!-- -->

**3. How do the distributions of life expectancy compare for the years 1952 and 2007?**


```r
gapminder %>%
  filter(year==1952) %>%
  ggplot(aes(x=lifeExp)) +theme_economist() +geom_histogram(bins=20, color = "black", fill = "khaki") + labs(title="Histogram of Life Expectancies in 1952", x="Life Expectancy (years)")
```

![](lab11_hw_files/figure-html/unnamed-chunk-7-1.png)<!-- -->


```r
gapminder %>%
  filter(year==2007) %>%
  ggplot(aes(x=lifeExp)) +theme_economist() +geom_histogram(bins=20, color = "black", fill = "khaki") + labs(title="Histogram of Life Expectancies in 2007", x="Life Expectancy (years)")
```

![](lab11_hw_files/figure-html/unnamed-chunk-8-1.png)<!-- -->

**4. Your answer above doesn't tell the whole story since life expectancy varies by region. Make a summary that shows the min, mean, and max life expectancy by continent for all years represented in the data.**


```r
gapminder %>%
  group_by(continent) %>%
  summarize(min=min(lifeExp), max=max(lifeExp), mean_exp=mean(lifeExp))
```

```
## # A tibble: 5 x 4
##   continent   min   max mean_exp
##   <fct>     <dbl> <dbl>    <dbl>
## 1 Africa     23.6  76.4     48.9
## 2 Americas   37.6  80.7     64.7
## 3 Asia       28.8  82.6     60.1
## 4 Europe     43.6  81.8     71.9
## 5 Oceania    69.1  81.2     74.3
```

**5. How has life expectancy changed between 1952-2007 for each continent?**


```r
gapminder %>%
  group_by(year, continent) %>%
  summarize(mean=mean(lifeExp)) %>%
  ggplot(aes(x=year, y=mean, color=continent)) +geom_point() +geom_line() +labs(y="Average Lifespan in Years", x="Year", title="Mean expected lifespans across Continents from 1952-2007")+theme(plot.title=element_text(size=rel(1.2)), axis.text=element_text(angle=90))+facet_wrap(~continent)
```

```
## `summarise()` has grouped output by 'year'. You can override using the `.groups` argument.
```

![](lab11_hw_files/figure-html/unnamed-chunk-10-1.png)<!-- -->

**6. We are interested in the relationship between per capita GDP and life expectancy; i.e. does having more money help you live longer?**


```r
gapminder %>%
  group_by(continent) %>%
  summarize(meanGDP=mean(gdpPercap), meanExp=mean(lifeExp)) %>%
  ggplot(aes(x=meanExp, y=meanGDP, color=continent)) +geom_point(size=3) +theme_economist() +labs(title="Life expectancy v. Mean GDP across continents (1952-2007)", x="Average life expectancy", y="Average GDP per capita") +theme(plot.title=element_text(size=rel(1.15)))
```

![](lab11_hw_files/figure-html/unnamed-chunk-11-1.png)<!-- -->


**7. Which countries have had the largest population growth since 1952?**


```r
gapminder %>%
  select(year, country, pop) %>%
  filter(year==1952 | year==2007) %>%
  group_by(country) %>%
  mutate(pop_growth=max(pop)-min(pop)) %>%
  arrange(desc(pop_growth)) %>%
  head(n=10)
```

```
## # A tibble: 10 x 4
## # Groups:   country [5]
##     year country              pop pop_growth
##    <int> <fct>              <int>      <int>
##  1  1952 China          556263527  762419569
##  2  2007 China         1318683096  762419569
##  3  1952 India          372000000  738396331
##  4  2007 India         1110396331  738396331
##  5  1952 United States  157553000  143586947
##  6  2007 United States  301139947  143586947
##  7  1952 Indonesia       82052000  141495000
##  8  2007 Indonesia      223547000  141495000
##  9  1952 Brazil          56602560  133408087
## 10  2007 Brazil         190010647  133408087
```

**8. Use your results from the question above to plot population growth for the top five countries since 1952.**


```r
gapminder %>%
  select(year, country, pop) %>%
  filter(country=="China" | country=="Brazil" | country=="Indonesia" | country=="India" | country=="United States") %>%
  ggplot(aes(x=year, y=pop, color=country)) +geom_point(size=3) +theme_economist() +labs(title="Top 5 Countries in Population Growth") +theme(plot.title=element_text(size=rel(1.2)))
```

![](lab11_hw_files/figure-html/unnamed-chunk-13-1.png)<!-- -->


**9. How does per-capita GDP growth compare between these same five countries?**

```r
gapminder %>%
  select(year, country, gdpPercap) %>%
  filter(country=="China" | country=="Brazil" | country=="Indonesia" | country=="India" | country=="United States") %>%
  ggplot(aes(x=year, y=gdpPercap, color=country))  +geom_point(size=3) +theme_economist() +labs(title="Top 5 Countries in Population Growth v. GDP per capita") +theme(plot.title=element_text(size=rel(1.2)))
```

![](lab11_hw_files/figure-html/unnamed-chunk-14-1.png)<!-- -->

**10. Make one plot of your choice that uses faceting!**


```r
gapminder %>%
  group_by(continent, year) %>%
  summarize(meanGDP=mean(gdpPercap)) %>%
  ggplot(aes(x=year, y=meanGDP, fill=continent)) +geom_point() +facet_wrap(~continent) +labs(title="GDP Per Capita by continent 1952-2007") +theme(axis.text=element_text(angle=90, size=7))
```

```
## `summarise()` has grouped output by 'continent'. You can override using the `.groups` argument.
```

![](lab11_hw_files/figure-html/unnamed-chunk-15-1.png)<!-- -->

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences. 
