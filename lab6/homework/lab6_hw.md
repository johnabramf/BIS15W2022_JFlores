---
title: "Lab 6 Homework"
author: "John Abram Flores"
date: "2022-01-23"
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
library(skimr)
```

For this assignment we are going to work with a large data set from the [United Nations Food and Agriculture Organization](http://www.fao.org/about/en/) on world fisheries. These data are pretty wild, so we need to do some cleaning. First, load the data.  

Load the data `FAO_1950to2012_111914.csv` as a new object titled `fisheries`.

```r
fisheries <- readr::read_csv(file = "data/FAO_1950to2012_111914.csv")
```

```
## Rows: 17692 Columns: 71
```

```
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr (69): Country, Common name, ISSCAAP taxonomic group, ASFIS species#, ASF...
## dbl  (2): ISSCAAP group#, FAO major fishing area
```

```
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

1. Do an exploratory analysis of the data (your choice). What are the names of the variables, what are the dimensions, are there any NA's, what are the classes of the variables?  
*The dimensions of the data frame are shown by dim(), the names and classes of variables are shown by summary(), and we check NA's by running anyNA(), which outputs TRUE, informing us that there are NA's present.*

```r
dim(fisheries)
```

```
## [1] 17692    71
```

```r
summary(fisheries)
```

```
##    Country          Common name        ISSCAAP group#  ISSCAAP taxonomic group
##  Length:17692       Length:17692       Min.   :11.00   Length:17692           
##  Class :character   Class :character   1st Qu.:33.00   Class :character       
##  Mode  :character   Mode  :character   Median :36.00   Mode  :character       
##                                        Mean   :37.38                          
##                                        3rd Qu.:38.00                          
##                                        Max.   :77.00                          
##  ASFIS species#     ASFIS species name FAO major fishing area
##  Length:17692       Length:17692       Min.   :18.00         
##  Class :character   Class :character   1st Qu.:31.00         
##  Mode  :character   Mode  :character   Median :37.00         
##                                        Mean   :45.34         
##                                        3rd Qu.:57.00         
##                                        Max.   :88.00         
##    Measure              1950               1951               1952          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1953               1954               1955               1956          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1957               1958               1959               1960          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1961               1962               1963               1964          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1965               1966               1967               1968          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1969               1970               1971               1972          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1973               1974               1975               1976          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1977               1978               1979               1980          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1981               1982               1983               1984          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1985               1986               1987               1988          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1989               1990               1991               1992          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1993               1994               1995               1996          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1997               1998               1999               2000          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      2001               2002               2003               2004          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      2005               2006               2007               2008          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      2009               2010               2011               2012          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
## 
```

```r
anyNA(fisheries)
```

```
## [1] TRUE
```

2. Use `janitor` to rename the columns and make them easier to use. As part of this cleaning step, change `country`, `isscaap_group_number`, `asfis_species_number`, and `fao_major_fishing_area` to data class factor. 

*I first clean up the column names using janitor's clean_names(df) command.*

```r
fisheries <- janitor::clean_names(fisheries)
```
*Then, I set the columns that we will observe to factors using as.factor().*

```r
fisheries$country <- as.factor(fisheries$country)
fisheries$isscaap_group_number <- as.factor(fisheries$isscaap_group_number)
fisheries$asfis_species_number <- as.factor(fisheries$asfis_species_number)
fisheries$fao_major_fishing_area <- as.factor(fisheries$fao_major_fishing_area)
```

*I double check that the factor setting worked using str(fisheries). The variables we set earlier show up as factors!*

```r
str(fisheries)
```

