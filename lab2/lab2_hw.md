---
title: "Lab 2 Homework"
author: "John Abram Flores"
date: "2022-01-09"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---

## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

1. What is a vector in R?  

*In R, vectors are a type of data structure that allows for storing of information. There are either numeric or character vectors, and we use a get command (<-) and a concatenate command (c()) in order to set a vector to an object.*

2. What is a data matrix in R?  

*In R, a data matrix is a series of vectors that are stacked on top of each other. You can use them for data tables or other functions*

3. Below are data collected by three scientists (Jill, Steve, Susan in order) measuring temperatures of eight hot springs. Run this code chunk to create the vectors.  

```r
spring_1 <- c(36.25, 35.40, 35.30)
spring_2 <- c(35.15, 35.35, 33.35)
spring_3 <- c(30.70, 29.65, 29.20)
spring_4 <- c(39.70, 40.05, 38.65)
spring_5 <- c(31.85, 31.40, 29.30)
spring_6 <- c(30.20, 30.65, 29.75)
spring_7 <- c(32.90, 32.50, 32.80)
spring_8 <- c(36.80, 36.45, 33.15)
```

4. Build a data matrix that has the springs as rows and the columns as scientists.  


```r
spring_temperatures <- c(spring_1, spring_2, spring_3, spring_4, spring_5, spring_6, spring_7, spring_8)
```


```r
spring_temperatures_matrix <- matrix(spring_temperatures, byrow=T, nrow = 8)
spring_temperatures_matrix
```

```
##       [,1]  [,2]  [,3]
## [1,] 36.25 35.40 35.30
## [2,] 35.15 35.35 33.35
## [3,] 30.70 29.65 29.20
## [4,] 39.70 40.05 38.65
## [5,] 31.85 31.40 29.30
## [6,] 30.20 30.65 29.75
## [7,] 32.90 32.50 32.80
## [8,] 36.80 36.45 33.15
```


5. The names of the springs are 1.Bluebell Spring, 2.Opal Spring, 3.Riverside Spring, 4.Too Hot Spring, 5.Mystery Spring, 6.Emerald Spring, 7.Black Spring, 8.Pearl Spring. Name the rows and columns in the data matrix. Start by making two new vectors with the names, then use `colnames()` and `rownames()` to name the columns and rows.


```r
springs <- c("Bluebell_Spring", "Opal_Spring", "Riverside_Spring", "Too_Hot_Spring", "Mystery_Spring", "Emerald_Spring", "Black_Spring", "Pearl_Spring")
scientists <- c("Jill", "Steve", "Susan")
```


```r
rownames(spring_temperatures_matrix) <- springs
colnames(spring_temperatures_matrix) <- scientists
spring_temperatures_matrix
```

```
##                   Jill Steve Susan
## Bluebell_Spring  36.25 35.40 35.30
## Opal_Spring      35.15 35.35 33.35
## Riverside_Spring 30.70 29.65 29.20
## Too_Hot_Spring   39.70 40.05 38.65
## Mystery_Spring   31.85 31.40 29.30
## Emerald_Spring   30.20 30.65 29.75
## Black_Spring     32.90 32.50 32.80
## Pearl_Spring     36.80 36.45 33.15
```

6. Calculate the mean temperature of all eight springs.

```r
mean(spring_1)
```

```
## [1] 35.65
```

```r
mean(spring_2)
```

```
## [1] 34.61667
```

```r
mean(spring_3)
```

```
## [1] 29.85
```

```r
mean(spring_4)
```

```
## [1] 39.46667
```

```r
mean(spring_5)
```

```
## [1] 30.85
```

```r
mean(spring_6)
```

```
## [1] 30.2
```

```r
mean(spring_7)
```

```
## [1] 32.73333
```

```r
mean(spring_8)
```

```
## [1] 35.46667
```

```r
Spring_Mean <- c(mean(spring_1), mean(spring_2), mean(spring_3), mean(spring_4), mean(spring_5), mean(spring_6), mean(spring_7), mean(spring_8))
Spring_Mean
```

```
## [1] 35.65000 34.61667 29.85000 39.46667 30.85000 30.20000 32.73333 35.46667
```

7. Add this as a new column in the data matrix.  


```r
spring_temperatures_matrix_with_mean <- cbind(spring_temperatures_matrix, Spring_Mean)
spring_temperatures_matrix_with_mean
```

```
##                   Jill Steve Susan Spring_Mean
## Bluebell_Spring  36.25 35.40 35.30    35.65000
## Opal_Spring      35.15 35.35 33.35    34.61667
## Riverside_Spring 30.70 29.65 29.20    29.85000
## Too_Hot_Spring   39.70 40.05 38.65    39.46667
## Mystery_Spring   31.85 31.40 29.30    30.85000
## Emerald_Spring   30.20 30.65 29.75    30.20000
## Black_Spring     32.90 32.50 32.80    32.73333
## Pearl_Spring     36.80 36.45 33.15    35.46667
```

8. Show Susan's value for Opal Spring only.


```r
spring_temperatures_matrix_with_mean[2,3]
```

```
## [1] 33.35
```

9. Calculate the mean for Jill's column only.  

```r
spring_temperatures_matrix_with_mean[1:8]
```

```
## [1] 36.25 35.15 30.70 39.70 31.85 30.20 32.90 36.80
```

```r
mean(spring_temperatures_matrix_with_mean[1:8])
```

```
## [1] 34.19375
```

10. Use the data matrix to perform one calculation or operation of your interest.

*I used the data matrix to calculate the means for each of the scientist's measurements, total spring mean, and rbind() it!*


```r
Jill_Mean <- c(mean(spring_temperatures_matrix_with_mean[1:8]))
Steve_Mean <- c(mean(spring_temperatures_matrix_with_mean[9:16]))
Susan_Mean <- c(mean(spring_temperatures_matrix_with_mean[17:24]))
Total_Mean <- c(mean(spring_temperatures_matrix_with_mean[1:24]))
Scientist_Mean <- c(Jill_Mean, Steve_Mean, Susan_Mean, Total_Mean)
Scientist_Mean
```

```
## [1] 34.19375 33.93125 32.68750 33.60417
```


```r
spring_temperatures_matrix_with_spring_and_scientist_mean <- rbind(spring_temperatures_matrix_with_mean, Scientist_Mean)
spring_temperatures_matrix_with_spring_and_scientist_mean
```

```
##                      Jill    Steve   Susan Spring_Mean
## Bluebell_Spring  36.25000 35.40000 35.3000    35.65000
## Opal_Spring      35.15000 35.35000 33.3500    34.61667
## Riverside_Spring 30.70000 29.65000 29.2000    29.85000
## Too_Hot_Spring   39.70000 40.05000 38.6500    39.46667
## Mystery_Spring   31.85000 31.40000 29.3000    30.85000
## Emerald_Spring   30.20000 30.65000 29.7500    30.20000
## Black_Spring     32.90000 32.50000 32.8000    32.73333
## Pearl_Spring     36.80000 36.45000 33.1500    35.46667
## Scientist_Mean   34.19375 33.93125 32.6875    33.60417
```
**Note**: The bottom right value represents the mean for all 24 total measurements (3 scientists, 8 springs)

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.  
