---
title: "Lesson 2 - Indexing and Tidyverse"
author: "Brennan T. Beal, PharmD, MS"
date: 2020-07-03T21:13:14-05:00
categories: ["R"]
draft: FALSE
tags: ["R Markdown", "Lessons", "Intro"]
---




In the previous lesson we covered the very basics of R. We discussed that R can be a fancy calculator, interpret logical arguments, and some fun facts sprinkled throughout. In addition, we covered basic data types and structures. Alone, this is good information to know... and is indeed the very bare bones introduction... but we are here to look at data - to manipulate data in a meaningful way. The first step towards data exploration is learning how to find values in a data set. We call this indexing.\
\
Indexing is one of the most parts of R and extends into everything you'll do from here one out. First, we will learn how to index and manipulate data "the hard way". Then, I will show you a much easier way with `tidyverse`. Of course, you could just skip to the `tidyverse` part but you are going to be sad when you need to write a loop in the next lesson. So, stick with me.\

\
In this lesson:

  1. Indexing and Manipulating Data
  2. `Tidyverse` 


### Indexing and Manipulating Data


#### Vectors

Recall in our last lesson, we learned how to create vectors with the basic assignment operator, `<-`, and the `c()` function: `x <- c(1, 8, 6)`. To find a value within this vector, we can simply reference the vector and tell R the position of the value we want to find.\
\
To tell R where to look, we use brackets, `[]`, and then give it coordinates to locate the value. Since vectors are one-dimensional, we just need to give it one value.\ 


```r
x <- c(1, 8, 6) # store a vector in variable x

# this gives us the second value of our x vector
x[2] 
```

```
## [1] 8
```

```r
# we can do arithmetic with these values
x[2] + x[3]
```

```
## [1] 14
```

We can pull as many values as we want from this vecor. To do this, we just provide another vector of positions.\


```r
x <- c(1, 8, 6) # store a vector in variable x

# this gives us the first and third value
x[c(1,3)]
```

```
## [1] 1 6
```

What would the following return? Type this into your console once you think you have an answer.\

```r
new_vector <- c(22, 18, 40, 49)
new_vector[c(4, 1)]
```

Intuitively, we can extract values and store them elsewhere as we need. Suppose we need to store the first and second value into a brand new vector that we are creating. We call this subsetting... because... we are taking a subset.\


```r
# place the first two values in our next vector, vector_subset
vector_subset <- new_vector[c(1,2)] 

# or if there is a value we DO NOT want, we can just provide the '-' sign.
vector_subset <- new_vector[-c(1,4)]

# we can subset these vectors as many times in a row as we'd like... though it'd be a little redundant. Just FYI. Think about this one:
new_vector[c(2,3,4)][c(1,2)][2]
```

This is useful, but usually we want to subset vectors based on some logical criteria. For example, what if we wanted to remove all values greater than 20?\


```r
# This reads: "subset new_vector for all the values of new_vector greater than 20"
new_vector[new_vector > 20] 
```

```
## [1] 22 40 49
```

Can you think of a way to subset `new_vector` for values greater than 20 AND less than 40? I'll let you noodle on that one. We can discuss in class.


#### Data Frames

Now that you're an expert in vector manipulation, there are only a couple extensions until you're an expert at manipulating data frames. For this segment of the lesson, we're going to use the `mtcars` dataset, which is preloaded in base R (so we don't have to actually read in the data).\
\
Go ahead and take a peak at the data (do you remember how?).\
\


```r
str(mtcars)
```

```
## 'data.frame':	32 obs. of  11 variables:
##  $ mpg : num  21 21 22.8 21.4 18.7 18.1 14.3 24.4 22.8 19.2 ...
##  $ cyl : num  6 6 4 6 8 6 8 4 4 6 ...
##  $ disp: num  160 160 108 258 360 ...
##  $ hp  : num  110 110 93 110 175 105 245 62 95 123 ...
##  $ drat: num  3.9 3.9 3.85 3.08 3.15 2.76 3.21 3.69 3.92 3.92 ...
##  $ wt  : num  2.62 2.88 2.32 3.21 3.44 ...
##  $ qsec: num  16.5 17 18.6 19.4 17 ...
##  $ vs  : num  0 0 1 1 0 1 0 1 1 1 ...
##  $ am  : num  1 1 1 0 0 0 0 0 0 0 ...
##  $ gear: num  4 4 4 3 3 3 3 4 4 4 ...
##  $ carb: num  4 4 1 1 2 1 4 2 2 4 ...
```