```
## spec_tbl_df [17,692 x 71] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ country                : Factor w/ 204 levels "Albania","Algeria",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ common_name            : chr [1:17692] "Angelsharks, sand devils nei" "Atlantic bonito" "Barracudas nei" "Blue and red shrimp" ...
##  $ isscaap_group_number   : Factor w/ 30 levels "11","12","21",..: 14 12 13 19 8 13 9 19 14 26 ...
##  $ isscaap_taxonomic_group: chr [1:17692] "Sharks, rays, chimaeras" "Tunas, bonitos, billfishes" "Miscellaneous pelagic fishes" "Shrimps, prawns" ...
##  $ asfis_species_number   : Factor w/ 1553 levels "1020100101","1020100201",..: 92 970 1068 1256 398 581 817 1228 27 1499 ...
##  $ asfis_species_name     : chr [1:17692] "Squatinidae" "Sarda sarda" "Sphyraena spp" "Aristeus antennatus" ...
##  $ fao_major_fishing_area : Factor w/ 19 levels "18","21","27",..: 6 6 6 6 6 6 6 6 6 6 ...
##  $ measure                : chr [1:17692] "Quantity (tonnes)" "Quantity (tonnes)" "Quantity (tonnes)" "Quantity (tonnes)" ...
##  $ x1950                  : chr [1:17692] NA NA NA NA ...
##  $ x1951                  : chr [1:17692] NA NA NA NA ...
##  $ x1952                  : chr [1:17692] NA NA NA NA ...
##  $ x1953                  : chr [1:17692] NA NA NA NA ...
##  $ x1954                  : chr [1:17692] NA NA NA NA ...
##  $ x1955                  : chr [1:17692] NA NA NA NA ...
##  $ x1956                  : chr [1:17692] NA NA NA NA ...
##  $ x1957                  : chr [1:17692] NA NA NA NA ...
##  $ x1958                  : chr [1:17692] NA NA NA NA ...
##  $ x1959                  : chr [1:17692] NA NA NA NA ...
##  $ x1960                  : chr [1:17692] NA NA NA NA ...
##  $ x1961                  : chr [1:17692] NA NA NA NA ...
##  $ x1962                  : chr [1:17692] NA NA NA NA ...
##  $ x1963                  : chr [1:17692] NA NA NA NA ...
##  $ x1964                  : chr [1:17692] NA NA NA NA ...
##  $ x1965                  : chr [1:17692] NA NA NA NA ...
##  $ x1966                  : chr [1:17692] NA NA NA NA ...
##  $ x1967                  : chr [1:17692] NA NA NA NA ...
##  $ x1968                  : chr [1:17692] NA NA NA NA ...
##  $ x1969                  : chr [1:17692] NA NA NA NA ...
##  $ x1970                  : chr [1:17692] NA NA NA NA ...
##  $ x1971                  : chr [1:17692] NA NA NA NA ...
##  $ x1972                  : chr [1:17692] NA NA NA NA ...
##  $ x1973                  : chr [1:17692] NA NA NA NA ...
##  $ x1974                  : chr [1:17692] NA NA NA NA ...
##  $ x1975                  : chr [1:17692] NA NA NA NA ...
##  $ x1976                  : chr [1:17692] NA NA NA NA ...
##  $ x1977                  : chr [1:17692] NA NA NA NA ...
##  $ x1978                  : chr [1:17692] NA NA NA NA ...
##  $ x1979                  : chr [1:17692] NA NA NA NA ...
##  $ x1980                  : chr [1:17692] NA NA NA NA ...
##  $ x1981                  : chr [1:17692] NA NA NA NA ...
##  $ x1982                  : chr [1:17692] NA NA NA NA ...
##  $ x1983                  : chr [1:17692] NA NA NA NA ...
##  $ x1984                  : chr [1:17692] NA NA NA NA ...
##  $ x1985                  : chr [1:17692] NA NA NA NA ...
##  $ x1986                  : chr [1:17692] NA NA NA NA ...
##  $ x1987                  : chr [1:17692] NA NA NA NA ...
##  $ x1988                  : chr [1:17692] NA NA NA NA ...
##  $ x1989                  : chr [1:17692] NA NA NA NA ...
##  $ x1990                  : chr [1:17692] NA NA NA NA ...
##  $ x1991                  : chr [1:17692] NA NA NA NA ...
##  $ x1992                  : chr [1:17692] NA NA NA NA ...
##  $ x1993                  : chr [1:17692] NA NA NA NA ...
##  $ x1994                  : chr [1:17692] NA NA NA NA ...
##  $ x1995                  : chr [1:17692] "0 0" "1" NA "0 0" ...
##  $ x1996                  : chr [1:17692] "53" "2" NA "3" ...
##  $ x1997                  : chr [1:17692] "20" "0 0" NA "0 0" ...
##  $ x1998                  : chr [1:17692] "31" "12" NA NA ...
##  $ x1999                  : chr [1:17692] "30" "30" NA NA ...
##  $ x2000                  : chr [1:17692] "30" "25" "2" NA ...
##  $ x2001                  : chr [1:17692] "16" "30" NA NA ...
##  $ x2002                  : chr [1:17692] "79" "24" NA "34" ...
##  $ x2003                  : chr [1:17692] "1" "4" NA "22" ...
##  $ x2004                  : chr [1:17692] "4" "2" "2" "15" ...
##  $ x2005                  : chr [1:17692] "68" "23" "4" "12" ...
##  $ x2006                  : chr [1:17692] "55" "30" "7" "18" ...
##  $ x2007                  : chr [1:17692] "12" "19" NA NA ...
##  $ x2008                  : chr [1:17692] "23" "27" NA NA ...
##  $ x2009                  : chr [1:17692] "14" "21" NA NA ...
##  $ x2010                  : chr [1:17692] "78" "23" "7" NA ...
##  $ x2011                  : chr [1:17692] "12" "12" NA NA ...
##  $ x2012                  : chr [1:17692] "5" "5" NA NA ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   Country = col_character(),
##   ..   `Common name` = col_character(),
##   ..   `ISSCAAP group#` = col_double(),
##   ..   `ISSCAAP taxonomic group` = col_character(),
##   ..   `ASFIS species#` = col_character(),
##   ..   `ASFIS species name` = col_character(),
##   ..   `FAO major fishing area` = col_double(),
##   ..   Measure = col_character(),
##   ..   `1950` = col_character(),
##   ..   `1951` = col_character(),
##   ..   `1952` = col_character(),
##   ..   `1953` = col_character(),
##   ..   `1954` = col_character(),
##   ..   `1955` = col_character(),
##   ..   `1956` = col_character(),
##   ..   `1957` = col_character(),
##   ..   `1958` = col_character(),
##   ..   `1959` = col_character(),
##   ..   `1960` = col_character(),
##   ..   `1961` = col_character(),
##   ..   `1962` = col_character(),
##   ..   `1963` = col_character(),
##   ..   `1964` = col_character(),
##   ..   `1965` = col_character(),
##   ..   `1966` = col_character(),
##   ..   `1967` = col_character(),
##   ..   `1968` = col_character(),
##   ..   `1969` = col_character(),
##   ..   `1970` = col_character(),
##   ..   `1971` = col_character(),
##   ..   `1972` = col_character(),
##   ..   `1973` = col_character(),
##   ..   `1974` = col_character(),
##   ..   `1975` = col_character(),
##   ..   `1976` = col_character(),
##   ..   `1977` = col_character(),
##   ..   `1978` = col_character(),
##   ..   `1979` = col_character(),
##   ..   `1980` = col_character(),
##   ..   `1981` = col_character(),
##   ..   `1982` = col_character(),
##   ..   `1983` = col_character(),
##   ..   `1984` = col_character(),
##   ..   `1985` = col_character(),
##   ..   `1986` = col_character(),
##   ..   `1987` = col_character(),
##   ..   `1988` = col_character(),
##   ..   `1989` = col_character(),
##   ..   `1990` = col_character(),
##   ..   `1991` = col_character(),
##   ..   `1992` = col_character(),
##   ..   `1993` = col_character(),
##   ..   `1994` = col_character(),
##   ..   `1995` = col_character(),
##   ..   `1996` = col_character(),
##   ..   `1997` = col_character(),
##   ..   `1998` = col_character(),
##   ..   `1999` = col_character(),
##   ..   `2000` = col_character(),
##   ..   `2001` = col_character(),
##   ..   `2002` = col_character(),
##   ..   `2003` = col_character(),
##   ..   `2004` = col_character(),
##   ..   `2005` = col_character(),
##   ..   `2006` = col_character(),
##   ..   `2007` = col_character(),
##   ..   `2008` = col_character(),
##   ..   `2009` = col_character(),
##   ..   `2010` = col_character(),
##   ..   `2011` = col_character(),
##   ..   `2012` = col_character()
##   .. )
##  - attr(*, "problems")=<externalptr>
```

We need to deal with the years because they are being treated as characters and start with an X. We also have the problem that the column names that are years actually represent data. We haven't discussed tidy data yet, so here is some help. You should run this ugly chunk to transform the data for the rest of the homework. It will only work if you have used janitor to rename the variables in question 2!

```r
fisheries_tidy <- fisheries %>% 
  pivot_longer(-c(country,common_name,isscaap_group_number,isscaap_taxonomic_group,asfis_species_number,asfis_species_name,fao_major_fishing_area,measure),
               names_to = "year",
               values_to = "catch",
               values_drop_na = TRUE) %>% 
  mutate(year= as.numeric(str_replace(year, 'x', ''))) %>% 
  mutate(catch= str_replace(catch, c(' F'), replacement = '')) %>% 
  mutate(catch= str_replace(catch, c('...'), replacement = '')) %>% 
  mutate(catch= str_replace(catch, c('-'), replacement = '')) %>% 
  mutate(catch= str_replace(catch, c('0 0'), replacement = ''))

