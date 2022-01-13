---
title: "Lab 3 Homework"
author: "John Abram Flores"
date: "2022-01-12"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---

## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the tidyverse

```r
library(tidyverse)
```

## Mammals Sleep
1. For this assignment, we are going to use built-in data on mammal sleep patterns. From which publication are these data taken from? Since the data are built-in you can use the help function in R.

```r
?msleep
msleep
```

```
## # A tibble: 83 × 11
##    name   genus vore  order conservation sleep_total sleep_rem sleep_cycle awake
##    <chr>  <chr> <chr> <chr> <chr>              <dbl>     <dbl>       <dbl> <dbl>
##  1 Cheet… Acin… carni Carn… lc                  12.1      NA        NA      11.9
##  2 Owl m… Aotus omni  Prim… <NA>                17         1.8      NA       7  
##  3 Mount… Aplo… herbi Rode… nt                  14.4       2.4      NA       9.6
##  4 Great… Blar… omni  Sori… lc                  14.9       2.3       0.133   9.1
##  5 Cow    Bos   herbi Arti… domesticated         4         0.7       0.667  20  
##  6 Three… Brad… herbi Pilo… <NA>                14.4       2.2       0.767   9.6
##  7 North… Call… carni Carn… vu                   8.7       1.4       0.383  15.3
##  8 Vespe… Calo… <NA>  Rode… <NA>                 7        NA        NA      17  
##  9 Dog    Canis carni Carn… domesticated        10.1       2.9       0.333  13.9
## 10 Roe d… Capr… herbi Arti… lc                   3        NA        NA      21  
## # … with 73 more rows, and 2 more variables: brainwt <dbl>, bodywt <dbl>
```

2. Store these data into a new data frame `sleep`.

```r
sleep <- data.frame(msleep)
```

3. What are the dimensions of this data frame (variables and observations)? How do you know? Please show the *code* that you used to determine this below.  

*The dimensions of this data frame are 83 mammals by 11 observations/variables*


```r
glimpse(sleep)
```

```
## Rows: 83
## Columns: 11
## $ name         <chr> "Cheetah", "Owl monkey", "Mountain beaver", "Greater shor…
## $ genus        <chr> "Acinonyx", "Aotus", "Aplodontia", "Blarina", "Bos", "Bra…
## $ vore         <chr> "carni", "omni", "herbi", "omni", "herbi", "herbi", "carn…
## $ order        <chr> "Carnivora", "Primates", "Rodentia", "Soricomorpha", "Art…
## $ conservation <chr> "lc", NA, "nt", "lc", "domesticated", NA, "vu", NA, "dome…
## $ sleep_total  <dbl> 12.1, 17.0, 14.4, 14.9, 4.0, 14.4, 8.7, 7.0, 10.1, 3.0, 5…
## $ sleep_rem    <dbl> NA, 1.8, 2.4, 2.3, 0.7, 2.2, 1.4, NA, 2.9, NA, 0.6, 0.8, …
## $ sleep_cycle  <dbl> NA, NA, NA, 0.1333333, 0.6666667, 0.7666667, 0.3833333, N…
## $ awake        <dbl> 11.9, 7.0, 9.6, 9.1, 20.0, 9.6, 15.3, 17.0, 13.9, 21.0, 1…
## $ brainwt      <dbl> NA, 0.01550, NA, 0.00029, 0.42300, NA, NA, NA, 0.07000, 0…
## $ bodywt       <dbl> 50.000, 0.480, 1.350, 0.019, 600.000, 3.850, 20.490, 0.04…
```

```r
dim(sleep)
```

```
## [1] 83 11
```

4. Are there any NAs in the data? How did you determine this? Please show your code.  

*There are a few NAs in the data*

```r
anyNA(sleep)
```

```
## [1] TRUE
```

```r
# is.na(sleep) <- used this command to pinpoint specifically which values were NA
```

5. Show a list of the column names is this data frame.

```r
names(sleep)
```

```
##  [1] "name"         "genus"        "vore"         "order"        "conservation"
##  [6] "sleep_total"  "sleep_rem"    "sleep_cycle"  "awake"        "brainwt"     
## [11] "bodywt"
```

6. How many herbivores are represented in the data?  

*There are 32 rows of entries, so there are 32 herbivores represented in the data*

```r
Herbivores <- subset(sleep, vore=="herbi")
Herbivores
```

