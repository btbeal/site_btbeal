---
title: "Solutions for PS 1"
author: "Brennan T. Beal"
date: 2020-06-29T21:13:14-05:00
categories: ["R"]
draft: FALSE
tags: ["R", "Solutions"]
---



### Basic Math and Logic with R

```r
# ---- SETUP
x <- 18
r_is_cool <- 23

# 1
x + r_is_cool
#2
x >= r_is_cool
#3
x < r_is_cool & x + 10 > r_is_cool

#4
sum(c(TRUE, TRUE, FALSE, TRUE))
```
  
## Indexing Vectors

```r
# ---- SETUP
what_the_vec <- c(seq.int(from = 8, to = 72, by = 2))

# 1
what_the_vec[14]
# 2
new_vec <- what_the_vec[c(2, 8, 20)]
# 3
new_vec[2] < new_vec[1]
# 4
new_vec2 <- new_vec[-1]
```
  
### Indexing Data Frames

#### Section 1
  

```r
# 1
dim(iris)
# --- OR ----
str(iris)
# str gives more info... but technically all we needed was dimensions

# 2
# str(iris) from above gives this answer as well
```

#### Section 2
  

```r
# 1
pet_width <- iris[,4]
# ----- OR -----
pet_width <- iris$Petal.Width # this is same thing as above
# 2
mean(pet_width)
# 3
iris[2,3]
# 4
sum(iris$Sepal.Width > 3.2)
# 5
sum(iris$Sepal.Width > 3.2)/length(iris$Sepal.Width)
# ---- OR
sum(iris$Sepal.Width > 3.2)/nrow(iris)
# Note that because iris$Sepal.Width is a vector, we can use length.
# nrow on a data frame gives a sort of "length" of a df
# So, either will work

# 6
table(iris$Species)
# 7
new_data <- iris[iris$Species != "setosa",]
# This one may have been tricky for you
# != means not equal and the comma tells us that we want
# the iris data frame for all rows where 
# iris$Species IS NOT EQUAL to "setosa"
# to check if you did this right - table(new_data$Species)

# 8 
mean(new_data$Sepal.Length)
# 9
new_data$large_petal_flag <- ifelse(new_data$Petal.Length > 4.5, 1, 0)
# 10
# sum
sum(new_data$large_petal_flag)
# proportion
sum(new_data$large_petal_flag)/length(new_data$large_petal_flag)
# --- OR ---
mean(new_data$large_petal_flag)
# Notice that the mean of a binary variable is the proportion of '1' values present
# So, both the above works better with mean being easier to read
```
  
### Dplyr
  

```r
# ---- SETUP
# install.packages("tidyverse")
library(tidyverse)
```

```
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──
```

```
## ✓ ggplot2 3.3.5     ✓ purrr   0.3.4
## ✓ tibble  3.1.6     ✓ dplyr   1.0.8
## ✓ tidyr   1.2.0     ✓ stringr 1.4.0
## ✓ readr   2.1.2     ✓ forcats 0.5.1
```

```
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
# 1 & # 2
fancy_iris <- iris %>%
  mutate(double_petal = 2 * Petal.Width) %>% # 1
  filter(double_petal > 2.3,
         double_petal < 4)
# Note that to filter by multiple things, we can separate by a comma
# If we wanted and "OR", we could've typed "|"
# Also, filter keeps true values. So, to filter OUT the values specified,
# we needed to reverse the signs

# 3
iris %>%
  group_by(Species) %>% 
  summarise(max_Sepal.Length = max(Sepal.Length))
```
