fisheries_tidy$catch <- as.numeric(fisheries_tidy$catch)
```

3. How many countries are represented in the data? Provide a count and list their names.
*There are a total of 204 countries, represented by the 204 total names listed below.*

```r
levels(fisheries_tidy$country)
```

```
##   [1] "Albania"                   "Algeria"                  
##   [3] "American Samoa"            "Angola"                   
##   [5] "Anguilla"                  "Antigua and Barbuda"      
##   [7] "Argentina"                 "Aruba"                    
##   [9] "Australia"                 "Bahamas"                  
##  [11] "Bahrain"                   "Bangladesh"               
##  [13] "Barbados"                  "Belgium"                  
##  [15] "Belize"                    "Benin"                    
##  [17] "Bermuda"                   "Bonaire/S.Eustatius/Saba" 
##  [19] "Bosnia and Herzegovina"    "Brazil"                   
##  [21] "British Indian Ocean Ter"  "British Virgin Islands"   
##  [23] "Brunei Darussalam"         "Bulgaria"                 
##  [25] "C<f4>te d'Ivoire"          "Cabo Verde"               
##  [27] "Cambodia"                  "Cameroon"                 
##  [29] "Canada"                    "Cayman Islands"           
##  [31] "Channel Islands"           "Chile"                    
##  [33] "China"                     "China, Hong Kong SAR"     
##  [35] "China, Macao SAR"          "Colombia"                 
##  [37] "Comoros"                   "Congo, Dem. Rep. of the"  
##  [39] "Congo, Republic of"        "Cook Islands"             
##  [41] "Costa Rica"                "Croatia"                  
##  [43] "Cuba"                      "Cura<e7>ao"               
##  [45] "Cyprus"                    "Denmark"                  
##  [47] "Djibouti"                  "Dominica"                 
##  [49] "Dominican Republic"        "Ecuador"                  
##  [51] "Egypt"                     "El Salvador"              
##  [53] "Equatorial Guinea"         "Eritrea"                  
##  [55] "Estonia"                   "Ethiopia"                 
##  [57] "Falkland Is.(Malvinas)"    "Faroe Islands"            
##  [59] "Fiji, Republic of"         "Finland"                  
##  [61] "France"                    "French Guiana"            
##  [63] "French Polynesia"          "French Southern Terr"     
##  [65] "Gabon"                     "Gambia"                   
##  [67] "Georgia"                   "Germany"                  
##  [69] "Ghana"                     "Gibraltar"                
##  [71] "Greece"                    "Greenland"                
##  [73] "Grenada"                   "Guadeloupe"               
##  [75] "Guam"                      "Guatemala"                
##  [77] "Guinea"                    "GuineaBissau"             
##  [79] "Guyana"                    "Haiti"                    
##  [81] "Honduras"                  "Iceland"                  
##  [83] "India"                     "Indonesia"                
##  [85] "Iran (Islamic Rep. of)"    "Iraq"                     
##  [87] "Ireland"                   "Isle of Man"              
##  [89] "Israel"                    "Italy"                    
##  [91] "Jamaica"                   "Japan"                    
##  [93] "Jordan"                    "Kenya"                    
##  [95] "Kiribati"                  "Korea, Dem. People's Rep" 
##  [97] "Korea, Republic of"        "Kuwait"                   
##  [99] "Latvia"                    "Lebanon"                  
## [101] "Liberia"                   "Libya"                    
## [103] "Lithuania"                 "Madagascar"               
## [105] "Malaysia"                  "Maldives"                 
## [107] "Malta"                     "Marshall Islands"         
## [109] "Martinique"                "Mauritania"               
## [111] "Mauritius"                 "Mayotte"                  
## [113] "Mexico"                    "Micronesia, Fed.States of"
## [115] "Monaco"                    "Montenegro"               
## [117] "Montserrat"                "Morocco"                  
## [119] "Mozambique"                "Myanmar"                  
## [121] "Namibia"                   "Nauru"                    
## [123] "Netherlands"               "Netherlands Antilles"     
## [125] "New Caledonia"             "New Zealand"              
## [127] "Nicaragua"                 "Nigeria"                  
## [129] "Niue"                      "Norfolk Island"           
## [131] "Northern Mariana Is."      "Norway"                   
## [133] "Oman"                      "Other nei"                
## [135] "Pakistan"                  "Palau"                    
## [137] "Palestine, Occupied Tr."   "Panama"                   
## [139] "Papua New Guinea"          "Peru"                     
## [141] "Philippines"               "Pitcairn Islands"         
## [143] "Poland"                    "Portugal"                 
## [145] "Puerto Rico"               "Qatar"                    
## [147] "R<e9>union"                "Romania"                  
## [149] "Russian Federation"        "Saint Barth<e9>lemy"      
## [151] "Saint Helena"              "Saint Kitts and Nevis"    
## [153] "Saint Lucia"               "Saint Vincent/Grenadines" 
## [155] "SaintMartin"               "Samoa"                    
## [157] "Sao Tome and Principe"     "Saudi Arabia"             
## [159] "Senegal"                   "Serbia and Montenegro"    
## [161] "Seychelles"                "Sierra Leone"             
## [163] "Singapore"                 "Sint Maarten"             
## [165] "Slovenia"                  "Solomon Islands"          
## [167] "Somalia"                   "South Africa"             
## [169] "Spain"                     "Sri Lanka"                
## [171] "St. Pierre and Miquelon"   "Sudan"                    
## [173] "Sudan (former)"            "Suriname"                 
## [175] "Svalbard and Jan Mayen"    "Sweden"                   
## [177] "Syrian Arab Republic"      "Taiwan Province of China" 
## [179] "Tanzania, United Rep. of"  "Thailand"                 
## [181] "TimorLeste"                "Togo"                     
## [183] "Tokelau"                   "Tonga"                    
## [185] "Trinidad and Tobago"       "Tunisia"                  
## [187] "Turkey"                    "Turks and Caicos Is."     
## [189] "Tuvalu"                    "Ukraine"                  
## [191] "Un. Sov. Soc. Rep."        "United Arab Emirates"     
## [193] "United Kingdom"            "United States of America" 
## [195] "Uruguay"                   "US Virgin Islands"        
## [197] "Vanuatu"                   "Venezuela, Boliv Rep of"  
## [199] "Viet Nam"                  "Wallis and Futuna Is."    
## [201] "Western Sahara"            "Yemen"                    
## [203] "Yugoslavia SFR"            "Zanzibar"
```

4. Refocus the data only to include country, isscaap_taxonomic_group, asfis_species_name, asfis_species_number, year, catch.

```r
fisheries_tidy_f <- select(fisheries_tidy, country, isscaap_taxonomic_group, asfis_species_name, asfis_species_number, year, catch)
```

5. Based on the asfis_species_number, how many distinct fish species were caught as part of these data?
*By piping the fisheries_tidy data frame into a summarise()  with n_distinct, we see that there are 1551 distinct fish species caught in this data.*

```r
fisheries_tidy_f %>%
  summarise(total_distinct = n_distinct(asfis_species_number))