```
##                              name        genus  vore          order
## 3                 Mountain beaver   Aplodontia herbi       Rodentia
## 5                             Cow          Bos herbi   Artiodactyla
## 6                Three-toed sloth     Bradypus herbi         Pilosa
## 10                       Roe deer    Capreolus herbi   Artiodactyla
## 11                           Goat        Capri herbi   Artiodactyla
## 12                     Guinea pig        Cavis herbi       Rodentia
## 14                     Chinchilla   Chinchilla herbi       Rodentia
## 19                     Tree hyrax  Dendrohyrax herbi     Hyracoidea
## 21                 Asian elephant      Elephas herbi    Proboscidea
## 23                          Horse        Equus herbi Perissodactyla
## 24                         Donkey        Equus herbi Perissodactyla
## 27      Western american chipmunk     Eutamias herbi       Rodentia
## 30                        Giraffe      Giraffa herbi   Artiodactyla
## 33                     Gray hyrax  Heterohyrax herbi     Hyracoidea
## 35                 Mongoose lemur        Lemur herbi       Primates
## 36               African elephant    Loxodonta herbi    Proboscidea
## 39               Mongolian gerbil     Meriones herbi       Rodentia
## 40                 Golden hamster Mesocricetus herbi       Rodentia
## 41                          Vole      Microtus herbi       Rodentia
## 42                    House mouse          Mus herbi       Rodentia
## 44           Round-tailed muskrat     Neofiber herbi       Rodentia
## 46                           Degu      Octodon herbi       Rodentia
## 48                         Rabbit  Oryctolagus herbi     Lagomorpha
## 49                          Sheep         Ovis herbi   Artiodactyla
## 61                        Potoroo     Potorous herbi  Diprotodontia
## 64                 Laboratory rat       Rattus herbi       Rodentia
## 68                     Cotton rat     Sigmodon herbi       Rodentia
## 70         Arctic ground squirrel Spermophilus herbi       Rodentia
## 71 Thirteen-lined ground squirrel Spermophilus herbi       Rodentia
## 72 Golden-mantled ground squirrel Spermophilus herbi       Rodentia
## 76      Eastern american chipmunk       Tamias herbi       Rodentia
## 77                Brazilian tapir      Tapirus herbi Perissodactyla
##    conservation sleep_total sleep_rem sleep_cycle awake brainwt   bodywt
## 3            nt        14.4       2.4          NA   9.6      NA    1.350
## 5  domesticated         4.0       0.7   0.6666667  20.0 0.42300  600.000
## 6          <NA>        14.4       2.2   0.7666667   9.6      NA    3.850
## 10           lc         3.0        NA          NA  21.0 0.09820   14.800
## 11           lc         5.3       0.6          NA  18.7 0.11500   33.500
## 12 domesticated         9.4       0.8   0.2166667  14.6 0.00550    0.728
## 14 domesticated        12.5       1.5   0.1166667  11.5 0.00640    0.420
## 19           lc         5.3       0.5          NA  18.7 0.01230    2.950
## 21           en         3.9        NA          NA  20.1 4.60300 2547.000
## 23 domesticated         2.9       0.6   1.0000000  21.1 0.65500  521.000
## 24 domesticated         3.1       0.4          NA  20.9 0.41900  187.000
## 27         <NA>        14.9        NA          NA   9.1      NA    0.071
## 30           cd         1.9       0.4          NA  22.1      NA  899.995
## 33           lc         6.3       0.6          NA  17.7 0.01227    2.625
## 35           vu         9.5       0.9          NA  14.5      NA    1.670
## 36           vu         3.3        NA          NA  20.7 5.71200 6654.000
## 39           lc        14.2       1.9          NA   9.8      NA    0.053
## 40           en        14.3       3.1   0.2000000   9.7 0.00100    0.120
## 41         <NA>        12.8        NA          NA  11.2      NA    0.035
## 42           nt        12.5       1.4   0.1833333  11.5 0.00040    0.022
## 44           nt        14.6        NA          NA   9.4      NA    0.266
## 46           lc         7.7       0.9          NA  16.3      NA    0.210
## 48 domesticated         8.4       0.9   0.4166667  15.6 0.01210    2.500
## 49 domesticated         3.8       0.6          NA  20.2 0.17500   55.500
## 61         <NA>        11.1       1.5          NA  12.9      NA    1.100
## 64           lc        13.0       2.4   0.1833333  11.0 0.00190    0.320
## 68         <NA>        11.3       1.1   0.1500000  12.7 0.00118    0.148
## 70           lc        16.6        NA          NA   7.4 0.00570    0.920
## 71           lc        13.8       3.4   0.2166667  10.2 0.00400    0.101
## 72           lc        15.9       3.0          NA   8.1      NA    0.205
## 76         <NA>        15.8        NA          NA   8.2      NA    0.112
## 77           vu         4.4       1.0   0.9000000  19.6 0.16900  207.501
```

7. We are interested in two groups; small and large mammals. Let's define small as less than or equal to 1kg body weight and large as greater than or equal to 200kg body weight. Make two new dataframes (large and small) based on these parameters.

```r
large <- subset(sleep, bodywt>=200)
large
```

