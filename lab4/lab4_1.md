---
title: "Transforming data 1: Dplyr `select()`"
date: "2022-01-13"
output:
  html_document: 
    theme: spacelab
    toc: yes
    toc_float: yes
    keep_md: yes
  pdf_document:
    toc: yes
---

## Learning Goals
*At the end of this exercise, you will be able to:*    
1. Use summary functions to assess the structure of a data frame.  
2. Us the select function of `dplyr` to build data frames restricted to variable of interest.  
3. Use the `rename()` function to provide new, consistent names to variables in data frames.  

## Review
At this point, you should have familiarity in RStudio, GitHub, and basic operations in R. If you need extra help, please [email me](mailto: jmledford@ucdavis.edu).  

## Load the tidyverse
For the remainder of the quarter, we will work within the `tidyverse`. At the start of each lab, the library needs to be called as shown below.  

```r
library("tidyverse")
```

## Load the data
These data are from: Gaeta J., G. Sass, S. Carpenter. 2012. Biocomplexity at North Temperate Lakes LTER: Coordinated Field Studies: Large Mouth Bass Growth 2006. Environmental Data Initiative.  [link](https://portal.edirepository.org/nis/mapbrowse?scope=knb-lter-ntl&identifier=267)  

```r
fish <- readr::read_csv("data/Gaeta_etal_CLC_data.csv")
```

```
## Rows: 4033 Columns: 6
```

```
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (2): lakeid, annnumber
## dbl (4): fish_id, length, radii_length_mm, scalelength
```

```
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

## Data Structure
Once data have been uploaded, let's get an idea of its structure, contents, and dimensions.  

```r
glimpse(fish)
```

```
## Rows: 4,033
## Columns: 6
## $ lakeid          <chr> "AL", "AL", "AL", "AL", "AL", "AL", "AL", "AL", "AL", …
## $ fish_id         <dbl> 299, 299, 299, 300, 300, 300, 300, 301, 301, 301, 301,…
## $ annnumber       <chr> "EDGE", "2", "1", "EDGE", "3", "2", "1", "EDGE", "3", …
## $ length          <dbl> 167, 167, 167, 175, 175, 175, 175, 194, 194, 194, 194,…
## $ radii_length_mm <dbl> 2.697443, 2.037518, 1.311795, 3.015477, 2.670733, 2.13…
## $ scalelength     <dbl> 2.697443, 2.697443, 2.697443, 3.015477, 3.015477, 3.01…
```


```r
str(fish)
```

```
## spec_tbl_df [4,033 × 6] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ lakeid         : chr [1:4033] "AL" "AL" "AL" "AL" ...
##  $ fish_id        : num [1:4033] 299 299 299 300 300 300 300 301 301 301 ...
##  $ annnumber      : chr [1:4033] "EDGE" "2" "1" "EDGE" ...
##  $ length         : num [1:4033] 167 167 167 175 175 175 175 194 194 194 ...
##  $ radii_length_mm: num [1:4033] 2.7 2.04 1.31 3.02 2.67 ...
##  $ scalelength    : num [1:4033] 2.7 2.7 2.7 3.02 3.02 ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   lakeid = col_character(),
##   ..   fish_id = col_double(),
##   ..   annnumber = col_character(),
##   ..   length = col_double(),
##   ..   radii_length_mm = col_double(),
##   ..   scalelength = col_double()
##   .. )
##  - attr(*, "problems")=<externalptr>
```


```r
summary(fish)
```

```
##     lakeid             fish_id       annnumber             length     
##  Length:4033        Min.   :  1.0   Length:4033        Min.   : 58.0  
##  Class :character   1st Qu.:156.0   Class :character   1st Qu.:253.0  
##  Mode  :character   Median :267.0   Mode  :character   Median :299.0  
##                     Mean   :258.3                      Mean   :293.3  
##                     3rd Qu.:376.0                      3rd Qu.:342.0  
##                     Max.   :478.0                      Max.   :420.0  
##  radii_length_mm    scalelength     
##  Min.   : 0.4569   Min.   : 0.6282  
##  1st Qu.: 2.3252   1st Qu.: 4.2596  
##  Median : 3.5380   Median : 5.4062  
##  Mean   : 3.6589   Mean   : 5.3821  
##  3rd Qu.: 4.8229   3rd Qu.: 6.4145  
##  Max.   :11.0258   Max.   :11.0258
```


```r
names(fish)
```

```
## [1] "lakeid"          "fish_id"         "annnumber"       "length"         
## [5] "radii_length_mm" "scalelength"
```

## Tidyverse
The [tidyverse](www.tidyverse.org) is an "opinionated" collection of packages that make workflow in R easier. The packages operate more intuitively than base R commands and share a common organizational philosophy.  
![*Data Science Workflow in the Tidyverse.*](tidy-1.png)  

## dplyr
The first package that we will use that is part of the tidyverse is `dplyr`. `dplyr` is used to transform data frames by extracting, rearranging, and summarizing data such that they are focused on a question of interest. This is very helpful,  especially when wrangling large data, and makes dplyr one of most frequently used packages in the tidyverse. The two functions we will use most are `select()` and `filter()`.  

## `select()`
Select allows you to pull out columns of interest from a dataframe. To do this, just add the names of the columns to the `select()` command. The order in which you add them, will determine the order in which they appear in the output.

```r
names(fish)
```

```
## [1] "lakeid"          "fish_id"         "annnumber"       "length"         
## [5] "radii_length_mm" "scalelength"
```

We are only interested in lakeid and scalelength.

```r
select(fish, "lakeid", "scalelength")
```

```
## # A tibble: 4,033 × 2
##    lakeid scalelength
##    <chr>        <dbl>
##  1 AL            2.70
##  2 AL            2.70
##  3 AL            2.70
##  4 AL            3.02
##  5 AL            3.02
##  6 AL            3.02
##  7 AL            3.02
##  8 AL            3.34
##  9 AL            3.34
## 10 AL            3.34
## # … with 4,023 more rows
```

To add a range of columns use `start_col:end_col`.

```r
fish
```

```
## # A tibble: 4,033 × 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 AL         299 EDGE         167            2.70        2.70
##  2 AL         299 2            167            2.04        2.70
##  3 AL         299 1            167            1.31        2.70
##  4 AL         300 EDGE         175            3.02        3.02
##  5 AL         300 3            175            2.67        3.02
##  6 AL         300 2            175            2.14        3.02
##  7 AL         300 1            175            1.23        3.02
##  8 AL         301 EDGE         194            3.34        3.34
##  9 AL         301 3            194            2.97        3.34
## 10 AL         301 2            194            2.29        3.34
## # … with 4,023 more rows
```


```r
select(fish, fish_id:length)
```

```
## # A tibble: 4,033 × 3
##    fish_id annnumber length
##      <dbl> <chr>      <dbl>
##  1     299 EDGE         167
##  2     299 2            167
##  3     299 1            167
##  4     300 EDGE         175
##  5     300 3            175
##  6     300 2            175
##  7     300 1            175
##  8     301 EDGE         194
##  9     301 3            194
## 10     301 2            194
## # … with 4,023 more rows
```

The - operator is useful in select. It allows us to select everything except the specified variables.

```r
select(fish, -fish_id, -annnumber, -length, -radii_length_mm)
```

```
## # A tibble: 4,033 × 2
##    lakeid scalelength
##    <chr>        <dbl>
##  1 AL            2.70
##  2 AL            2.70
##  3 AL            2.70
##  4 AL            3.02
##  5 AL            3.02
##  6 AL            3.02
##  7 AL            3.02
##  8 AL            3.34
##  9 AL            3.34
## 10 AL            3.34
## # … with 4,023 more rows
```

For very large data frames with lots of variables, `select()` utilizes lots of different operators to make things easier. Let's say we are only interested in the variables that deal with length.


```r
names(fish)
```

```
## [1] "lakeid"          "fish_id"         "annnumber"       "length"         
## [5] "radii_length_mm" "scalelength"
```


```r
select(fish, contains("length"))
```

```
## # A tibble: 4,033 × 3
##    length radii_length_mm scalelength
##     <dbl>           <dbl>       <dbl>
##  1    167            2.70        2.70
##  2    167            2.04        2.70
##  3    167            1.31        2.70
##  4    175            3.02        3.02
##  5    175            2.67        3.02
##  6    175            2.14        3.02
##  7    175            1.23        3.02
##  8    194            3.34        3.34
##  9    194            2.97        3.34
## 10    194            2.29        3.34
## # … with 4,023 more rows
```

When columns are sequentially named, `starts_with()` makes selecting columns easier.

```r
select(fish, starts_with("radii"))
```

```
## # A tibble: 4,033 × 1
##    radii_length_mm
##              <dbl>
##  1            2.70
##  2            2.04
##  3            1.31
##  4            3.02
##  5            2.67
##  6            2.14
##  7            1.23
##  8            3.34
##  9            2.97
## 10            2.29
## # … with 4,023 more rows
```

Options to select columns based on a specific criteria include:  
1. ends_with() = Select columns that end with a character string  
2. contains() = Select columns that contain a character string  
3. matches() = Select columns that match a regular expression  
4. one_of() = Select columns names that are from a group of names  


```r
names(fish)
```

```
## [1] "lakeid"          "fish_id"         "annnumber"       "length"         
## [5] "radii_length_mm" "scalelength"
```


```r
select(fish, ends_with("id"))
```

```
## # A tibble: 4,033 × 2
##    lakeid fish_id
##    <chr>    <dbl>
##  1 AL         299
##  2 AL         299
##  3 AL         299
##  4 AL         300
##  5 AL         300
##  6 AL         300
##  7 AL         300
##  8 AL         301
##  9 AL         301
## 10 AL         301
## # … with 4,023 more rows
```


```r
select(fish, contains("fish"))
```

```
## # A tibble: 4,033 × 1
##    fish_id
##      <dbl>
##  1     299
##  2     299
##  3     299
##  4     300
##  5     300
##  6     300
##  7     300
##  8     301
##  9     301
## 10     301
## # … with 4,023 more rows
```

We won't cover [regex](https://en.wikipedia.org/wiki/Regular_expression) in this class, but the following code is helpful when you know that a column contains a letter (in this case "a") followed by a subsequent string (in this case "er").  

```r
select(fish, matches("a.+er"))
```

```
## # A tibble: 4,033 × 1
##    annnumber
##    <chr>    
##  1 EDGE     
##  2 2        
##  3 1        
##  4 EDGE     
##  5 3        
##  6 2        
##  7 1        
##  8 EDGE     
##  9 3        
## 10 2        
## # … with 4,023 more rows
```

You can also select columns based on the class of data.  

```r
glimpse(fish)
```

```
## Rows: 4,033
## Columns: 6
## $ lakeid          <chr> "AL", "AL", "AL", "AL", "AL", "AL", "AL", "AL", "AL", …
## $ fish_id         <dbl> 299, 299, 299, 300, 300, 300, 300, 301, 301, 301, 301,…
## $ annnumber       <chr> "EDGE", "2", "1", "EDGE", "3", "2", "1", "EDGE", "3", …
## $ length          <dbl> 167, 167, 167, 175, 175, 175, 175, 194, 194, 194, 194,…
## $ radii_length_mm <dbl> 2.697443, 2.037518, 1.311795, 3.015477, 2.670733, 2.13…
## $ scalelength     <dbl> 2.697443, 2.697443, 2.697443, 3.015477, 3.015477, 3.01…
```


```r
select_if(fish, is.numeric)
```

```
## # A tibble: 4,033 × 4
##    fish_id length radii_length_mm scalelength
##      <dbl>  <dbl>           <dbl>       <dbl>
##  1     299    167            2.70        2.70
##  2     299    167            2.04        2.70
##  3     299    167            1.31        2.70
##  4     300    175            3.02        3.02
##  5     300    175            2.67        3.02
##  6     300    175            2.14        3.02
##  7     300    175            1.23        3.02
##  8     301    194            3.34        3.34
##  9     301    194            2.97        3.34
## 10     301    194            2.29        3.34
## # … with 4,023 more rows
```

To select all columns that are *not* a class of data, you need to add a `~`.

```r
select_if(fish, ~!is.numeric(.))
```

```
## # A tibble: 4,033 × 2
##    lakeid annnumber
##    <chr>  <chr>    
##  1 AL     EDGE     
##  2 AL     2        
##  3 AL     1        
##  4 AL     EDGE     
##  5 AL     3        
##  6 AL     2        
##  7 AL     1        
##  8 AL     EDGE     
##  9 AL     3        
## 10 AL     2        
## # … with 4,023 more rows
```

## Practice  
For this exercise we will use life history data for mammals. The [data](http://esapubs.org/archive/ecol/E084/093/) are from:  
**S. K. Morgan Ernest. 2003. Life history characteristics of placental non-volant mammals. Ecology 84:3402.**  

```r
mammals <- readr::read_csv("data/mammal_lifehistories_v2.csv")
```

```
## Rows: 1440 Columns: 13
```

```
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (4): order, family, Genus, species
## dbl (9): mass, gestation, newborn, weaning, wean mass, AFR, max. life, litte...
```

```
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

1. Use one or more of your favorite functions to assess the structure of the data.  

```r
glimpse(mammals)
```

```
## Rows: 1,440
## Columns: 13
## $ order          <chr> "Artiodactyla", "Artiodactyla", "Artiodactyla", "Artiod…
## $ family         <chr> "Antilocapridae", "Bovidae", "Bovidae", "Bovidae", "Bov…
## $ Genus          <chr> "Antilocapra", "Addax", "Aepyceros", "Alcelaphus", "Amm…
## $ species        <chr> "americana", "nasomaculatus", "melampus", "buselaphus",…
## $ mass           <dbl> 45375.0, 182375.0, 41480.0, 150000.0, 28500.0, 55500.0,…
## $ gestation      <dbl> 8.13, 9.39, 6.35, 7.90, 6.80, 5.08, 5.72, 5.50, 8.93, 9…
## $ newborn        <dbl> 3246.36, 5480.00, 5093.00, 10166.67, -999.00, 3810.00, …
## $ weaning        <dbl> 3.00, 6.50, 5.63, 6.50, -999.00, 4.00, 4.04, 2.13, 10.7…
## $ `wean mass`    <dbl> 8900, -999, 15900, -999, -999, -999, -999, -999, 157500…
## $ AFR            <dbl> 13.53, 27.27, 16.66, 23.02, -999.00, 14.89, 10.23, 20.1…
## $ `max. life`    <dbl> 142, 308, 213, 240, -999, 251, 228, 255, 300, 324, 300,…
## $ `litter size`  <dbl> 1.85, 1.00, 1.00, 1.00, 1.00, 1.37, 1.00, 1.00, 1.00, 1…
## $ `litters/year` <dbl> 1.00, 0.99, 0.95, -999.00, -999.00, 2.00, -999.00, 1.89…
```

```r
mammals
```

```
## # A tibble: 1,440 × 13
##    order   family   Genus  species    mass gestation newborn weaning `wean mass`
##    <chr>   <chr>    <chr>  <chr>     <dbl>     <dbl>   <dbl>   <dbl>       <dbl>
##  1 Artiod… Antiloc… Antil… america… 4.54e4      8.13   3246.    3           8900
##  2 Artiod… Bovidae  Addax  nasomac… 1.82e5      9.39   5480     6.5         -999
##  3 Artiod… Bovidae  Aepyc… melampus 4.15e4      6.35   5093     5.63       15900
##  4 Artiod… Bovidae  Alcel… buselap… 1.5 e5      7.9   10167.    6.5         -999
##  5 Artiod… Bovidae  Ammod… clarkei  2.85e4      6.8    -999  -999           -999
##  6 Artiod… Bovidae  Ammot… lervia   5.55e4      5.08   3810     4           -999
##  7 Artiod… Bovidae  Antid… marsupi… 3   e4      5.72   3910     4.04        -999
##  8 Artiod… Bovidae  Antil… cervica… 3.75e4      5.5    3846     2.13        -999
##  9 Artiod… Bovidae  Bison  bison    4.98e5      8.93  20000    10.7       157500
## 10 Artiod… Bovidae  Bison  bonasus  5   e5      9.14  23000.    6.6         -999
## # … with 1,430 more rows, and 4 more variables: AFR <dbl>, max. life <dbl>,
## #   litter size <dbl>, litters/year <dbl>
```

2. Are there any NAs? Are you sure? Try taking an average of `max.life` as a test.  

```r
anyNA(mammals) # after running code, no NAs present!
```

```
## [1] FALSE
```

```r
max_life_column <- c(select(mammals, contains("life")))
mean(max_life_column) # Hmm.. weird. No NAs were present after running anyNA, but I guess that the -999 values (or the fact that there are thousands of entries may mess up the code).
```

```
## Warning in mean.default(max_life_column): argument is not numeric or logical:
## returning NA
```

```
## [1] NA
```

3. What are the names of the columns in the `mammals` data?

*order, family, Genus, species, mass, gestation, newborn, weaning, wean mass, AFR, max. life, litter size, litters/year*


```r
names(mammals)
```

```
##  [1] "order"        "family"       "Genus"        "species"      "mass"        
##  [6] "gestation"    "newborn"      "weaning"      "wean mass"    "AFR"         
## [11] "max. life"    "litter size"  "litters/year"
```

4. Rename any columns that have capitol letters or punctuation issues.  

```r
rename(mammals, "Order" = "order", "Family" = "family", "Species" = "species", "Mass" = "mass", "Gestation" = "gestation", "Newborn" = "newborn", "Weaning" = "weaning", "Wean Mass" = "wean mass", "Max Life" = "max. life", "Litter Size" = "litter size", "Litters/Year" = "litters/year")
```

```
## # A tibble: 1,440 × 13
##    Order   Family   Genus  Species    Mass Gestation Newborn Weaning `Wean Mass`
##    <chr>   <chr>    <chr>  <chr>     <dbl>     <dbl>   <dbl>   <dbl>       <dbl>
##  1 Artiod… Antiloc… Antil… america… 4.54e4      8.13   3246.    3           8900
##  2 Artiod… Bovidae  Addax  nasomac… 1.82e5      9.39   5480     6.5         -999
##  3 Artiod… Bovidae  Aepyc… melampus 4.15e4      6.35   5093     5.63       15900
##  4 Artiod… Bovidae  Alcel… buselap… 1.5 e5      7.9   10167.    6.5         -999
##  5 Artiod… Bovidae  Ammod… clarkei  2.85e4      6.8    -999  -999           -999
##  6 Artiod… Bovidae  Ammot… lervia   5.55e4      5.08   3810     4           -999
##  7 Artiod… Bovidae  Antid… marsupi… 3   e4      5.72   3910     4.04        -999
##  8 Artiod… Bovidae  Antil… cervica… 3.75e4      5.5    3846     2.13        -999
##  9 Artiod… Bovidae  Bison  bison    4.98e5      8.93  20000    10.7       157500
## 10 Artiod… Bovidae  Bison  bonasus  5   e5      9.14  23000.    6.6         -999
## # … with 1,430 more rows, and 4 more variables: AFR <dbl>, Max Life <dbl>,
## #   Litter Size <dbl>, Litters/Year <dbl>
```

5. We are only interested in the variables `genus`, `species`, and `mass`. Use `select()` to build a new dataframe `mass` focused on these variables.

```r
select(mammals, "mass", "Genus", "species")
```

```
## # A tibble: 1,440 × 3
##       mass Genus       species      
##      <dbl> <chr>       <chr>        
##  1  45375  Antilocapra americana    
##  2 182375  Addax       nasomaculatus
##  3  41480  Aepyceros   melampus     
##  4 150000  Alcelaphus  buselaphus   
##  5  28500  Ammodorcas  clarkei      
##  6  55500  Ammotragus  lervia       
##  7  30000  Antidorcas  marsupialis  
##  8  37500  Antilope    cervicapra   
##  9 497667. Bison       bison        
## 10 500000  Bison       bonasus      
## # … with 1,430 more rows
```

```r
data.frame(select(mammals, "mass", "Genus", "species"))
```

```
##              mass            Genus           species
## 1        45375.00      Antilocapra         americana
## 2       182375.00            Addax     nasomaculatus
## 3        41480.00        Aepyceros          melampus
## 4       150000.00       Alcelaphus        buselaphus
## 5        28500.00       Ammodorcas           clarkei
## 6        55500.00       Ammotragus            lervia
## 7        30000.00       Antidorcas       marsupialis
## 8        37500.00         Antilope        cervicapra
## 9       497666.67            Bison             bison
## 10      500000.00            Bison           bonasus
## 11      333000.00              Bos         grunniens
## 12      800000.00              Bos         frontalis
## 13      666666.67              Bos         javanicus
## 14      169000.00       Boselaphus      tragocamelus
## 15        -999.00          Bubalus    depressicornis
## 16      233333.33          Bubalus       mindorensis
## 17      950000.00          Bubalus           bubalis
## 18      302000.00         Budorcas         taxicolor
## 19       55000.00            Capra         caucasica
## 20       41000.00            Capra         falconeri
## 21       71500.00            Capra              ibex
## 22       50000.00            Capra    cylindricornis
## 23       60000.00            Capra            hircus
## 24       18100.00      Cephalophus             niger
## 25       13900.00      Cephalophus        nigrifrons
## 26       10000.00      Cephalophus        natalensis
## 27       12700.00      Cephalophus       leucogaster
## 28       20000.00      Cephalophus           ogilbyi
## 29        -999.00      Cephalophus             zebra
## 30       12500.00      Cephalophus         rufilatus
## 31       11600.00      Cephalophus          dorsalis
## 32        6250.00      Cephalophus         monticola
## 33       62500.00      Cephalophus       silvicultor
## 34        9000.00      Cephalophus         maxwellii
## 35      132250.00     Connochaetes              gnou
## 36      164500.00     Connochaetes          taurinus
## 37      159000.00       Damaliscus           hunteri
## 38       84500.00       Damaliscus          pygargus
## 39      128000.00       Damaliscus           lunatus
## 40       40000.00          Gazella     soemmerringii
## 41       27000.00          Gazella         rufifrons
## 42       73000.00          Gazella              dama
## 43       23600.00          Gazella        leptoceros
## 44       47636.36          Gazella            granti
## 45       20000.00          Gazella            spekei
## 46       17500.00          Gazella           cuvieri
## 47       16985.00          Gazella            dorcas
## 48       25500.00          Gazella      subgutturosa
## 49       16300.00          Gazella         thomsonii
## 50       20750.00          Gazella           gazella
## 51        -999.00       Hemitragus         hylocrius
## 52       35200.00       Hemitragus        jemlahicus
## 53      242000.00      Hippotragus           equinus
## 54      200500.00      Hippotragus             niger
## 55       17500.00            Kobus         megaceros
## 56       63950.00            Kobus          vardonii
## 57       81733.33            Kobus             leche
## 58      175333.33            Kobus    ellipsiprymnus
## 59       58600.00            Kobus               kob
## 60       41333.33      Litocranius           walleri
## 61        3250.00          Madoqua          saltiana
## 62        4550.00          Madoqua         guentheri
## 63        4876.67          Madoqua            kirkii
## 64       30000.00      Naemorhedus           crispus
## 65       87500.00      Naemorhedus      sumatraensis
## 66       27666.67      Naemorhedus             goral
## 67        2500.00        Neotragus            batesi
## 68        7166.67        Neotragus         moschatus
## 69       82350.00         Oreamnos        americanus
## 70       13000.00       Oreotragus        oreotragus
## 71      177500.00             Oryx            dammah
## 72      121350.00             Oryx          leucoryx
## 73      195000.00             Oryx           gazella
## 74       17500.00          Ourebia            ourebi
## 75      258000.00           Ovibos         moschatus
## 76       50500.00             Ovis          nivicola
## 77       60000.00             Ovis            vignei
## 78       57666.67             Ovis             dalli
## 79       68166.67             Ovis        canadensis
## 80      180000.00             Ovis             ammon
## 81       50000.00             Ovis             aries
## 82       27500.00       Pantholops         hodgsonii
## 83       25000.00            Pelea         capreolus
## 84       24000.00         Procapra         gutturosa
## 85       46666.67         Pseudois            nayaur
## 86       11333.33       Raphicerus        campestris
## 87       48850.00          Redunca         arundinum
## 88       40000.00          Redunca           redunca
## 89       30000.00          Redunca       fulvorufula
## 90       26100.00        Rupicapra         rupicapra
## 91       38250.00            Saiga          tatarica
## 92      153000.00       Sigmoceros    lichtensteinii
## 93       19000.00       Sylvicapra           grimmia
## 94      504666.67         Syncerus            caffer
## 95      680000.00      Taurotragus         derbianus
## 96      432500.00      Taurotragus              oryx
## 97       19000.00       Tetracerus      quadricornis
## 98      218000.00      Tragelaphus           buxtoni
## 99      186250.00      Tragelaphus         eurycerus
## 100      87500.00      Tragelaphus            spekii
## 101     190000.00      Tragelaphus      strepsiceros
## 102     100666.67      Tragelaphus           angasii
## 103      64500.00      Tragelaphus          imberbis
## 104      36250.00      Tragelaphus          scriptus
## 105       -999.00          Camelus        bactrianus
## 106     434000.00          Camelus       dromedarius
## 107     142500.00             Lama             glama
## 108      60000.00             Lama             pacos
## 109     110000.00             Lama          guanicoe
## 110      50000.00          Vicugna           vicugna
## 111     351000.00            Alces             alces
## 112      33500.00             Axis          porcinus
## 113      55000.00             Axis              axis
## 114     102500.00      Blastocerus        dichotomus
## 115      21666.67        Capreolus         capreolus
## 116      39450.00        Capreolus          pygargus
## 117     143000.00           Cervus        duvaucelii
## 118     120333.33           Cervus           elaphus
## 119     125000.00           Cervus       albirostris
## 120      53000.00           Cervus        timorensis
## 121     171000.00           Cervus          unicolor
## 122      73000.00           Cervus             eldii
## 123      96500.00           Cervus            nippon
## 124      54500.00             Dama              dama
## 125      33500.00        Elaphodus       cephalophus
## 126     149000.00        Elaphurus        davidianus
## 127      70000.00     Hippocamelus          bisulcus
## 128      68600.00     Hippocamelus        antisensis
## 129      14000.00       Hydropotes           inermis
## 130       8200.00           Mazama            rufina
## 131      16650.00           Mazama       gouazoupira
## 132      23000.00           Mazama         americana
## 133      14000.00        Muntiacus           muntjak
## 134      12000.00        Muntiacus           reevesi
## 135      55766.67       Odocoileus          hemionus
## 136      59500.00       Odocoileus       virginianus
## 137      35000.00       Ozotoceros       bezoarticus
## 138       9750.00             Pudu    mephistophiles
## 139       8250.00             Pudu              puda
## 140     113200.00         Rangifer          tarandus
## 141     800000.00          Giraffa    camelopardalis
## 142     287500.00           Okapia         johnstoni
## 143     215000.00     Hexaprotodon       liberiensis
## 144    1258333.33     Hippopotamus         amphibius
## 145      11000.00          Moschus      chrysogaster
## 146      10900.00          Moschus       berezovskii
## 147      12500.00          Moschus       moschiferus
## 148     100000.00        Babyrousa         babyrussa
## 149     202500.00      Hylochoerus    meinertzhageni
## 150      71000.00     Phacochoerus       aethiopicus
## 151      71000.00    Potamochoerus            porcus
## 152       8150.00              Sus         salvanius
## 153     102750.00              Sus          barbatus
## 154     100900.00              Sus            scrofa
## 155      35300.00        Catagonus           wagneri
## 156      19200.00           Pecari            tajacu
## 157      33750.00          Tayassu            pecari
## 158      12250.00       Hyemoschus         aquaticus
## 159       2450.00        Moschiola           meminna
## 160       3850.00         Tragulus         javanicus
## 161       5900.00         Tragulus              napu
## 162       4500.00           Alopex           lagopus
## 163       9500.00       Atelocynus          microtis
## 164      12675.00            Canis          simensis
## 165      11000.00            Canis            aureus
## 166      26000.00            Canis             rufus
## 167      34875.00            Canis             lupus
## 168       9750.00            Canis         mesomelas
## 169      11800.00            Canis           latrans
## 170       8250.00            Canis           adustus
## 171       6500.00        Cerdocyon             thous
## 172      23000.00       Chrysocyon        brachyurus
## 173      12760.00             Cuon           alpinus
## 174      27133.33           Lycaon            pictus
## 175       4232.00      Nyctereutes      procyonoides
## 176       4150.00          Otocyon         megalotis
## 177       4690.00      Pseudalopex       gymnocercus
## 178       3990.00      Pseudalopex           griseus
## 179      13000.00      Pseudalopex          culpaeus
## 180       6000.00         Speothos         venaticus
## 181       4220.00          Urocyon  cinereoargenteus
## 182       1890.67          Urocyon        littoralis
## 183       1000.00           Vulpes              cana
## 184       4000.00           Vulpes             chama
## 185       2250.00           Vulpes         rueppelli
## 186       7000.00           Vulpes         ferrilata
## 187       2550.00           Vulpes           pallida
## 188       1800.00           Vulpes       bengalensis
## 189       2700.00           Vulpes            corsac
## 190       5662.50           Vulpes            vulpes
## 191       1200.00           Vulpes             zerda
## 192       2766.67           Vulpes             velox
## 193      58750.00         Acinonyx           jubatus
## 194      14000.00          Caracal           caracal
## 195      11500.00         Catopuma        temminckii
## 196      10000.00            Felis             chaus
## 197       2350.00            Felis         margarita
## 198       4150.00            Felis        silvestris
## 199       2125.00            Felis          nigripes
## 200       6750.00      Herpailurus        yaguarondi
## 201       3666.67        Leopardus            wiedii
## 202       2250.00        Leopardus          tigrinus
## 203       8800.00        Leopardus          pardalis
## 204      13350.00      Leptailurus            serval
## 205       9400.00             Lynx          pardinus
## 206       8600.00             Lynx             rufus
## 207      18026.67             Lynx              lynx
## 208       8900.00             Lynx        canadensis
## 209      19500.00         Neofelis          nebulosa
## 210       2950.00        Oncifelis          colocolo
## 211       4000.00        Oncifelis         geoffroyi
## 212       3500.00       Otocolobus             manul
## 213     119700.00         Panthera            tigris
## 214      81150.00         Panthera              onca
## 215     139500.00         Panthera               leo
## 216      42325.00         Panthera            pardus
## 217       3500.00       Pardofelis         marmorata
## 218      10850.00     Prionailurus        viverrinus
## 219       1350.00     Prionailurus       rubiginosus
## 220       4150.00     Prionailurus       bengalensis
## 221      10650.00         Profelis            aurata
## 222      48000.00             Puma          concolor
## 223      45625.00            Uncia             uncia
## 224       3300.00           Atilax       paludinosus
## 225       1570.00         Bdeogale       crassicauda
## 226        727.00      Crossarchus          obscurus
## 227        598.50         Cynictis       penicillata
## 228        683.00        Galerella      pulverulenta
## 229        448.00        Galerella         sanguinea
## 230        800.00          Galidia           elegans
## 231       -999.00       Galidictis          fasciata
## 232        274.80         Helogale           parvula
## 233        800.00        Herpestes         javanicus
## 234       1008.00        Herpestes         edwardsii
## 235       2920.00        Herpestes         ichneumon
## 236       3150.00        Ichneumia         albicauda
## 237       1331.50           Mungos             mungo
## 238        700.00      Mungotictis      decemlineata
## 239       1800.00     Paracynictis           selousi
## 240        776.00         Suricata         suricatta
## 241      63000.00          Crocuta           crocuta
## 242      40000.00           Hyaena            hyaena
## 243      60200.00       Parahyaena           brunnea
## 244      10000.00         Proteles         cristatus
## 245       3000.00         Amblonyx          cinereus
## 246      24000.00            Aonyx          congicus
## 247      11800.00            Aonyx          capensis
## 248      10500.00         Arctonyx          collaris
## 249       3500.00        Conepatus        leuconotus
## 250       1200.00        Conepatus      semistriatus
## 251       1996.67        Conepatus        mesoleucus
## 252       4500.00             Eira           barbara
## 253      21800.00          Enhydra            lutris
## 254       2350.00         Galictis           vittata
## 255      16333.33             Gulo              gulo
## 256        500.00          Ictonyx            libyca
## 257        765.00          Ictonyx          striatus
## 258       7500.00           Lontra       longicaudis
## 259       -999.00           Lontra            felina
## 260       6225.00           Lontra        canadensis
## 261       4000.00            Lutra      maculicollis
## 262       6750.00            Lutra             lutra
## 263       9000.00        Lutrogale     perspicillata
## 264       2500.00           Martes         flavigula
## 265       1066.67           Martes         zibellina
## 266       1700.00           Martes             foina
## 267        606.67           Martes         americana
## 268       2600.00           Martes          pennanti
## 269       1300.00           Martes            martes
## 270      13000.00            Meles             meles
## 271      10000.00        Mellivora          capensis
## 272       2000.00         Melogale         personata
## 273        965.00         Mephitis          macroura
## 274       1821.25         Mephitis          mephitis
## 275        809.00          Mustela          nigripes
## 276        440.00          Mustela          lutreola
## 277        171.00          Mustela           altaica
## 278        405.00          Mustela          sibirica
## 279        899.75          Mustela             vison
## 280        150.60          Mustela           frenata
## 281        110.33          Mustela           erminea
## 282       1350.00          Mustela       eversmannii
## 283        588.67          Mustela          putorius
## 284         49.75          Mustela           nivalis
## 285        257.50      Poecilogale         albinucha
## 286      24000.00        Pteronura      brasiliensis
## 287        260.00        Spilogale           pygmaea
## 288        511.50        Spilogale          putorius
## 289       6050.00          Taxidea             taxus
## 290        542.50          Vormela         peregusna
## 291     650000.00         Odobenus          rosmarus
## 292      27000.00    Arctocephalus     galapagoensis
## 293      50000.00    Arctocephalus        tropicalis
## 294      55000.00    Arctocephalus          forsteri
## 295      40500.00    Arctocephalus           gazella
## 296      77666.67    Arctocephalus          pusillus
## 297      45000.00    Arctocephalus         australis
## 298     288932.50       Eumetopias           jubatus
## 299      79633.33         Neophoca           cinerea
## 300     140000.00           Otaria           byronia
## 301       -999.00       Phocarctos           hookeri
## 302      84666.67         Zalophus     californianus
## 303      49100.00      Callorhinus           ursinus
## 304     212000.00       Cystophora          cristata
## 305     286666.67       Erignathus          barbatus
## 306     186000.00      Halichoerus            grypus
## 307     398666.67         Hydrurga          leptonyx
## 308     421666.67    Leptonychotes         weddellii
## 309     238333.33          Lobodon     carcinophagus
## 310     579400.00         Mirounga           leonina
## 311     716666.67         Mirounga    angustirostris
## 312     275000.00         Monachus          monachus
## 313     173000.00         Monachus     schauinslandi
## 314     187666.67      Ommatophoca            rossii
## 315      80000.00            Phoca            largha
## 316      86000.00            Phoca           caspica
## 317      86140.00            Phoca           hispida
## 318     128786.67            Phoca      groenlandica
## 319      82500.00            Phoca          fasciata
## 320     101250.00            Phoca          vitulina
## 321      81666.67            Phoca          sibirica
## 322       1242.50      Bassaricyon            gabbii
## 323        900.00      Bassariscus       sumichrasti
## 324        975.50      Bassariscus           astutus
## 325       4750.00            Nasua             nasua
## 326       3750.00            Nasua            narica
## 327       3000.00            Potos            flavus
## 328       6270.00          Procyon       cancrivorus
## 329       4400.00          Procyon             lotor
## 330     111600.00       Ailuropoda       melanoleuca
## 331       4325.00          Ailurus           fulgens
## 332      46000.00        Helarctos         malayanus
## 333     100000.00         Melursus           ursinus
## 334      61000.00       Tremarctos           ornatus
## 335      76666.67            Ursus        thibetanus
## 336     286366.67            Ursus         maritimus
## 337     203500.00            Ursus            arctos
## 338     110560.00            Ursus        americanus
## 339      12250.00        Arctictis         binturong
## 340       2250.00     Arctogalidia        trivirgata
## 341       3250.00       Chrotogale           owstoni
## 342      13666.67      Civettictis           civetta
## 343       9500.00     Cryptoprocta             ferox
## 344       4000.00         Cynogale         bennettii
## 345       3000.00         Eupleres          goudotii
## 346       1540.00            Fossa           fossana
## 347       2225.00          Genetta          maculata
## 348       1866.67          Genetta           genetta
## 349       1820.00          Genetta           tigrina
## 350       2375.00        Hemigalus         derbyanus
## 351       1900.00         Nandinia          binotata
## 352       4300.00           Paguma           larvata
## 353       2380.00      Paradoxurus       zeylonensis
## 354       3100.00      Paradoxurus    hermaphroditus
## 355       4500.00        Prionodon        pardicolor
## 356        698.00        Prionodon           linsang
## 357      10000.00          Viverra           zibetha
## 358       3000.00      Viverricula            indica
## 359   80000000.00          Balaena        mysticetus
## 360   23000000.00        Eubalaena         australis
## 361   23000000.00        Eubalaena         glacialis
## 362   66800000.00     Balaenoptera          physalus
## 363  149000000.00     Balaenoptera          musculus
## 364   14766666.67     Balaenoptera          borealis
## 365   20000000.00     Balaenoptera             edeni
## 366   16266666.67     Balaenoptera     acutorostrata
## 367   30000000.00        Megaptera      novaeangliae
## 368       -999.00  Cephalorhynchus        heavisidii
## 369      72400.00  Cephalorhynchus       commersonii
## 370       -999.00  Cephalorhynchus           hectori
## 371      76365.00        Delphinus           delphis
## 372     133000.00           Feresa         attenuata
## 373    1060000.00     Globicephala             melas
## 374     726000.00     Globicephala     macrorhynchus
## 375     425000.00          Grampus           griseus
## 376     164000.00    Lagenodelphis             hosei
## 377     180000.00   Lagenorhynchus       albirostris
## 378     103000.00   Lagenorhynchus       obliquidens
## 379     182000.00   Lagenorhynchus            acutus
## 380     110000.00   Lagenorhynchus          obscurus
## 381     190000.00         Orcaella      brevirostris
## 382    4300000.00          Orcinus              orca
## 383     206000.00    Peponocephala           electra
## 384    1360000.00        Pseudorca        crassidens
## 385      46666.67          Sotalia       fluviatilis
## 386     122950.00         Stenella      coeruleoalba
## 387      70333.33         Stenella      longirostris
## 388      68700.00         Stenella         attenuata
## 389     122500.00            Steno       bredanensis
## 390     173333.33         Tursiops         truncatus
## 391   25066666.67     Eschrichtius          robustus
## 392     665000.00   Delphinapterus            leucas
## 393     900000.00          Monodon         monoceros
## 394    3200000.00          Caperea         marginata
## 395      32500.00      Neophocaena      phocaenoides
## 396      45000.00         Phocoena             sinus
## 397      53183.33         Phocoena          phocoena
## 398     101666.67     Phocoenoides             dalli
## 399     190233.33            Kogia             simus
## 400     363000.00            Kogia         breviceps
## 401   15400000.00         Physeter           catodon
## 402      96500.00             Inia       geoffrensis
## 403      83500.00          Lipotes        vexillifer
## 404       -999.00       Platanista             minor
## 405      21000.00       Platanista         gangetica
## 406      40333.33       Pontoporia       blainvillei
## 407       -999.00        Berardius           arnuxii
## 408    8200000.00        Berardius           bairdii
## 409    5266666.67       Hyperoodon        ampullatus
## 410    1500000.00       Mesoplodon        carlhubbsi
## 411    1050000.00       Mesoplodon      densirostris
## 412    3400000.00       Mesoplodon            bidens
## 413    2952500.00          Ziphius       cavirostris
## 414       1000.00     Cynocephalus        variegatus
## 415       -999.00     Cynocephalus            volans
## 416       2950.00      Dendrohyrax          arboreus
## 417       3116.67      Dendrohyrax          dorsalis
## 418       2456.67      Heterohyrax            brucei
## 419       3600.00         Procavia          capensis
## 420         64.80       Amblysomus       hottentotus
## 421         39.80    Chrysochloris        stuhlmanni
## 422         42.00    Chrysochloris          asiatica
## 423        104.00     Chrysospalax          villosus
## 424         23.20       Eremitalpa            granti
## 425        745.00         Atelerix           algirus
## 426        334.00         Atelerix       albiventris
## 427        309.00         Atelerix         frontalis
## 428        956.50      Echinosorex           gymnura
## 429        719.00        Erinaceus          concolor
## 430        771.00        Erinaceus         europaeus
## 431        171.00      Hemiechinus          micropus
## 432        292.00      Hemiechinus         hypomelas
## 433        358.00      Hemiechinus       aethiopicus
## 434        342.00      Hemiechinus           auritus
## 435         64.00          Hylomys           suillus
## 436       1000.00        Solenodon           cubanus
## 437        900.00        Solenodon         paradoxus
## 438          9.25          Blarina      carolinensis
## 439         14.50          Blarina         hylophaga
## 440         21.63          Blarina        brevicauda
## 441         12.00        Crocidura        fuliginosa
## 442         15.00        Crocidura             turba
## 443          7.50        Crocidura       canariensis
## 444          5.00        Crocidura       fuscomurina
## 445          2.50        Crocidura         planiceps
## 446          7.20        Crocidura           crossei
## 447          9.70        Crocidura       mariquensis
## 448         17.10        Crocidura            viaria
## 449         22.00        Crocidura        flavescens
## 450          9.91        Crocidura          leucodon
## 451         13.67        Crocidura             hirta
## 452         11.63        Crocidura           russula
## 453          7.70        Crocidura        suaveolens
## 454          6.20        Cryptotis             parva
## 455         10.00     Diplomesodon        pulchellum
## 456         11.00         Myosorex            varius
## 457         13.00         Myosorex             cafer
## 458       -999.00         Myosorex             geata
## 459         16.00           Neomys          anomalus
## 460         14.32           Neomys           fodiens
## 461          4.15       Notiosorex         crawfordi
## 462       -999.00            Sorex         mirabilis
## 463          4.90            Sorex            dispar
## 464          6.71            Sorex         pacificus
## 465          8.34            Sorex           bairdii
## 466         15.88            Sorex          bendirii
## 467          9.60            Sorex         coronatus
## 468          4.00            Sorex          merriami
## 469          4.26            Sorex          cinereus
## 470          8.20            Sorex          arcticus
## 471          5.57            Sorex           ornatus
## 472          4.37            Sorex              hoyi
## 473          6.00            Sorex        monticolus
## 474          4.44            Sorex       trowbridgii
## 475          7.25            Sorex           minutus
## 476          3.00            Sorex           haydeni
## 477          8.98            Sorex           araneus
## 478          3.68            Sorex      longirostris
## 479          2.50            Sorex             nanus
## 480          8.20            Sorex            fumeus
## 481          5.58            Sorex           vagrans
## 482         12.47            Sorex         palustris
## 483         45.87           Suncus           murinus
## 484       -999.00           Suncus           varilla
## 485          2.10           Suncus          etruscus
## 486       -999.00       Surdisorex             norae
## 487       -999.00       Surdisorex           polulus
## 488       -999.00       Sylvisorex            granti
## 489         55.33        Condylura          cristata
## 490        383.00          Desmana          moschata
## 491         61.30          Galemys        pyrenaicus
## 492         10.00     Neurotrichus           gibbsii
## 493         50.18      Parascalops           breweri
## 494         74.60         Scalopus         aquaticus
## 495        118.80         Scapanus        townsendii
## 496         76.90         Scapanus           orarius
## 497         52.00         Scapanus         latimanus
## 498         85.00            Talpa           altaica
## 499        133.30            Talpa          europaea
## 500         19.50       Urotrichus         talpoides
## 501        180.00         Echinops          telfairi
## 502          6.70          Geogale            aurita
## 503        149.60     Hemicentetes      semispinosus
## 504         79.50        Limnogale          mergulus
## 505         37.80        Microgale           dobsoni
## 506         48.10        Microgale          talazaci
## 507         69.60  Micropotamogale          lamottei
## 508        761.70       Potamogale             velox
## 509        259.45          Setifer           setosus
## 510        884.00           Tenrec         ecaudatus
## 511        421.33      Brachylagus        idahoensis
## 512       1250.00        Bunolagus      monticularis
## 513       2500.00       Caprolagus          hispidus
## 514       2099.50            Lepus       nigricollis
## 515       1820.00            Lepus          callotis
## 516       -999.00            Lepus         insularis
## 517       2110.00            Lepus             tolai
## 518       4506.33            Lepus             othus
## 519       3766.00            Lepus          arcticus
## 520       3030.43            Lepus           timidus
## 521       1489.25            Lepus        americanus
## 522       3035.50            Lepus        townsendii
## 523       2358.00            Lepus          capensis
## 524       3759.67            Lepus            alleni
## 525       2317.00            Lepus      californicus
## 526       3673.33            Lepus         europaeus
## 527       2686.00            Lepus         saxatilis
## 528       1558.25      Oryctolagus         cuniculus
## 529       2500.00         Poelagus         marjorita
## 530       2470.00       Pronolagus         randensis
## 531       1945.00       Pronolagus    crassicaudatus
## 532       1700.00       Pronolagus         rupestris
## 533        494.50      Romerolagus             diazi
## 534        950.00       Sylvilagus      brasiliensis
## 535       1233.00       Sylvilagus         palustris
## 536        902.60       Sylvilagus    transitionalis
## 537       2120.67       Sylvilagus         aquaticus
## 538        707.67       Sylvilagus          bachmani
## 539        759.96       Sylvilagus         nuttallii
## 540       1269.00       Sylvilagus        floridanus
## 541        909.38       Sylvilagus         audubonii
## 542        167.00         Ochotona          macrotis
## 543       -999.00         Ochotona         curzoniae
## 544        129.00         Ochotona          collaris
## 545        115.00         Ochotona            roylei
## 546        128.00         Ochotona          dauurica
## 547       -999.00         Ochotona            alpina
## 548        140.20         Ochotona          princeps
## 549       -999.00         Ochotona            rutila
## 550       -999.00         Ochotona           pallasi
## 551       -999.00         Ochotona           pusilla
## 552        250.00         Ochotona         rufescens
## 553         60.00     Elephantulus         rupestris
## 554       -999.00     Elephantulus            fuscus
## 555         51.95     Elephantulus            intufi
## 556         51.94     Elephantulus            myurus
## 557         48.00     Elephantulus            rozeti
## 558         58.00     Elephantulus         rufescens
## 559         44.33     Elephantulus    brachyrhynchus
## 560         40.00    Macroscelides      proboscideus
## 561        220.00      Petrodromus     tetradactylus
## 562        540.00      Rhynchocyon       chrysopygus
## 563     250000.00            Equus            asinus
## 564     275000.00            Equus             kiang
## 565     384000.00            Equus            grevyi
## 566     296000.00            Equus             zebra
## 567     257000.00            Equus        burchellii
## 568     230000.00            Equus          hemionus
## 569    2233333.33    Ceratotherium             simum
## 570    1266666.67     Dicerorhinus       sumatrensis
## 571     927766.67          Diceros          bicornis
## 572    1602333.33       Rhinoceros         unicornis
## 573    1750000.00       Rhinoceros         sondaicus
## 574     148950.00          Tapirus         pinchaque
## 575     200000.00          Tapirus        terrestris
## 576     296250.00          Tapirus           indicus
## 577     300000.00          Tapirus           bairdii
## 578      33000.00            Manis          gigantea
## 579       5150.00            Manis          javanica
## 580       2350.00            Manis      pentadactyla
## 581       1480.00            Manis         tricuspis
## 582       3900.00            Manis     crassicaudata
## 583       2750.00            Manis      tetradactyla
## 584       7230.00            Manis        temminckii
## 585        558.33        Callimico           goeldii
## 586        343.33       Callithrix       humeralifer
## 587        357.25       Callithrix         argentata
## 588        116.83       Callithrix           pygmaea
## 589        307.00       Callithrix       penicillata
## 590        406.00       Callithrix         flaviceps
## 591        309.00       Callithrix           jacchus
## 592        558.00   Leontopithecus           rosalia
## 593        490.00         Saguinus          leucopus
## 594        445.80         Saguinus         imperator
## 595        465.00         Saguinus           bicolor
## 596        411.00         Saguinus       nigricollis
## 597        526.13         Saguinus            mystax
## 598        499.33         Saguinus         geoffroyi
## 599        370.00         Saguinus       fuscicollis
## 600        401.00         Saguinus           oedipus
## 601        501.33         Saguinus          labiatus
## 602        466.75         Saguinus             midas
## 603      11600.00         Alouatta             pigra
## 604       5555.29         Alouatta         seniculus
## 605       5659.14         Alouatta          palliata
## 606       5353.00         Alouatta            caraya
## 607        833.56            Aotus       trivirgatus
## 608        874.00            Aotus         lemurinus
## 609       1230.00            Aotus            azarai
## 610       5400.00           Ateles         belzebuth
## 611       6265.80           Ateles         geoffroyi
## 612       7758.80           Ateles          paniscus
## 613       7761.80           Ateles         fusciceps
## 614      11142.50      Brachyteles       arachnoides
## 615       4092.00          Cacajao            calvus
## 616       1050.00       Callicebus         torquatus
## 617       1120.00       Callicebus           cupreus
## 618        850.00       Callicebus            moloch
## 619       2410.00            Cebus         olivaceus
## 620       2602.57            Cebus         capucinus
## 621       2528.50            Cebus            apella
## 622       2539.07            Cebus         albifrons
## 623       2900.00       Chiropotes         albinasus
## 624       2750.00       Chiropotes           satanas
## 625       6076.25        Lagothrix        lagotricha
## 626       1926.67         Pithecia          monachus
## 627       1377.20         Pithecia          pithecia
## 628        580.00          Saimiri          oerstedi
## 629        607.80          Saimiri          sciureus
## 630       3301.67   Allenopithecus      nigroviridis
## 631       5425.75       Cercocebus         galeritus
## 632       7036.67       Cercocebus         torquatus
## 633       2870.00    Cercopithecus             wolfi
## 634       4700.00    Cercopithecus           lhoesti
## 635       3833.33    Cercopithecus              mona
## 636       4225.33    Cercopithecus         nictitans
## 637       -999.00    Cercopithecus        erythrotis
## 638       2866.25    Cercopithecus            cephus
## 639       2973.67    Cercopithecus          pogonias
## 640       3392.60    Cercopithecus          ascanius
## 641       3150.00    Cercopithecus         campbelli
## 642       5685.71    Cercopithecus             mitis
## 643       3920.00    Cercopithecus           solatus
## 644       4670.17    Cercopithecus         neglectus
## 645       4216.50    Cercopithecus             diana
## 646       3732.64      Chlorocebus          aethiops
## 647       9000.00          Colobus        angolensis
## 648       9500.00          Colobus           satanas
## 649       8508.00          Colobus         polykomos
## 650       9725.17          Colobus           guereza
## 651       5883.40     Erythrocebus             patas
## 652       6725.80       Lophocebus          albigena
## 653      10036.67           Macaca         thibetana
## 654       8857.50           Macaca           fuscata
## 655       5575.00           Macaca             maura
## 656       9752.60           Macaca          sylvanus
## 657       7307.67           Macaca         arctoides
## 658       6211.67           Macaca             nigra
## 659       3495.00           Macaca            sinica
## 660       4875.00           Macaca           silenus
## 661       5413.40           Macaca           mulatta
## 662       6316.67           Macaca          cyclopis
## 663       6133.10           Macaca        nemestrina
## 664       3456.29           Macaca      fascicularis
## 665       3735.00           Macaca           radiata
## 666       9087.50       Mandrillus       leucophaeus
## 667      11916.67       Mandrillus            sphinx
## 668       1093.33      Miopithecus          talapoin
## 669       9462.60          Nasalis          larvatus
## 670      14028.57            Papio         hamadryas
## 671       6200.00        Presbytis            comata
## 672       6300.00        Presbytis         rubicunda
## 673       6400.00        Presbytis        potenziani
## 674       6600.00        Presbytis        melalophos
## 675       3600.00       Procolobus             verus
## 676       6163.50       Procolobus            badius
## 677       9960.00        Pygathrix             bieti
## 678       8310.00        Pygathrix           nemaeus
## 679      11253.33    Semnopithecus          entellus
## 680      12242.33    Theropithecus            gelada
## 681       7325.00   Trachypithecus         francoisi
## 682       8100.00   Trachypithecus              geei
## 683      12000.00   Trachypithecus            johnii
## 684       6572.00   Trachypithecus         cristatus
## 685       7081.75   Trachypithecus           vetulus
## 686       6426.00   Trachypithecus          obscurus
## 687       8400.00   Trachypithecus           phayrei
## 688        394.20     Cheirogaleus             major
## 689        170.60     Cheirogaleus            medius
## 690         69.00       Microcebus             rufus
## 691        306.67       Microcebus         coquereli
## 692         71.00       Microcebus           murinus
## 693        400.00           Phaner          furcifer
## 694       2610.33      Daubentonia  madagascariensis
## 695        280.00         Euoticus       elegantulus
## 696        289.20           Galago            alleni
## 697        180.00           Galago            moholi
## 698        171.38           Galago      senegalensis
## 699         66.00       Galagoides          demidoff
## 700        144.00       Galagoides      zanzibaricus
## 701       1143.33         Otolemur    crassicaudatus
## 702        731.33         Otolemur         garnettii
## 703     101386.11          Gorilla           gorilla
## 704      37618.18              Pan       troglodytes
## 705      32733.33              Pan          paniscus
## 706      37115.60            Pongo          pygmaeus
## 707       6048.40        Hylobates            moloch
## 708       5900.00        Hylobates           klossii
## 709       6500.00        Hylobates           hoolock
## 710       6389.67        Hylobates          concolor
## 711       5666.67        Hylobates            agilis
## 712       9554.67        Hylobates       syndactylus
## 713       5050.56        Hylobates               lar
## 714       1094.43            Avahi           laniger
## 715       7918.00            Indri             indri
## 716       3545.00      Propithecus       tattersalli
## 717       6260.00      Propithecus           diadema
## 718       3657.13      Propithecus         verreauxi
## 719       1765.33          Eulemur       rubriventer
## 720       2171.83          Eulemur            macaco
## 721       1666.20          Eulemur            mongoz
## 722       1389.25          Eulemur         coronatus
## 723       2426.29          Eulemur            fulvus
## 724       1390.00        Hapalemur            aureus
## 725       1202.71        Hapalemur           griseus
## 726       2466.67            Lemur             catta
## 727       3431.43          Varecia         variegata
## 728        314.14       Arctocebus      calabarensis
## 729        237.00            Loris       tardigradus
## 730        307.00       Nycticebus          pygmaeus
## 731        966.20       Nycticebus           coucang
## 732       1083.50     Perodicticus             potto
## 733        700.00        Lepilemur          leucopus
## 734        779.00        Lepilemur      ruficaudatus
## 735        713.00        Lepilemur        mustelinus
## 736       -999.00          Tarsius           pumilis
## 737        107.00          Tarsius            dianae
## 738        112.00          Tarsius          syrichta
## 739        101.20          Tarsius          bancanus
## 740        179.60          Tarsius          spectrum
## 741    3178000.00          Elephas           maximus
## 742    3507000.00        Loxodonta          africana
## 743        250.00         Abrocoma           cinerea
## 744        307.00         Abrocoma          bennetti
## 745       9000.00           Agouti              paca
## 746        389.00       Anomalurus         beecrofti
## 747        770.00       Anomalurus         derbianus
## 748       1650.00       Anomalurus             pelii
## 749         15.75          Idiurus           zenkeri
## 750        687.00       Aplodontia              rufa
## 751        635.00       Bathyergus           suillus
## 752        332.00       Bathyergus           janetta
## 753        144.00        Cryptomys        damarensis
## 754        132.50        Cryptomys       hottentotus
## 755        200.00        Cryptomys  ochraceocinereus
## 756        181.00        Georychus          capensis
## 757        160.00     Heliophobius  argenteocinereus
## 758         35.00   Heterocephalus            glaber
## 759       4683.33         Capromys         pilorides
## 760        660.00      Geocapromys         ingrahami
## 761       1500.00      Geocapromys           brownii
## 762       -999.00        Mysateles         melanurus
## 763       -999.00        Mysateles       prehensilis
## 764       1267.00     Plagiodontia            aedium
## 765      19000.00           Castor             fiber
## 766      19606.67           Castor        canadensis
## 767        637.50            Cavia          tschudii
## 768        341.00            Cavia            aperea
## 769        728.00            Cavia         porcellus
## 770       1600.00       Dolichotis        salinicola
## 771      12500.00       Dolichotis         patagonum
## 772        326.00            Galea            spixii
## 773        337.85            Galea       musteloides
## 774        950.00          Kerodon         rupestris
## 775        350.00       Microcavia         australis
## 776        500.00       Chinchilla      brevicaudata
## 777        642.50       Chinchilla          lanigera
## 778        600.00         Lagidium          viscacia
## 779       1270.00         Lagidium          peruanum
## 780       3250.00       Lagostomus           maximus
## 781        174.75    Ctenodactylus              vali
## 782        288.80    Ctenodactylus             gundi
## 783        194.00      Massoutiera             mzabi
## 784        275.00         Ctenomys         torquatus
## 785        164.00         Ctenomys             haigi
## 786        490.00         Ctenomys          peruanus
## 787        100.00         Ctenomys           talarum
## 788        400.00         Ctenomys            opimus
## 789       2675.00       Dasyprocta          punctata
## 790       2650.00       Dasyprocta          cristata
## 791       3265.00       Dasyprocta          leporina
## 792        775.00        Myoprocta           acouchy
## 793      12500.00          Dinomys         branickii
## 794         52.00        Allactaga      tetradactyla
## 795         35.90        Allactaga        euphratica
## 796        117.50        Allactaga          sibirica
## 797         58.67        Allactaga            elater
## 798         87.00            Dipus           sagitta
## 799         51.00       Eremodipus     lichtensteini
## 800        134.00          Jaculus        orientalis
## 801         55.00          Jaculus           jaculus
## 802       -999.00          Jaculus       turcmenicus
## 803         21.33      Napaeozapus          insignis
## 804       -999.00       Pygeretmus         platyurus
## 805         52.33       Pygeretmus           pumilio
## 806       -999.00      Salpingotus       crassicauda
## 807       -999.00          Sicista            napaea
## 808          8.00          Sicista          betulina
## 809         55.00       Stylodipus             telum
## 810         28.75            Zapus          princeps
## 811         24.00            Zapus        trinotatus
## 812         18.25            Zapus         hudsonius
## 813        640.00          Echimys         chrysurus
## 814        504.00         Hoplomys          gymnurus
## 815        460.00    Kannabateomys         amblyonyx
## 816        276.00         Makalata            armata
## 817        356.50       Proechimys       cayennensis
## 818        500.00       Proechimys      semispinosus
## 819        350.00       Proechimys           guairae
## 820        373.50       Thrichomys        apereoides
## 821       3900.00          Coendou       prehensilis
## 822       9000.00        Erethizon          dorsatum
## 823       1750.00       Sphiggurus          villosus
## 824       2000.00       Sphiggurus         mexicanus
## 825        176.67           Geomys         bursarius
## 826        142.50           Geomys           pinetis
## 827        397.00           Geomys        personatus
## 828        210.00           Geomys         arenarius
## 829        650.00      Orthogeomys          hispidus
## 830        225.00      Orthogeomys          cherriei
## 831        266.00      Pappogeomys         castanops
## 832       -999.00         Thomomys           clusius
## 833         80.00         Thomomys         monticola
## 834        100.00         Thomomys          umbrinus
## 835         91.40         Thomomys         talpoides
## 836        359.90         Thomomys        bulbivorus
## 837        103.50         Thomomys            bottae
## 838        233.00         Thomomys        townsendii
## 839         14.40      Chaetodipus           nelsoni
## 840         15.23      Chaetodipus      penicillatus
## 841         25.50      Chaetodipus      californicus
## 842         16.50      Chaetodipus       intermedius
## 843         18.70      Chaetodipus            fallax
## 844         19.45      Chaetodipus          formosus
## 845         35.25      Chaetodipus          hispidus
## 846         85.00        Dipodomys      elephantinus
## 847         35.50        Dipodomys       nitratoides
## 848         72.40        Dipodomys      panamintinus
## 849         55.16        Dipodomys           microps
## 850         60.57        Dipodomys            agilis
## 851         64.50        Dipodomys         stephensi
## 852         82.00        Dipodomys          venustus
## 853         40.00        Dipodomys          merriami
## 854        119.60        Dipodomys       spectabilis
## 855         84.96        Dipodomys      californicus
## 856        132.00        Dipodomys            ingens
## 857         65.00        Dipodomys         heermanni
## 858         54.50        Dipodomys             ordii
## 859        112.00        Dipodomys           deserti
## 860         70.00        Heteromys          anomalus
## 861         81.00        Heteromys         oresterus
## 862         85.00        Heteromys          goldmani
## 863         77.00        Heteromys    desmarestianus
## 864         50.00           Liomys            pictus
## 865         42.50           Liomys         irroratus
## 866         65.00           Liomys         adspersus
## 867         41.00           Liomys           salvini
## 868         13.55    Microdipodops          pallidus
## 869         13.45    Microdipodops      megacephalus
## 870         11.55      Perognathus         fasciatus
## 871          9.00      Perognathus      longimembris
## 872          7.83      Perognathus            flavus
## 873          9.00      Perognathus        flavescens
## 874         20.10      Perognathus            parvus
## 875          8.00      Perognathus          merriami
## 876          8.65      Perognathus         inornatus
## 877      55000.00     Hydrochaeris      hydrochaeris
## 878       2000.00        Atherurus         macrourus
## 879       3180.00        Atherurus         africanus
## 880       2740.00          Hystrix            pumila
## 881       8000.00          Hystrix         brachyura
## 882      14650.00          Hystrix            indica
## 883      18333.33          Hystrix          cristata
## 884      15680.00          Hystrix  africaeaustralis
## 885       -999.00           Acomys         percivali
## 886         54.43           Acomys          russatus
## 887         20.00           Acomys           wilsoni
## 888         45.25           Acomys         cahirinus
## 889        107.00         Aethomys            hindei
## 890        198.00         Aethomys           kaiseri
## 891         78.55         Aethomys      chrysophilus
## 892         47.56         Aethomys       namaquensis
## 893         37.30           Akodon            varius
## 894         39.70           Akodon            cursor
## 895         25.95           Akodon       boliviensis
## 896         33.00           Akodon           molinae
## 897         50.00           Akodon           dolores
## 898         27.33           Akodon            azarae
## 899         37.55           Akodon        longipilis
## 900         28.03           Akodon         olivaceus
## 901         50.00           Akodon            urichi
## 902       -999.00   Allocricetulus        eversmanni
## 903         35.00         Alticola            roylei
## 904         50.00         Alticola         strelzowi
## 905         37.70         Alticola        argentatus
## 906       -999.00         Apodemus         argenteus
## 907         32.90         Apodemus        peninsulae
## 908       -999.00         Apodemus         uralensis
## 909         45.60         Apodemus        mystacinus
## 910         29.35         Apodemus       flavicollis
## 911         21.50         Apodemus          agrarius
## 912         23.38         Apodemus        sylvaticus
## 913         36.00        Arborimus              pomo
## 914         23.00        Arborimus       longicaudus
## 915         23.00        Arborimus           albipes
## 916         64.50      Arvicanthis         niloticus
## 917        187.50         Arvicola           sapidus
## 918        120.00         Arvicola        terrestris
## 919         72.60       Auliscomys          micropus
## 920          7.83          Baiomys           taylori
## 921        545.00        Bandicota            indica
## 922        166.67        Bandicota       bengalensis
## 923        101.67           Beamys            hindei
## 924         39.93          Bolomys          lasiurus
## 925         26.60          Calomys           lepidus
## 926         34.00          Calomys            laucha
## 927         27.00          Calomys       hummelincki
## 928         41.00          Calomys          callosus
## 929         17.60          Calomys        musculinus
## 930         20.40       Calomyscus         bailwardi
## 931         20.40       Calomyscus            mystax
## 932        650.00         Cannomys            badius
## 933         54.00        Chionomys           nivalis
## 934       -999.00        Chionomys               gud
## 935         25.50     Chiropodomys         gliroides
## 936         43.40       Chiruromys             vates
## 937         20.57    Clethrionomys           gapperi
## 938         29.50    Clethrionomys         rufocanus
## 939         20.77    Clethrionomys         glareolus
## 940         23.60    Clethrionomys      californicus
## 941         25.00    Clethrionomys           rutilus
## 942         58.50          Colomys          goslingi
## 943        144.00        Conilurus      penicillatus
## 944         14.10        Cremnomys         blanfordi
## 945         59.80        Cremnomys         cutchicus
## 946       1092.60       Cricetomys         gambianus
## 947       -999.00       Cricetulus        barabensis
## 948         30.80       Cricetulus       migratorius
## 949        506.67         Cricetus          cricetus
## 950        106.18          Dasymys          incomtus
## 951       -999.00        Dendromus         messorius
## 952       -999.00        Dendromus       kahuziensis
## 953          7.20        Dendromus        mystacalis
## 954         10.60        Dendromus         mesomelas
## 955          7.36        Dendromus         melanotis
## 956         45.00         Dephomys             defua
## 957         10.00   Desmodilliscus           braueri
## 958         54.50      Desmodillus       auricularis
## 959       -999.00      Dicrostonyx      unalascensis
## 960         55.00      Dicrostonyx       richardsoni
## 961         57.00      Dicrostonyx         hudsonius
## 962         85.00      Dicrostonyx         torquatus
## 963         55.05      Dicrostonyx     groenlandicus
## 964         56.00        Dinaromys         bogdanovi
## 965         21.40     Eligmodontia             typus
## 966         80.00         Ellobius     fuscocapillus
## 967         38.00         Ellobius          talpinus
## 968         26.00        Eolagurus            luteus
## 969         38.60      Gerbillurus           setzeri
## 970         28.50      Gerbillurus             paeba
## 971         24.00      Gerbillurus           tytonis
## 972         26.10        Gerbillus          gleadowi
## 973         22.00        Gerbillus         andersoni
## 974         11.70        Gerbillus           henleyi
## 975         25.00        Gerbillus         gerbillus
## 976         24.40        Gerbillus          dasyurus
## 977       -999.00        Gerbillus       perpallidus
## 978       -999.00        Gerbillus            simoni
## 979         43.35        Gerbillus         cheesmani
## 980         18.50        Gerbillus             nanus
## 981         37.00        Gerbillus         pyramidum
## 982         65.00          Golunda           ellioti
## 983         53.00        Grammomys          rutilans
## 984         43.70        Grammomys        dolichurus
## 985       -999.00        Grammomys           cometes
## 986         66.00          Graomys      griseoflavus
## 987        368.00          Hodomys            alleni
## 988        155.00       Holochilus      brasiliensis
## 989         51.80          Hybomys       univittatus
## 990        606.00         Hydromys      chrysogaster
## 991         20.00       Hylomyscus            alleni
## 992         16.70       Hylomyscus            stella
## 993        882.00           Hyomys           goliath
## 994       -999.00      Hyperacrius          fertilis
## 995         52.60      Hyperacrius            wynnei
## 996       1250.00       Hypogeomys          antimena
## 997         20.30          Lagurus           lagurus
## 998       -999.00     Lasiopodomys          brandtii
## 999         17.50        Leggadina     lakedownensis
## 1000        20.00        Leggadina          forresti
## 1001        27.17        Lemmiscus          curtatus
## 1002        51.67           Lemmus            lemmus
## 1003        59.20           Lemmus         sibiricus
## 1004        59.00      Lemniscomys          griselda
## 1005        42.30      Lemniscomys          striatus
## 1006       332.50       Leporillus          conditor
## 1007       755.00        Lophiomys           imhausi
## 1008      -999.00       Lophuromys          woosnami
## 1009      -999.00       Lophuromys       luteogaster
## 1010        55.00       Lophuromys    flavopunctatus
## 1011        44.50       Lophuromys          sikapusi
## 1012      -999.00    Macrotarsomys          bastardi
## 1013        12.33      Malacothrix            typica
## 1014        37.77         Mastomys     erythroleucus
## 1015        44.97         Mastomys        natalensis
## 1016       111.00     Megadontomys           thomasi
## 1017        81.60          Melomys           levipes
## 1018        45.90          Melomys             rubex
## 1019        57.40          Melomys         rufescens
## 1020        89.80          Melomys         moncktoni
## 1021        96.00          Melomys       leucogaster
## 1022        65.00          Melomys          capensis
## 1023        62.00          Melomys           burtoni
## 1024        94.00          Melomys        cervinipes
## 1025       100.00         Meriones          persicus
## 1026       105.00         Meriones         tristrami
## 1027        77.00         Meriones           libycus
## 1028       185.00         Meriones             shawi
## 1029       150.00         Meriones       vinogradovi
## 1030        48.30         Meriones           crassus
## 1031      -999.00         Meriones      tamariscinus
## 1032        53.00         Meriones        meridianus
## 1033        70.00         Meriones         hurrianae
## 1034        53.20         Meriones      unguiculatus
## 1035       268.50     Mesembriomys          macrurus
## 1036       900.00     Mesembriomys           gouldii
## 1037       197.50     Mesocricetus           brandti
## 1038      -999.00     Mesocricetus            raddei
## 1039       105.00     Mesocricetus           auratus
## 1040         6.00         Micromys           minutus
## 1041      -999.00         Microtus       lusitanicus
## 1042      -999.00         Microtus     transcaspicus
## 1043      -999.00         Microtus  duodecimcostatus
## 1044        50.00         Microtus       abbreviatus
## 1045        35.00         Microtus         mexicanus
## 1046      -999.00         Microtus         juldaschi
## 1047        54.00         Microtus           breweri
## 1048        39.85         Microtus        montebelli
## 1049        54.80         Microtus        townsendii
## 1050        39.50         Microtus            miurus
## 1051        36.70         Microtus       longicaudus
## 1052        39.00         Microtus     chrotorrhinus
## 1053        35.30         Microtus        canicaudus
## 1054       106.20         Microtus       richardsoni
## 1055       141.50         Microtus     xanthognathus
## 1056        43.67         Microtus    pennsylvanicus
## 1057        25.50         Microtus         pinetorum
## 1058        47.50         Microtus          gregalis
## 1059        51.60         Microtus         guentheri
## 1060        93.33         Microtus         oeconomus
## 1061        46.00         Microtus          agrestis
## 1062        19.50         Microtus      subterraneus
## 1063        40.00         Microtus       ochrogaster
## 1064        20.50         Microtus           oregoni
## 1065        49.50         Microtus          montanus
## 1066        53.50         Microtus      californicus
## 1067        48.00         Microtus          socialis
## 1068        27.50         Microtus           arvalis
## 1069        63.00         Microtus            fortis
## 1070        70.00        Millardia           meltada
## 1071        11.25              Mus            caroli
## 1072        12.80              Mus           spretus
## 1073      -999.00              Mus            mayori
## 1074        17.70              Mus        cervicolor
## 1075         6.00              Mus       musculoides
## 1076        28.20              Mus        platythrix
## 1077         9.22              Mus           booduga
## 1078        12.45              Mus            triton
## 1079         7.25              Mus        minutoides
## 1080        20.50              Mus          musculus
## 1081        30.00           Myomys           fumatus
## 1082        32.00           Myomys            derooi
## 1083        33.50           Myomys           daltoni
## 1084        25.00           Myopus      schisticolor
## 1085      -999.00        Myospalax         myospalax
## 1086       106.00        Mystromys      albicaudatus
## 1087       178.00      Nannospalax          leucodon
## 1088        19.00         Neacomys          tenuipes
## 1089       290.00         Nectomys         squamipes
## 1090       257.50         Nectomys          parvipes
## 1091       269.00         Neofiber            alleni
## 1092        96.70          Neotoma             devia
## 1093       233.58          Neotoma          fuscipes
## 1094       142.00          Neotoma         stephensi
## 1095       335.50          Neotoma           cinerea
## 1096       227.50          Neotoma            phenax
## 1097       203.00          Neotoma          mexicana
## 1098       198.00          Neotoma          albigula
## 1099       125.33          Neotoma            lepida
## 1100       236.33          Neotoma          micropus
## 1101       370.50          Neotoma         floridana
## 1102        50.00       Neotomodon           alstoni
## 1103       143.50          Nesokia            indica
## 1104        66.40       Niviventer        niviventer
## 1105        35.00          Notomys          cervinus
## 1106        35.00          Notomys            fuscus
## 1107        39.00          Notomys            aquilo
## 1108        52.00          Notomys        mitchellii
## 1109        35.00          Notomys            alexis
## 1110        53.33         Nyctomys       sumichrasti
## 1111        22.00       Ochrotomys          nuttalli
## 1112        60.67          Oecomys          concolor
## 1113        85.50          Oenomys       hypoxanthus
## 1114        22.65     Oligoryzomys          nigripes
## 1115        21.00     Oligoryzomys        flavescens
## 1116        27.75     Oligoryzomys     longicaudatus
## 1117      1135.80          Ondatra        zibethicus
## 1118        40.00        Onychomys       leucogaster
## 1119        19.00        Onychomys          torridus
## 1120         7.50         Oryzomys         subflavus
## 1121        47.67         Oryzomys         palustris
## 1122        63.15         Oryzomys            capito
## 1123       121.00           Otomys         sloggetti
## 1124      -999.00           Otomys             typus
## 1125      -999.00           Otomys             denti
## 1126       150.95           Otomys       angoniensis
## 1127       146.85           Otomys         irroratus
## 1128       101.00       Ototylomys         phyllotis
## 1129        85.50      Oxymycterus             rufus
## 1130        32.50      Pachyuromys           duprasi
## 1131       120.00        Parotomys          brantsii
## 1132        50.00          Pelomys             minor
## 1133       118.00          Pelomys            fallax
## 1134        36.00       Peromyscus           hooperi
## 1135        71.00       Peromyscus          megalops
## 1136        59.00       Peromyscus      melanocarpus
## 1137        60.50       Peromyscus         mexicanus
## 1138      -999.00       Peromyscus   interparietalis
## 1139        40.00       Peromyscus         perfulvus
## 1140        40.00       Peromyscus       melanophrys
## 1141        31.50       Peromyscus        pectoralis
## 1142        26.90       Peromyscus            gratus
## 1143        28.70       Peromyscus            boylii
## 1144        26.30       Peromyscus       yucatanicus
## 1145        29.00       Peromyscus        gossypinus
## 1146        28.00       Peromyscus        difficilis
## 1147        30.00       Peromyscus         attwateri
## 1148        28.00       Peromyscus           nasutus
## 1149        28.30       Peromyscus         sitkensis
## 1150        16.33       Peromyscus          crinitus
## 1151        14.00       Peromyscus        polionotus
## 1152        27.00       Peromyscus             truei
## 1153        20.50       Peromyscus       maniculatus
## 1154        25.00       Peromyscus          eremicus
## 1155        23.00       Peromyscus          leucopus
## 1156        42.00       Peromyscus      californicus
## 1157        39.60       Peromyscus         melanotis
## 1158        21.00      Petromyscus          collinus
## 1159        27.00       Phenacomys            ungava
## 1160        33.00       Phenacomys       intermedius
## 1161        23.40         Phodopus         campbelli
## 1162        23.40         Phodopus          sungorus
## 1163      -999.00         Phodopus       roborovskii
## 1164        50.83        Phyllotis           darwini
## 1165       104.00       Pithecheir            parvus
## 1166        31.25          Podomys        floridanus
## 1167        60.90    Pogonomelomys             sevia
## 1168        39.10        Pogonomys        sylvestris
## 1169        95.00        Pogonomys            loriae
## 1170        45.10        Pogonomys         macrourus
## 1171        40.00          Praomys          jacksoni
## 1172        21.00          Praomys             morio
## 1173        36.97          Praomys         tullbergi
## 1174        27.00          Praomys        delectorum
## 1175       177.33        Psammomys            obesus
## 1176       148.00        Pseudomys            fuscus
## 1177        95.00        Pseudomys            oralis
## 1178        12.00        Pseudomys hermannsburgensis
## 1179        11.00        Pseudomys      pilligaensis
## 1180        11.00        Pseudomys       delicatulus
## 1181        63.00        Pseudomys   gracilicaudatus
## 1182        25.00        Pseudomys          desertor
## 1183        34.00        Pseudomys             nanus
## 1184        65.00        Pseudomys         australis
## 1185        45.00        Pseudomys            fieldi
## 1186        45.00        Pseudomys         praeconis
## 1187        64.00        Pseudomys       shortridgei
## 1188        26.00        Pseudomys      albocinereus
## 1189        60.10        Pseudomys          higginsi
## 1190        70.00        Pseudomys            fumeus
## 1191        20.00        Pseudomys       apodemoides
## 1192        18.00        Pseudomys   novaehollandiae
## 1193        61.00           Rattus          colletti
## 1194       200.00           Rattus     villosissimus
## 1195       127.50           Rattus        tiomanicus
## 1196       124.00           Rattus          leucopus
## 1197       155.00           Rattus            steini
## 1198       139.35           Rattus           praetor
## 1199       113.50           Rattus           tunneyi
## 1200       125.00           Rattus          sordidus
## 1201       202.00           Rattus     argentiventer
## 1202       115.00           Rattus         lutreolus
## 1203       133.00           Rattus          fuscipes
## 1204        70.00           Rattus           exulans
## 1205       280.00           Rattus        norvegicus
## 1206       246.50           Rattus            rattus
## 1207        72.17       Reithrodon           auritus
## 1208        10.30  Reithrodontomys           humulis
## 1209        11.03  Reithrodontomys         megalotis
## 1210         9.00  Reithrodontomys          montanus
## 1211        11.05  Reithrodontomys       raviventris
## 1212        12.50  Reithrodontomys        fulvescens
## 1213        51.00        Rhabdomys           pumilio
## 1214        57.50       Rhipidomys         latimanus
## 1215        75.00       Rhipidomys        mastacalis
## 1216      3000.00         Rhizomys       sumatrensis
## 1217      2450.00         Rhizomys         pruinosus
## 1218       143.00        Rhombomys            opimus
## 1219        47.38      Saccostomus        campestris
## 1220        12.00       Scotinomys           teguina
## 1221        15.00       Scotinomys      xerampelinus
## 1222        38.50       Sekeetamys           calurus
## 1223       135.50         Sigmodon          leucotis
## 1224       113.67         Sigmodon      ochrognathus
## 1225       211.00         Sigmodon       fulviventer
## 1226       185.00         Sigmodon          hispidus
## 1227        55.70         Sigmodon           alstoni
## 1228      -999.00           Spalax    microphthalmus
## 1229        24.00        Steatomys           krebsii
## 1230        25.53        Steatomys         pratensis
## 1231        74.50        Stochomys     longicaudatus
## 1232       334.00         Sundamys          muelleri
## 1233        37.05       Synaptomys           cooperi
## 1234        21.30       Synaptomys          borealis
## 1235      -999.00     Tachyoryctes           ruandae
## 1236       207.50     Tachyoryctes         splendens
## 1237        84.00         Tateomys    rhinogradoides
## 1238       131.00           Tatera           inclusa
## 1239        64.86           Tatera       leucogaster
## 1240        93.00           Tatera           robusta
## 1241       113.00           Tatera            valida
## 1242        75.00           Tatera          brantsii
## 1243       146.67           Tatera            indica
## 1244       114.00           Tatera        nigricauda
## 1245        74.80           Tatera              afra
## 1246      -999.00       Taterillus             emini
## 1247        48.10       Taterillus          pygargus
## 1248        51.85       Taterillus          gracilis
## 1249        77.43        Thallomys         paedulcus
## 1250        65.00        Thamnomys          venustus
## 1251       280.00          Tylomys        nudicaudus
## 1252        47.00         Uranomys             ruddi
## 1253       625.00           Uromys    caudimaculatus
## 1254        56.60      Vandeleuria         nolthenii
## 1255        47.00         Wiedomys      pyrrhorhinos
## 1256        54.50        Zelotomys          woosnami
## 1257        58.00     Zygodontomys        brevicauda
## 1258        94.00          Zyzomys             maini
## 1259        50.00          Zyzomys           argurus
## 1260       130.00          Zyzomys         woodwardi
## 1261      7150.00        Myocastor            coypus
## 1262      -999.00          Dryomys           laniger
## 1263        25.00          Dryomys          nitedula
## 1264       115.00          Eliomys         quercinus
## 1265        27.00         Glirulus         japonicus
## 1266      -999.00       Graphiurus    crassicaudatus
## 1267        24.00       Graphiurus           murinus
## 1268        68.80       Graphiurus          ocularis
## 1269        27.33      Muscardinus      avellanarius
## 1270       125.00           Myoxus              glis
## 1271       235.00          Octodon             degus
## 1272       158.00     Octodontomys         gliroides
## 1273        98.05       Spalacopus            cyanus
## 1274      3500.00          Pedetes          capensis
## 1275       200.00         Petromus           typicus
## 1276       126.00 Ammospermophilus          harrisii
## 1277       154.50 Ammospermophilus           nelsoni
## 1278       110.20 Ammospermophilus         interpres
## 1279        97.50 Ammospermophilus          leucurus
## 1280       400.00     Callosciurus         prevostii
## 1281        30.00     Callosciurus          caniceps
## 1282       202.00     Callosciurus     nigrovittatus
## 1283       190.00     Callosciurus           notatus
## 1284       863.00          Cynomys      ludovicianus
## 1285       816.67          Cynomys         gunnisoni
## 1286       516.00          Cynomys         parvidens
## 1287       873.50          Cynomys          leucurus
## 1288       828.10          Cynomys         mexicanus
## 1289       172.50         Dremomys           lokriah
## 1290       388.00         Epixerus              ebii
## 1291        14.60     Exilisciurus            exilis
## 1292      -999.00       Funambulus       sublineatus
## 1293      -999.00       Funambulus          palmarum
## 1294       147.65       Funambulus         pennantii
## 1295       218.00      Funisciurus        anerythrus
## 1296       141.00      Funisciurus       lemniscatus
## 1297       271.67      Funisciurus          pyrropus
## 1298       106.00      Funisciurus          congicus
## 1299       132.17        Glaucomys          sabrinus
## 1300        65.38        Glaucomys            volans
## 1301       326.50     Heliosciurus      rufobrachium
## 1302       240.00        Hylopetes         alboniger
## 1303       510.00        Hylopetes        fimbriatus
## 1304       120.00            Iomys        horsfieldi
## 1305      -999.00          Marmota         menzbieri
## 1306      -999.00          Marmota      camtschatica
## 1307      4750.00          Marmota    vancouverensis
## 1308      6300.00          Marmota           olympus
## 1309      6343.33          Marmota          caligata
## 1310      -999.00          Marmota             bobak
## 1311      2930.00          Marmota      flaviventris
## 1312      3180.00          Marmota           broweri
## 1313      2010.00          Marmota           marmota
## 1314      8000.00          Marmota          sibirica
## 1315      3413.00          Marmota             monax
## 1316      4100.00          Marmota           caudata
## 1317      -999.00          Marmota         baibacina
## 1318        16.50       Myosciurus           pumilio
## 1319      -999.00        Paraxerus       flavovittis
## 1320       134.00        Paraxerus           poensis
## 1321      -999.00        Paraxerus         ochraceus
## 1322       295.00        Paraxerus         palliatus
## 1323       216.00        Paraxerus            cepapi
## 1324      -999.00       Petaurista        leucogenys
## 1325      1273.33       Petaurista           elegans
## 1326      1780.00       Petaurista        magnificus
## 1327      -999.00       Petaurista      philippensis
## 1328      1330.00       Petaurista        petaurista
## 1329       110.00        Petinomys        genibarbis
## 1330        42.25        Petinomys           setosus
## 1331        37.00        Petinomys       vordermanni
## 1332       712.00        Petinomys     fuscocapillus
## 1333       130.00         Pteromys            volans
## 1334      1060.00           Ratufa            indica
## 1335      1280.00           Ratufa          macroura
## 1336      2150.00           Ratufa           bicolor
## 1337       221.00     Rhinosciurus      laticaudatus
## 1338        39.00       Sciurillus          pusillus
## 1339       595.00          Sciurus       aureogaster
## 1340       225.00          Sciurus      yucatanensis
## 1341       461.00          Sciurus            alleni
## 1342       760.50          Sciurus       arizonensis
## 1343       751.67          Sciurus           griseus
## 1344       702.50          Sciurus            aberti
## 1345       803.70          Sciurus             niger
## 1346       540.33          Sciurus      carolinensis
## 1347       374.00          Sciurus       granatensis
## 1348       324.75          Sciurus          vulgaris
## 1349      -999.00  Spermophilopsis     leptodactylus
## 1350       600.00     Spermophilus          relictus
## 1351       751.00     Spermophilus           parryii
## 1352       190.00     Spermophilus        mohavensis
## 1353       200.00     Spermophilus          dauricus
## 1354       157.60     Spermophilus         lateralis
## 1355       278.50     Spermophilus          beldingi
## 1356       308.00     Spermophilus           armatus
## 1357      -999.00     Spermophilus             canus
## 1358      -999.00     Spermophilus      erythrogenys
## 1359       136.00     Spermophilus          pygmaeus
## 1360      -999.00     Spermophilus             major
## 1361       165.40     Spermophilus            mollis
## 1362       707.00     Spermophilus      nayaritensis
## 1363       466.33     Spermophilus       columbianus
## 1364       275.00     Spermophilus         saturatus
## 1365       840.00     Spermophilus         undulatus
## 1366       130.95     Spermophilus         mexicanus
## 1367       401.05     Spermophilus           elegans
## 1368       163.33     Spermophilus      tereticaudus
## 1369       205.00     Spermophilus          brunneus
## 1370       609.33     Spermophilus          beecheyi
## 1371       596.00     Spermophilus            fulvus
## 1372      -999.00     Spermophilus          suslicus
## 1373       342.50     Spermophilus      richardsonii
## 1374       459.00     Spermophilus        franklinii
## 1375       155.00     Spermophilus        townsendii
## 1376       203.00     Spermophilus       washingtoni
## 1377       172.67     Spermophilus  tridecemlineatus
## 1378       217.00     Spermophilus          citellus
## 1379       663.03     Spermophilus        variegatus
## 1380       119.50     Spermophilus         spilosoma
## 1381        67.00     Sundasciurus             lowii
## 1382        75.00     Sundasciurus            tenuis
## 1383        94.10           Tamias        ochrogenys
## 1384        59.70           Tamias           palmeri
## 1385        75.00           Tamias          siskiyou
## 1386        70.00           Tamias           canipes
## 1387        75.00           Tamias           sonomae
## 1388        57.29           Tamias        ruficaudus
## 1389        54.15           Tamias      panamintinus
## 1390        60.00           Tamias         speciosus
## 1391        84.00           Tamias        townsendii
## 1392        94.81           Tamias             senex
## 1393        44.15           Tamias           minimus
## 1394        62.10           Tamias     cinereicollis
## 1395        88.45           Tamias   quadrimaculatus
## 1396        48.25           Tamias           amoenus
## 1397        85.00           Tamias         sibiricus
## 1398        58.13           Tamias    quadrivittatus
## 1399        62.80           Tamias          dorsalis
## 1400        95.70           Tamias          striatus
## 1401       229.80     Tamiasciurus         douglasii
## 1402       194.50     Tamiasciurus        hudsonicus
## 1403      -999.00      Trogopterus         xanthipes
## 1404       502.00            Xerus        erythropus
## 1405       588.00            Xerus           inauris
## 1406      1880.00       Thryonomys       gregorianus
## 1407      3687.50       Thryonomys      swinderianus
## 1408       283.00           Tupaia           montana
## 1409       700.00           Tupaia             minor
## 1410       797.00           Tupaia              tana
## 1411       159.00           Tupaia              glis
## 1412      -999.00           Tupaia         belangeri
## 1413       350.00          Urogale          everetti
## 1414        42.50      Ptilocercus             lowii
## 1415    479500.00           Dugong             dugon
## 1416   4000000.00     Hydrodamalis             gigas
## 1417    500000.00       Trichechus      senegalensis
## 1418    387500.00       Trichechus           manatus
## 1419    480000.00       Trichechus          inunguis
## 1420     60000.00      Orycteropus              afer
## 1421      3900.00         Bradypus         torquatus
## 1422      4860.00         Bradypus        variegatus
## 1423      3770.00         Bradypus       tridactylus
## 1424      2750.00        Cabassous         centralis
## 1425      2150.00   Chaetophractus           nationi
## 1426      2020.00   Chaetophractus          villosus
## 1427     10150.00          Dasypus          kappleri
## 1428      1150.00          Dasypus        sabanicola
## 1429      1500.00          Dasypus          hybridus
## 1430      1475.00          Dasypus     septemcinctus
## 1431      4300.00          Dasypus      novemcinctus
## 1432      4600.00       Euphractus        sexcinctus
## 1433     50000.00       Priodontes           maximus
## 1434      1225.00       Tolypeutes           matacus
## 1435      1500.00          Zaedyus            pichiy
## 1436      4750.00        Choloepus         hoffmanni
## 1437      6070.00        Choloepus        didactylus
## 1438      5070.00         Cyclopes        didactylus
## 1439     28500.00     Myrmecophaga        tridactyla
## 1440      5030.00         Tamandua      tetradactyla
```

6. What if we only wanted to exclude `order` and `family`? Use the `-` operator to make the code efficient.

```r
select(mammals, -order, -family)
```

```
## # A tibble: 1,440 × 11
##    Genus species   mass gestation newborn weaning `wean mass`    AFR `max. life`
##    <chr> <chr>    <dbl>     <dbl>   <dbl>   <dbl>       <dbl>  <dbl>       <dbl>
##  1 Anti… americ… 4.54e4      8.13   3246.    3           8900   13.5         142
##  2 Addax nasoma… 1.82e5      9.39   5480     6.5         -999   27.3         308
##  3 Aepy… melamp… 4.15e4      6.35   5093     5.63       15900   16.7         213
##  4 Alce… busela… 1.5 e5      7.9   10167.    6.5         -999   23.0         240
##  5 Ammo… clarkei 2.85e4      6.8    -999  -999           -999 -999          -999
##  6 Ammo… lervia  5.55e4      5.08   3810     4           -999   14.9         251
##  7 Anti… marsup… 3   e4      5.72   3910     4.04        -999   10.2         228
##  8 Anti… cervic… 3.75e4      5.5    3846     2.13        -999   20.1         255
##  9 Bison bison   4.98e5      8.93  20000    10.7       157500   29.4         300
## 10 Bison bonasus 5   e5      9.14  23000.    6.6         -999   30.0         324
## # … with 1,430 more rows, and 2 more variables: litter size <dbl>,
## #   litters/year <dbl>
```

7. Select the columns that include "mass" as part of the name.  

```r
select(mammals, contains("mass"))
```

```
## # A tibble: 1,440 × 2
##       mass `wean mass`
##      <dbl>       <dbl>
##  1  45375         8900
##  2 182375         -999
##  3  41480        15900
##  4 150000         -999
##  5  28500         -999
##  6  55500         -999
##  7  30000         -999
##  8  37500         -999
##  9 497667.      157500
## 10 500000         -999
## # … with 1,430 more rows
```

8. Select all of the columns that are of class `character`.  

```r
select_if(mammals, is.character)
```

```
## # A tibble: 1,440 × 4
##    order        family         Genus       species      
##    <chr>        <chr>          <chr>       <chr>        
##  1 Artiodactyla Antilocapridae Antilocapra americana    
##  2 Artiodactyla Bovidae        Addax       nasomaculatus
##  3 Artiodactyla Bovidae        Aepyceros   melampus     
##  4 Artiodactyla Bovidae        Alcelaphus  buselaphus   
##  5 Artiodactyla Bovidae        Ammodorcas  clarkei      
##  6 Artiodactyla Bovidae        Ammotragus  lervia       
##  7 Artiodactyla Bovidae        Antidorcas  marsupialis  
##  8 Artiodactyla Bovidae        Antilope    cervicapra   
##  9 Artiodactyla Bovidae        Bison       bison        
## 10 Artiodactyla Bovidae        Bison       bonasus      
## # … with 1,430 more rows
```

## Other
Here are two examples of code that are super helpful to have in your bag of tricks.  

Imported data frames often have a mix of lower and uppercase column names. Use `toupper()` or `tolower()` to fix this issue. I always try to use lowercase to keep things consistent.  

```r
mammals <- select_all(mammals, tolower)
mammals
```

```
## # A tibble: 1,440 × 13
##    order   family   genus  species    mass gestation newborn weaning `wean mass`
##    <chr>   <chr>    <chr>  <chr>     <dbl>     <dbl>   <dbl>   <dbl>       <dbl>
##  1 Artiod… Antiloc… Antil… america… 4.54e4      8.13   3246.    3           8900
##  2 Artiod… Bovidae  Addax  nasomac… 1.82e5      9.39   5480     6.5         -999
##  3 Artiod… Bovidae  Aepyc… melampus 4.15e4      6.35   5093     5.63       15900
##  4 Artiod… Bovidae  Alcel… buselap… 1.5 e5      7.9   10167.    6.5         -999
##  5 Artiod… Bovidae  Ammod… clarkei  2.85e4      6.8    -999  -999           -999
##  6 Artiod… Bovidae  Ammot… lervia   5.55e4      5.08   3810     4           -999
##  7 Artiod… Bovidae  Antid… marsupi… 3   e4      5.72   3910     4.04        -999
##  8 Artiod… Bovidae  Antil… cervica… 3.75e4      5.5    3846     2.13        -999
##  9 Artiod… Bovidae  Bison  bison    4.98e5      8.93  20000    10.7       157500
## 10 Artiod… Bovidae  Bison  bonasus  5   e5      9.14  23000.    6.6         -999
## # … with 1,430 more rows, and 4 more variables: afr <dbl>, max. life <dbl>,
## #   litter size <dbl>, litters/year <dbl>
```

When naming columns, blank spaces are often added (don't do this, please). Here is a trick to remove these.  

```r
select_all(mammals, ~str_replace(., " ", "_"))
```

```
## # A tibble: 1,440 × 13
##    order  family genus species   mass gestation newborn weaning wean_mass    afr
##    <chr>  <chr>  <chr> <chr>    <dbl>     <dbl>   <dbl>   <dbl>     <dbl>  <dbl>
##  1 Artio… Antil… Anti… americ… 4.54e4      8.13   3246.    3         8900   13.5
##  2 Artio… Bovid… Addax nasoma… 1.82e5      9.39   5480     6.5       -999   27.3
##  3 Artio… Bovid… Aepy… melamp… 4.15e4      6.35   5093     5.63     15900   16.7
##  4 Artio… Bovid… Alce… busela… 1.5 e5      7.9   10167.    6.5       -999   23.0
##  5 Artio… Bovid… Ammo… clarkei 2.85e4      6.8    -999  -999         -999 -999  
##  6 Artio… Bovid… Ammo… lervia  5.55e4      5.08   3810     4         -999   14.9
##  7 Artio… Bovid… Anti… marsup… 3   e4      5.72   3910     4.04      -999   10.2
##  8 Artio… Bovid… Anti… cervic… 3.75e4      5.5    3846     2.13      -999   20.1
##  9 Artio… Bovid… Bison bison   4.98e5      8.93  20000    10.7     157500   29.4
## 10 Artio… Bovid… Bison bonasus 5   e5      9.14  23000.    6.6       -999   30.0
## # … with 1,430 more rows, and 3 more variables: max._life <dbl>,
## #   litter_size <dbl>, litters/year <dbl>
```

## That's it! Take a break and I will see you on Zoom!  

-->[Home](https://jmledford3115.github.io/datascibiol/)  