```

```
## # A tibble: 1 x 1
##   total_distinct
##            <int>
## 1           1551
```

6. Which country had the largest overall catch in the year 2000?
*By running a summarize() command to sum all catches by country, we see that China had the largest overal catch in the year 200 with 25889 total fish caught.*

```r
fisheries_tidy_f %>%
  select(year,catch,country) %>%
  filter(year==2000) %>%
  group_by(country) %>%
  summarize(year2k_overall_catch = sum(catch, na.rm=TRUE)) %>%
  slice_max(year2k_overall_catch, n=10)
```

```
## # A tibble: 10 x 2
##    country                  year2k_overall_catch
##    <fct>                                   <dbl>
##  1 China                                   25899
##  2 Russian Federation                      12181
##  3 United States of America                11762
##  4 Japan                                    8510
##  5 Indonesia                                8341
##  6 Peru                                     7443
##  7 Chile                                    6906
##  8 India                                    6351
##  9 Thailand                                 6243
## 10 Korea, Republic of                       6124
```

7. Which country caught the most sardines (_Sardina pilchardus_) between the years 1990-2000?
*Morocco caught the most sardines between 1990 and 2000 with 7470 total sardines caught.*

```r
fisheries_tidy %>%
  filter(asfis_species_name == "Sardina pilchardus") %>%
  filter(between(year,1990,2000))%>%
  group_by(country) %>%
  summarize(sardine_sum = sum(catch, na.rm=TRUE)) %>%
  slice_max(sardine_sum, n=10)
