---
title: "Lab 13 Homework"
author: "John Abram Flores"
date: "2022-02-28"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above. For any included plots, make sure they are clearly labeled. You are free to use any plot type that you feel best communicates the results of your analysis.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Libraries

```r
if (!require("tidyverse")) install.packages('tidyverse')
```

```
## Loading required package: tidyverse
```

```
## -- Attaching packages --------------------------------------- tidyverse 1.3.1 --
```

```
## v ggplot2 3.3.5     v purrr   0.3.4
## v tibble  3.1.6     v dplyr   1.0.8
## v tidyr   1.2.0     v stringr 1.4.0
## v readr   2.1.2     v forcats 0.5.1
```

```
## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```


```r
library(tidyverse)
library(shiny)
library(shinydashboard)
library(janitor)
```

## Data
The data for this assignment come from the [University of California Information Center](https://www.universityofcalifornia.edu/infocenter). Admissions data were collected for the years 2010-2019 for each UC campus. Admissions are broken down into three categories: applications, admits, and enrollees. The number of individuals in each category are presented by demographic.  

```r
UC_admit <- readr::read_csv("data/UC_admit.csv") %>% clean_names()
```

```
## Rows: 2160 Columns: 6
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr (4): Campus, Category, Ethnicity, Perc FR
## dbl (2): Academic_Yr, FilteredCountFR
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

**1. Use the function(s) of your choice to get an idea of the overall structure of the data frame, including its dimensions, column names, variable classes, etc. As part of this, determine if there are NA's and how they are treated.**  


```r
glimpse(UC_admit) #2160 rows, 6 columns, 2160x6
```

```
## Rows: 2,160
## Columns: 6
## $ campus            <chr> "Davis", "Davis", "Davis", "Davis", "Davis", "Davis"~
## $ academic_yr       <dbl> 2019, 2019, 2019, 2019, 2019, 2019, 2019, 2019, 2018~
## $ category          <chr> "Applicants", "Applicants", "Applicants", "Applicant~
## $ ethnicity         <chr> "International", "Unknown", "White", "Asian", "Chica~
## $ perc_fr           <chr> "21.16%", "2.51%", "18.39%", "30.76%", "22.44%", "0.~
## $ filtered_count_fr <dbl> 16522, 1959, 14360, 24024, 17526, 277, 3425, 78093, ~
```

```r
naniar::miss_var_summary(UC_admit)
```

```
## # A tibble: 6 x 3
##   variable          n_miss pct_miss
##   <chr>              <int>    <dbl>
## 1 perc_fr                1   0.0463
## 2 filtered_count_fr      1   0.0463
## 3 campus                 0   0     
## 4 academic_yr            0   0     
## 5 category               0   0     
## 6 ethnicity              0   0
```

```r
UC_admit$perc_fr <- as.numeric(sub("%", "", UC_admit$perc_fr,fixed=TRUE))/100
```


```r
UC_admit %>%
  count(academic_yr)
```

```
## # A tibble: 10 x 2
##    academic_yr     n
##          <dbl> <int>
##  1        2010   216
##  2        2011   216
##  3        2012   216
##  4        2013   216
##  5        2014   216
##  6        2015   216
##  7        2016   216
##  8        2017   216
##  9        2018   216
## 10        2019   216
```

**2. The president of UC has asked you to build a shiny app that shows admissions by ethnicity across all UC campuses. Your app should allow users to explore year, campus, and admit category as interactive variables. Use shiny dashboard and try to incorporate the aesthetics you have learned in ggplot to make the app neat and clean.**


```r
library(shiny)

ui <- dashboardPage(skin="yellow",
  dashboardHeader(),
  dashboardSidebar(disable=T),
  dashboardBody(box(width=3,
    selectInput("campus", "Select UC Campus", choices=c("Davis", "Berkeley", "Santa_Barbara", "San_Diego", "Merced", "Irvine", "Los_Angeles", "Riverside", "Santa_Cruz"), selected="Davis"),
    
    sliderInput("academic_yr", "Select Year of Admissions", min=2010, max=2019, value=2019, step=1),
    
    radioButtons("category", "Select Category of Applicants", choices=c("Enrollees", "Applicants", "Admits"))
    ),
    plotOutput("plot", width="600px", height="500px")
    
    )
)

server <- function(input, output, session) {
  output$plot <- renderPlot({
    UC_admit %>%
      filter(!ethnicity=="All") %>%
      filter(campus==input$campus & academic_yr==input$academic_yr & category==input$category) %>%
      ggplot(aes_string(x="ethnicity", y="perc_fr", fill="ethnicity")) +geom_col() +ylim(0,1) +theme_linedraw() +labs(x="Ethnicity", y="Percent", title="Ethnicity of students by campus, category of student, and year") +coord_flip()
    
      })
  session$onSessionEnded(stopApp)
}

shinyApp(ui, server)
```

`<div style="width: 100% ; height: 400px ; text-align: center; box-sizing: border-box; -moz-box-sizing: border-box; -webkit-box-sizing: border-box;" class="muted well">Shiny applications not supported in static R Markdown documents</div>`{=html}

**3. Make alternate version of your app above by tracking enrollment at a campus over all of the represented years while allowing users to interact with campus, category, and ethnicity.**


```r
library(shiny)

ui <- dashboardPage(skin="yellow",
  dashboardHeader(),
  dashboardSidebar(disable=T),
  dashboardBody(box(width=3,
    selectInput("campus", "Select UC Campus", choices=c("Davis", "Berkeley", "Santa_Barbara", "San_Diego", "Merced", "Irvine", "Los_Angeles", "Riverside", "Santa_Cruz"), selected="Davis"),
    
    selectInput("ethnicity", "Select Ethnicity", choices=c("African American", "American Indian", "Asian", "Chicano/Latino", "International", "Unknown", "White"), selected="American Indian"),
    
    radioButtons("category", "Select Category of Applicants", choices=c("Enrollees", "Applicants", "Admits"))
    ),
    plotOutput("plot", width="600px", height="500px")
    
    )
)

server <- function(input, output, session) {
  output$plot <- renderPlot({
    UC_admit %>%
      filter(ethnicity==input$ethnicity) %>%
      filter(campus==input$campus & category==input$category) %>%
      ggplot(aes_string(x="academic_yr", y="perc_fr")) +geom_col(fill="blue", alpha=0.4, color="black") +ylim(0,1) +labs(x="Year", y="Percent", title="Admissions Stats for each year (2010-2019) by Ethnicity, Category of Applicants, and Campus") +coord_flip() +scale_x_continuous(limits=c(2010, 2019), breaks = c(2010, 2011, 2012, 2013, 2014, 2015, 2016, 2017, 2018, 2019)) +theme_linedraw()
    
      })
  session$onSessionEnded(stopApp)
}

shinyApp(ui, server)
```

`<div style="width: 100% ; height: 400px ; text-align: center; box-sizing: border-box; -moz-box-sizing: border-box; -webkit-box-sizing: border-box;" class="muted well">Shiny applications not supported in static R Markdown documents</div>`{=html}

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences. 