So we see we have a data set of 32 observations (or rows) and 11 variables (or columns). We could have found this out by typing `nrow(mtcars)` or `ncol(mtcars)` as well (these are useful commands for other reasons). Since we subset vectors with just one number (the positions within the vector), we could've probably guessed that to subset a data frame, we will need two numbers: the row and the column.\


```r
# So, we can take a look at the first row and second column like this:
mtcars[1,2]
```

```
## [1] 6
```

There is another way to subset dataframes as well, which is by using the `$` symbol. To look at any given variable within our data, we simply type something like this: `df$variable`. We can think of that output as its own vector. See if you can take a peek at the variable `cyl` within mtcars.\
\
Did you figure it out?\
\
You should've come up with something like this:\


```r
#subsetting with the $ sign gives a vector
mtcars$cyl
```

And just like any other regular vector, we can subset this vector based on logical operations or positions.\


```r
#subsetting the cyl based on position (first and third)
mtcars$cyl[c(1,3)]


# subsetting the cyl value based on a logical operation
mtcars$cyl[mtcars$cyl > 4]
```

\
Here, I want to take a bit of a side step to provide another useful function for looking at categorical data, like the `cyl` column of the `mtcars` data frame. There are a few ways to look at categorical data, but one of my favorites is to simply type `table`, which shows me my unique categories and how many there are. Try it!\


```r
table(mtcars$cyl)
```


Hopefully by now, things are starting to come together. You can subset a vector and sort of subset a data frame. Just like vectors, we mostly don't want to just type coordinates in to the data frame. We, more often than not, want to subset the frame by a logical criteria. So, can you think of a way to subset the `mtcars` data for all cars with greater than 4 cylinders?\


```r
# this reads: subset the mtcars df for all cylinder values of mtcars > 4
# notice the comma for the data frames - this means that we want all columns (the second value) that match the row criteria.
mtcars[mtcars$cyl > 4,]
```

And just like vectors, we can store that new data frame somewhere else.\


```r
my_fancy_newDF <- mtcars[mtcars$cyl > 4,]
```


Try to creat a new data frame called `df_subset`, which has all the columns where the cylinders are greater than 4 AND the wt is less than three.\
\
Is your new data set 3 rows? If so, you nailed it. Again, if not we will have class time to sort these things out!\
\
Finally, for data frames, we have two more useful tricks. Sometimes, we are going to want to change all values of the same type. Let's say, for instance, that we want to make all gears that are greater than 3, 100. This is completely arbitrary... but stick with me.\


```r
# first, let's put the mtcars into another data frame... notably, when we are changing values, there is no "undo". So, i usually create a dummy df so I have my original one if needed.
new_df <- mtcars
new_df$gear[new_df$gear > 3] <- 100
```

Now you have a new data frame, `new_df`, where all the gear values greater than 3 have been changed to 100. This is the long way... but it is important that you learn it this way first for debugging issues down the road.\
\
Last, we can make brand new columns on these data frames by simply using the `$` operator that we learned about and calling a brand new variable that you've made up. This can be useful in many cases. For now, we can use it to break our data into binary variables.\


```r
# let us create a new column, my_column, that is binary form of mpg
# Here, I will introduce you to the ifelse statement. Use ?ifelse to learn more
new_df$my_column <- ifelse(new_df$mpg > 20, yes = 1, no = 0)

# this reads:
# for my new column, my_column, if the mpg vector is greater than 20, assign 1, else assign 0.
```

Check to see that you've done it. I would suggest pausing here and playing around with the data. What can you see? How many rows match the recent criteria? Try summing `new_df$my_column`, which should tell you. Are the pieces coming together?


### Dplyr from Tidyverse
Now that you've learned the hard way, let's make everyone's life easier. Tidyverse is a package - your very first package! - which comes complete with a ton of tools to make your life easier. One of these is dplyr. Mainly, for today, I just want to talk bout the set of verbs that are used most frequently from dplyr but if you want to learn more (and you should!), you can find a lot of info [on their website.](https://www.tidyverse.org/)\

The most common data manipulation verbs are:

  1. Mutate (for changing a variable or creating a new one)
  2. Filter (for subsetting your data)
  3. Select (for selecting groups of variables from your data)
  4. Group_by (for... grouping by certain kinds of data)
  5. Summarise (to... summarise your data)