```

```
## # A tibble: 10 x 2
##    country               sardine_sum
##    <fct>                       <dbl>
##  1 Morocco                      7470
##  2 Spain                        3507
##  3 Russian Federation           1639
##  4 Ukraine                      1030
##  5 France                        966
##  6 Portugal                      818
##  7 Greece                        528
##  8 Italy                         507
##  9 Serbia and Montenegro         478
## 10 Denmark                       477
```

8. Which five countries caught the most cephalopods between 2008-2012?
*China, the Republic of Korea, Peru, Japan, and Chile caught the most cephalopods between 2008-2012.*


```r
fisheries_tidy %>%
  filter(str_detect(isscaap_taxonomic_group, "Squids")) %>%
  filter(between(year,2008,2012)) %>%
  group_by(country) %>%
  summarize(cephalopod_sum_by_group = sum(catch, na.rm=TRUE)) %>%
  slice_max(cephalopod_sum_by_group, n=10)
```

```
## # A tibble: 10 x 2
##    country                  cephalopod_sum_by_group
##    <fct>                                      <dbl>
##  1 China                                       8349
##  2 Korea, Republic of                          3480
##  3 Peru                                        3422
##  4 Japan                                       3248
##  5 Chile                                       2775
##  6 United States of America                    2417
##  7 Indonesia                                   1622
##  8 Taiwan Province of China                    1394
##  9 Spain                                       1147
## 10 France                                      1138
```

9. Which species had the highest catch total between 2008-2012? (hint: Osteichthyes is not a species)
*The Theragra chalcogramma, commonly known as the Alaskan pollock, had the highest catch total in 2008-2012 with 41075 total caught.*

```r
fisheries_tidy %>%
  filter(!asfis_species_name == "Osteichthyes") %>%
  filter(between(year,2008,2012)) %>%
  group_by(asfis_species_name) %>%
  summarize(total_species_catch = sum(catch, na.rm=TRUE)) %>%
  slice_max(total_species_catch, n=20)