```
##                name         genus  vore          order conservation sleep_total
## 5               Cow           Bos herbi   Artiodactyla domesticated         4.0
## 21   Asian elephant       Elephas herbi    Proboscidea           en         3.9
## 23            Horse         Equus herbi Perissodactyla domesticated         2.9
## 30          Giraffe       Giraffa herbi   Artiodactyla           cd         1.9
## 31      Pilot whale Globicephalus carni        Cetacea           cd         2.7
## 36 African elephant     Loxodonta herbi    Proboscidea           vu         3.3
## 77  Brazilian tapir       Tapirus herbi Perissodactyla           vu         4.4
##    sleep_rem sleep_cycle awake brainwt   bodywt
## 5        0.7   0.6666667 20.00   0.423  600.000
## 21        NA          NA 20.10   4.603 2547.000
## 23       0.6   1.0000000 21.10   0.655  521.000
## 30       0.4          NA 22.10      NA  899.995
## 31       0.1          NA 21.35      NA  800.000
## 36        NA          NA 20.70   5.712 6654.000
## 77       1.0   0.9000000 19.60   0.169  207.501
```

```r
small <- subset(sleep, bodywt<=1)
small
```

```
##                              name        genus    vore           order
## 2                      Owl monkey        Aotus    omni        Primates
## 4      Greater short-tailed shrew      Blarina    omni    Soricomorpha
## 8                    Vesper mouse      Calomys    <NA>        Rodentia
## 12                     Guinea pig        Cavis   herbi        Rodentia
## 14                     Chinchilla   Chinchilla   herbi        Rodentia
## 15                Star-nosed mole    Condylura    omni    Soricomorpha
## 16      African giant pouched rat   Cricetomys    omni        Rodentia
## 17      Lesser short-tailed shrew    Cryptotis    omni    Soricomorpha
## 22                  Big brown bat    Eptesicus insecti      Chiroptera
## 25              European hedgehog    Erinaceus    omni  Erinaceomorpha
## 27      Western american chipmunk     Eutamias   herbi        Rodentia
## 29                         Galago       Galago    omni        Primates
## 37           Thick-tailed opposum   Lutreolina   carni Didelphimorphia
## 39               Mongolian gerbil     Meriones   herbi        Rodentia
## 40                 Golden hamster Mesocricetus   herbi        Rodentia
## 41                          Vole      Microtus   herbi        Rodentia
## 42                    House mouse          Mus   herbi        Rodentia
## 43               Little brown bat       Myotis insecti      Chiroptera
## 44           Round-tailed muskrat     Neofiber   herbi        Rodentia
## 46                           Degu      Octodon   herbi        Rodentia
## 47     Northern grasshopper mouse    Onychomys   carni        Rodentia
## 55                Desert hedgehog  Paraechinus    <NA>  Erinaceomorpha
## 57                     Deer mouse   Peromyscus    <NA>        Rodentia
## 64                 Laboratory rat       Rattus   herbi        Rodentia
## 65          African striped mouse    Rhabdomys    omni        Rodentia
## 66                Squirrel monkey      Saimiri    omni        Primates
## 67          Eastern american mole     Scalopus insecti    Soricomorpha
## 68                     Cotton rat     Sigmodon   herbi        Rodentia
## 69                       Mole rat       Spalax    <NA>        Rodentia
## 70         Arctic ground squirrel Spermophilus   herbi        Rodentia
## 71 Thirteen-lined ground squirrel Spermophilus   herbi        Rodentia
## 72 Golden-mantled ground squirrel Spermophilus   herbi        Rodentia
## 73                     Musk shrew       Suncus    <NA>    Soricomorpha
## 76      Eastern american chipmunk       Tamias   herbi        Rodentia
## 78                         Tenrec       Tenrec    omni    Afrosoricida
## 79                     Tree shrew       Tupaia    omni      Scandentia
##    conservation sleep_total sleep_rem sleep_cycle awake brainwt bodywt
## 2          <NA>        17.0       1.8          NA   7.0 0.01550  0.480
## 4            lc        14.9       2.3   0.1333333   9.1 0.00029  0.019
## 8          <NA>         7.0        NA          NA  17.0      NA  0.045
## 12 domesticated         9.4       0.8   0.2166667  14.6 0.00550  0.728
## 14 domesticated        12.5       1.5   0.1166667  11.5 0.00640  0.420
## 15           lc        10.3       2.2          NA  13.7 0.00100  0.060
## 16         <NA>         8.3       2.0          NA  15.7 0.00660  1.000
## 17           lc         9.1       1.4   0.1500000  14.9 0.00014  0.005
## 22           lc        19.7       3.9   0.1166667   4.3 0.00030  0.023
## 25           lc        10.1       3.5   0.2833333  13.9 0.00350  0.770
## 27         <NA>        14.9        NA          NA   9.1      NA  0.071
## 29         <NA>         9.8       1.1   0.5500000  14.2 0.00500  0.200
## 37           lc        19.4       6.6          NA   4.6      NA  0.370
## 39           lc        14.2       1.9          NA   9.8      NA  0.053
## 40           en        14.3       3.1   0.2000000   9.7 0.00100  0.120
## 41         <NA>        12.8        NA          NA  11.2      NA  0.035
## 42           nt        12.5       1.4   0.1833333  11.5 0.00040  0.022
## 43         <NA>        19.9       2.0   0.2000000   4.1 0.00025  0.010
## 44           nt        14.6        NA          NA   9.4      NA  0.266
## 46           lc         7.7       0.9          NA  16.3      NA  0.210
## 47           lc        14.5        NA          NA   9.5      NA  0.028
## 55           lc        10.3       2.7          NA  13.7 0.00240  0.550
## 57         <NA>        11.5        NA          NA  12.5      NA  0.021
## 64           lc        13.0       2.4   0.1833333  11.0 0.00190  0.320
## 65         <NA>         8.7        NA          NA  15.3      NA  0.044
## 66         <NA>         9.6       1.4          NA  14.4 0.02000  0.743
## 67           lc         8.4       2.1   0.1666667  15.6 0.00120  0.075
## 68         <NA>        11.3       1.1   0.1500000  12.7 0.00118  0.148
## 69         <NA>        10.6       2.4          NA  13.4 0.00300  0.122
## 70           lc        16.6        NA          NA   7.4 0.00570  0.920
## 71           lc        13.8       3.4   0.2166667  10.2 0.00400  0.101
## 72           lc        15.9       3.0          NA   8.1      NA  0.205
## 73         <NA>        12.8       2.0   0.1833333  11.2 0.00033  0.048
## 76         <NA>        15.8        NA          NA   8.2      NA  0.112
## 78         <NA>        15.6       2.3          NA   8.4 0.00260  0.900
## 79         <NA>         8.9       2.6   0.2333333  15.1 0.00250  0.104
```