Already, I hope you find these namess intuitive. One of the biggest advantages to using the tidyverse parlance is it makes the code so much more readable. That is important in any endeavor but especially for academic transparency. The other advantage is you don't have to constantly save the data everytime you manipulate it. Remember doing that with the columns above? Storing a new variable? With tidyverse, we use what is called "piping", which refers to the pipe operator, ` %>% `. This operator is fancy computer code for "next, do this". This will make more sense when we get to an example.\
\
Anytime you need to use a package in R, you must first install it with `install.packages("some_package")`. Then, you have to load it with `library(some_package). Notice the quotes and lack thereof.\


```r
# first, install the package. We only need to do this once.
# install.packages("tidyverse")

# then load it. We need to do this anytime we restart R.
library(tidyverse)
```

```
## ?????? Attaching packages ????????????????????????????????????????????????????????????????????????????????????????????????????????????????????? tidyverse 1.3.1 ??????
```

```
## ??? ggplot2 3.3.5     ??? purrr   0.3.4
## ??? tibble  3.1.6     ??? dplyr   1.0.8
## ??? tidyr   1.2.0     ??? stringr 1.4.0
## ??? readr   2.1.2     ??? forcats 0.5.1
```

```
## ?????? Conflicts ?????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????? tidyverse_conflicts() ??????
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

Cool. You've installed your first package. Now, lets `mutate()` a column. Continuing on with the mtcars data, let us suppose we want to create a binary variable to distinguish between cars with greater than or equal to 90 hp. Typically, we would have done something like this: `mtcars$new_variable <- ifelse(mtcars$hp >= 90, 1, 0)`. Now, we can just tell R to `mutate` a new column onto the existing data like this:\


```r
mtcars %>% # notice the pipe (shortcut for typing this is shift + control + m on mac)
  mutate(new_column = ifelse(hp >= 90, 1, 0))

# you can mutate as many new variables as you'd like in one go:
mtcars %>% 
  mutate(new_column1 = ifelse(hp >= 90, 1, 0), # notice the comma to separate columns
         new_column2 = cyl + 6) # every data point in cyl plus 6. 
```


Notice a few things about the above. First, instead of referring to hp as `mtcars$hp`, we can just tell it to use the variable without specifying the data set. It knows what data we are referring to because we starting our operations with it. Second, notice that we just started with `mtcars %>% `, which tells R to manipulate that data in some fashion. Finally, notice we didn't actually assign this data anywhere so... it just gets printed and thrown out. If we wanted to assign that data to a new df, you know what to do (`<-`).\
\
Now let's suppose that we want to just get rid of all those columns that are less than 90. Here, we can use the `filter()` command (clue is in the verb, usually).\
\

```r
 # filter out all values that are NOT greater than or equal to 90
mtcars %>% 
  filter(hp >= 90)

# we can filter  by as many things as we'd like
# note that the comma represents 'AND'. You can place | just like our other operations if you need
mtcars %>% 
  filter(hp >= 90,
         cyl > 4)

# notably, we could mutate a column, then filter by it 
# this may make your code slightly more readable
mtcars %>% 
  mutate(wt2 = wt * 2) %>% 
  filter(wt2 < 6)
```


You can see that this way is much more straightforward than the first way we learned.\
\
Next up, we can learn to summarise our data. For example, we may want to know the mpg average for each kind of cylinder.\
\
To do this, we can simply `group_by()` the cylinder variable, and then `summarise()` the `mpg` variable.\
\

```r
mtcars %>% 
  group_by(cyl) %>% 
  summarise(mean_mpg = mean(mpg)) %>% 
  ungroup() # it is always a good idea to "ungroup()" once you're done with grouping manipulation
```

```
## # A tibble: 3 ?? 2
##     cyl mean_mpg
##   <dbl>    <dbl>
## 1     4     26.7
## 2     6     19.7
## 3     8     15.1
```

You can see that we now have the means of each cylinder group. Nice! The tidyverse package makes data manipulation much easier. This is the extent of our second lesson. By now, you should be familiar with how to index vectors and data sets by selecting a relative position, or with logical expressions. To make this easy, we have also skimmed the service of the tidyverse package. For our third lesson, we are going to go more into depth with tidyverse looking at date and string manipulation!\
\
See you there.