```

```
## # A tibble: 20 x 2
##    asfis_species_name     total_species_catch
##    <chr>                                <dbl>
##  1 Theragra chalcogramma                41075
##  2 Engraulis ringens                    35523
##  3 Katsuwonus pelamis                   32153
##  4 Trichiurus lepturus                  30400
##  5 Clupea harengus                      28527
##  6 Thunnus albacares                    20119
##  7 Scomber japonicus                    14723
##  8 Gadus morhua                         13253
##  9 Thunnus alalunga                     12019
## 10 Natantia                             11984
## 11 Thunnus obesus                       11692
## 12 Decapterus spp                       11244
## 13 Scomber scombrus                     11054
## 14 Xiphias gladius                      10333
## 15 Elasmobranchii                        9764
## 16 Sciaenidae                            9462
## 17 Makaira nigricans                     9379
## 18 Engraulis japonicus                   8376
## 19 Sepiidae, Sepiolidae                  7646
## 20 Engraulis encrasicolus                7335
```

10. Use the data to do at least one analysis of your choice.
*With the data, I chose to look at the most caught species of shark, ray, or chimaeras (class of Chondrichthyes) between the years 1972 and 2002. The Elasmobranchii, Rajiformes, and Raja spp are not specific species, and the highest actual species is the Squalus acanthias, or the spiny dogfish, with a total catch of 7522 in the year span.*


```r
fisheries_tidy %>%
  filter(str_detect(isscaap_taxonomic_group, "Shark")) %>%
  filter(between(year,1972,2002)) %>%
  group_by(asfis_species_name) %>%
  summarize(total_shark_catch = sum(catch, na.rm = TRUE)) %>%
  slice_max(total_shark_catch, n=10)
```

```
## # A tibble: 10 x 2
##    asfis_species_name total_shark_catch
##    <chr>                          <dbl>
##  1 Elasmobranchii                 48447
##  2 Rajiformes                     27945
##  3 Raja spp                        8952
##  4 Squalus acanthias               7522
##  5 Isurus oxyrinchus               4444
##  6 Mustelus spp                    4257
##  7 Lamna nasus                     4152
##  8 Squalidae                       4109
##  9 Carcharhinidae                  3016
## 10 Prionace glauca                 2822
```

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