8. What is the mean weight for both the small and large mammals?

*For small mammals, the mean weight is 0.2597 kg, while the mean weight for large mammals is 1747.071 kg*


```r
smallWeights <- small[,11]
smallWeights
```

```
##  [1] 0.480 0.019 0.045 0.728 0.420 0.060 1.000 0.005 0.023 0.770 0.071 0.200
## [13] 0.370 0.053 0.120 0.035 0.022 0.010 0.266 0.210 0.028 0.550 0.021 0.320
## [25] 0.044 0.743 0.075 0.148 0.122 0.920 0.101 0.205 0.048 0.112 0.900 0.104
```

```r
mean(smallWeights)
```

```
## [1] 0.2596667
```


```r
largeWeights <- large[,11]
largeWeights
```

```
## [1]  600.000 2547.000  521.000  899.995  800.000 6654.000  207.501
```

```r
mean(largeWeights)
```

```
## [1] 1747.071
```

9. Using a similar approach as above, do large or small animals sleep longer on average?  

*On average, small animals tend to sleep longer with 12.65833 hours on average, while large animals sleep 3.3 hours on average.*

```r
smallSleep <- small[,6]
smallSleep
```

```
##  [1] 17.0 14.9  7.0  9.4 12.5 10.3  8.3  9.1 19.7 10.1 14.9  9.8 19.4 14.2 14.3
## [16] 12.8 12.5 19.9 14.6  7.7 14.5 10.3 11.5 13.0  8.7  9.6  8.4 11.3 10.6 16.6
## [31] 13.8 15.9 12.8 15.8 15.6  8.9
```

```r
mean(smallSleep)
```

```
## [1] 12.65833
```


```r
largeSleep <- large[,6]
largeSleep
```

```
## [1] 4.0 3.9 2.9 1.9 2.7 3.3 4.4
```

```r
mean(largeSleep)
```

```
## [1] 3.3
```

10. Which animal is the sleepiest among the entire dataframe?

*In terms of total sleep, the Little brown bat is the sleepiest animal with 19.9 hours. In terms of solely* **REM** *sleep, the Thick-tailed opposum had the most REM sleep with 6.6 hours*

```r
max(sleep$sleep_total)
```

```
## [1] 19.9
```

```r
laziest_animal <- sleep$name[which.max(sleep$sleep_total)]
laziest_animal
```

```
## [1] "Little brown bat"
```


```r
max(sleep$sleep_rem, na.rm = TRUE)
```

```
## [1] 6.6
```

```r
REM_animal <- sleep$name[which.max(sleep$sleep_rem)]
REM_animal
```

```
## [1] "Thick-tailed opposum"
```

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
