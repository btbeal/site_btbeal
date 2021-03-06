---
title: "Lesson 1 - intro to R"
author: "Brennan T. Beal"
date: 2020-07-04T21:13:14-05:00
categories: ["R"]
draft: FALSE
tags: ["R Markdown", "Lessons", "Intro"]
---



Welcome to The University of Washington and your crash course on R. Note that this class will not be incredibly robust (we have three days) but it will aim to provide you the tools necessary to do well in your stats classes and beyond. Futher, I believe several of you will take a semester long course in R. You'll be ahead of that game but will go into more depth during that class.\
\
My goal for this course is for you to become acquainted with R and all the basic capabilities. But, importantly, I will also teach you how to ask good questions and solve problems on your own - this is a skill that came late to me and can save you a lot of time (maybe tears) when you get stuck (you will get stuck).\
\
Finally, know that this is just supposed to be a primer for what we do in class. You're expected to at least skim the document; however, we will go over all of this in class as well so that you can ask questions while we go! This will be here for backup.\
\
For our first lesson, I want to discuss some of the basics:

  1. R as a Fancy Calculator
  2. R and Logical Operations
  3. Basic Data Structures in R
  4. Asking Good Questions

\


### R as a Fancy Calculator
At its very basic, R is just a fancy calculator. So, if I'm short paper and mental capacity for the day, I can always resort to R to handle some basic affairs. Try typing these into your own console... what happens?


```r
2 + 2 # this is the standard addition operator
9 * 5 # ... the multiplication opperator
8 / 2 # ... you get it.
```
\
It is rare that you're going to want to specify the number itself like we did above. Sometimes you may not even know even know the number to specificy. In this case, we can use the assignment operator, `<-`, to assign values to variables and store them for later use. This looks something like...
\

```r
x <- 5  # 'assign' the value 5 to the variable x (we could've picked any variable name... almost)
y <- 12 # same thing for y, here. We would say "'y' gets 12". 

# now with our variables assigned, we can play around with them however we choose. In the simple case from above...
x + y
```

```
## [1] 17
```

```r
# also noteworthy, though this is rarely useful:
# placing the whole thing in brackets prints the value AND stores it:
(x <- 5)
```

```
## [1] 5
```
\
You should remember that anything you store this will be in your environment until reassigned or removed. We will talk more about keeping your workspace tidy in class. If you're reading this and not taking the class, it may be worth poking around the internet about proper global environmnt management in R.\
\
Notably, R abides by the mathematical order of operations; however, you must specify the operation. Commonly when writing, I will represent 5 x 4 as (5)(4) and this will not work in R. Parenthesis in R, unless otherwise specified by the user, are reserved for functions (we will talk about this later). For now, try these out to see what I mean.\
\

```r
y*(x + 5)
# y(x+5) this will not run for the above reason
```
\
R can also do calculations on sets of numbers as well. To create a set of anything, we just concatenate values together using `c()` (for... `c`oncatenate). This works as so:\


```r
new_values_for_fun <- c(8, 7, 4, 3) # assigning a vector (more on that later) to a new variable. 
new_values_for_fun * 2
```

```
## [1] 16 14  8  6
```
So now you get it. R can just be a fancy calculator.

### R and Logical Operations



R, on top of the ability to do math, also understands logical operators such as `>`, `<`, `>=`, etc. R evaluates these as either TRUE or FALSE. A simple use case is to evaluate two statements. Try typing these into your console.\

```r
value_1 <- 10
value_2 <- 8

value_1 > value_2   # is value_1 greater than value_2?
value_1 == value_2  # is value_1 equivalent to value_2? (notice that we must use two '=' signs here. A single '=' is reserved for other cases that we will get to when we discuss functions)

value_1 != value_2
```

What happens? Could you anticipate it?\
\
We can use the `&` or the "or", `|`, sign as well.\


```r
6 > 8 & 9 < 12      # is 6 greater than 8 AND is 9 less than 12?
7 < 9 | (10/2) >= 6 # is 7 less than 9 OR is the quotient of 10 and 2 greater than or equal to 6?
```
\
I would recommend playing around with these for a bit. There are also use cases for `||` and `&&`, which we aren't going to go into for the sake of brevity.\
\
A couple final things to note about our logical machine. First, we can evaluate values within vectors as meeting some TRUE/FALSE criteria and that the TRUE values are equal to 1, while false values are equal to 0. This is a convenient truth that we can use later for all kinds of fun tricks (think about means of binary variables).\


```r
new_values_for_fun     # remember this variable from above? It should still be in your environment, so you can use it!
new_values_for_fun > 3 # Notice that R evaluates each value on the condition specified.
TRUE + TRUE
TRUE + FALSE
FALSE + FALSE
sum(new_values_for_fun > 3) # since we know TRUE is equal to 1, we can sum a condition and determine how many values within a given vector match that criteria. 
sum(new_values_for_fun > 3 & new_values_for_fun < 8) # two values in this vector are both greater than 3 AND less than 8.
```

Great. Now you can do math and basic logical operations in R! And wait... we also just used our first function, `sum()`. Sum, of course, is pretty intuitive - clue is in the name. We will be using every day functions throughout this course. I won't always explicitly define them. At any point, you can type `?some_function()` into the R console and it will pull up a screen describing the function and how to use it. Try typing `?sum()` into R to see.

### Basic Data Structures in R
There are four main data types in R. These are:

  1. Character
  2. Numeric
  3. Integer
  3. Logical
  
R will always guess what your data is, so most of the time you do not need to tell it. For example:


```r
char_variable <- "Words are fun" # this is a character value
another_var   <- 4               # this is an integer
num_var       <- 4.3             # ... numeric
logical_var   <- TRUE            # logical

# the command 'typeof()' will tell you what you're working with:
typeof(char_variable)

# these can be vectors - R will coerce into the simplest type
z <- c("Word", 4, 4.8, TRUE)
typeof(z) # notice what happens - R has 'coerced' everything into a character variable. 
z         # if we just print z, we notice that 4 is now "4" meaning it is a character and has lost its numeric meaning
```

R also has 4 main types of data structures (here in increasing complexity):

  1. Vector
  2. Matrix
  3. Data Frame
  4. List
  
We have already seen vectors and so I won't go over them again here. Here I want to introduce the other three and how we can check them out in our console. For our purposes, we will create these objects on the fly.


#### Matrices


Matrices are objects that have all rows and columns of a similar data type. For example, a full matrix of numeric, character, or logical data. We can create a matrix with `matrix()`. Type `?matrix()` to see all the arguments that `matrix()` needs. We will touch more on arguments to functions a bit later. For now, the goal is to understand what exactly a matrix is (not necessarily how to create one).\

```r
vector_for_matrix <- c(1, 9, 10, 8, 3, 2)
my_first_matrix <- matrix( # type ?matrix() to see all the arguments that matrix() needs. We will go more in to depth on this during functions. 
  data = vector_for_matrix,# tell it you want to use the numbers defined above
  nrow = 2,                # number of rows (we want a 2 x 3 matrix)
  byrow = TRUE             # do we want the numbers to fill in by row?
)

# now look at the matrix...
my_first_matrix
```

```
##      [,1] [,2] [,3]
## [1,]    1    9   10
## [2,]    8    3    2
```

```r
# what if we made one of those values a character?
vector_for_matrix <- c(1, 9, 10, 8, 3, "two")
my_first_matrix <- matrix( # type ?matrix() to see all the arguments that matrix() needs. We will go more in to depth on this during functions. 
  data = vector_for_matrix,# tell it you want to use the numbers defined above
  nrow = 2,                # number of rows (we want a 2 x 3 matrix)
  byrow = TRUE             # do we want the numbers to fill in by row?
)

# Now we have a character matrix - a matrix can only have one kind of value and therefore coerced the others to character
my_first_matrix
```

```
##      [,1] [,2] [,3] 
## [1,] "1"  "9"  "10" 
## [2,] "8"  "3"  "two"
```

If we want to check to see what kind of structure it is, we can use the `class()` command. Try typing `class(my_first_matrix)` into the console.


#### Data Frames


Data frames are similar with the exception that only **columns** need to be the same type of data. Only columns must be the same data type and columns must be the same length. For example...\


```r
my_first_df <- data.frame(  # note that data frame is commonly referenced as 'df'
  column_1 = c(8, 4, 9, 2), # one column is numeric (also notice arbitrary naming of columns)
  column_2 = c("green", "eggs", "and", "ham"), #... character
  column_3 = c(TRUE, TRUE, FALSE, TRUE) # logical
)

my_first_df # print it and check it out. 
```

```
##   column_1 column_2 column_3
## 1        8    green     TRUE
## 2        4     eggs     TRUE
## 3        9      and    FALSE
## 4        2      ham     TRUE
```

A little more time is deserved here given that most structures you will be using will be data frames. Here are some useful commands for checking them out (since you're not going to want to print a 1,000,000 x 40 data set to your console... or maybe you do?).\


```r
# the str() function is useful to check out the structure of your data
str(my_first_df)
```

```
## 'data.frame':	4 obs. of  3 variables:
##  $ column_1: num  8 4 9 2
##  $ column_2: Factor w/ 4 levels "and","eggs","green",..: 3 2 1 4
##  $ column_3: logi  TRUE TRUE FALSE TRUE
```

```r
# Notice we have 3 columns with 4 observations a numerical, logical, and.... factor?
# Okay there is this whole debate on data.frame converting characters to factors by default. Let us change that back.
my_first_df <- data.frame(  # note that data frame is commonly referenced as 'df'
  column_1 = c(8, 4, 9, 2), # one column is numeric (also notice arbitrary naming of columns)
  column_2 = c("green", "eggs", "and", "ham"), #... character
  column_3 = c(TRUE, TRUE, FALSE, TRUE), # logical
  stringsAsFactors = FALSE
)

str(my_first_df) # that's better
```

```
## 'data.frame':	4 obs. of  3 variables:
##  $ column_1: num  8 4 9 2
##  $ column_2: chr  "green" "eggs" "and" "ham"
##  $ column_3: logi  TRUE TRUE FALSE TRUE
```

```r
# sometimes we just want to see the first few rows
head(my_first_df, 2) # head() tells you to take the first x rows of the df
```

```
##   column_1 column_2 column_3
## 1        8    green     TRUE
## 2        4     eggs     TRUE
```

```r
# or rename the columns (note we need three names, one for each column)
names(my_first_df) <- c("Brennan", "Is", "Cool")
```

We will go into more detail in a bit about acutal manipulation of thesee data frames. But for now, those should be a helpful starting point.

#### Lists


Finally, we should take a look at lists. These are slightly more complex and rarely used for basic data analysis. But, it is probably helpful that you know about them nonetheless. Lists are... well they are lists. They are the most flexible of all the structures as they can take any amount of the other data types/structures we have discussed. Imagine lists as a storage facility with each of the units being an individual data structure. For example, let us create a list of all the structures we have created thus far...


```r
# so far, we have made a matrix, my_first_matrix; a data frame, my_first_df, and some misc. vectors. We can combine these in a list.
my_first_list <- list(
  storage_unit_1 = my_first_df,
  storage_unit_2 = my_first_matrix,
  storage_unit_3 = new_values_for_fun # remember that vector from the beginning?
)

# print it 
my_first_list
```

```
## $storage_unit_1
##   Brennan    Is  Cool
## 1       8 green  TRUE
## 2       4  eggs  TRUE
## 3       9   and FALSE
## 4       2   ham  TRUE
## 
## $storage_unit_2
##      [,1] [,2] [,3] 
## [1,] "1"  "9"  "10" 
## [2,] "8"  "3"  "two"
## 
## $storage_unit_3
## [1] 8 7 4 3
```

That looks like a mess but they're actually quite useful. We don't have time here to get into all the uses. Just know that they exist and if you come across them, you will be familiar.\
\
That is it for our first lesson - now you have the tools you need to begin wading through the waters of R. But before you go, I mentioned asking good questions, and where to ask them.

### Asking Good Questions


This could've easily been titled "How to Not Be a Jerk Online" but alas. One of the reasons R is great is for the community. R is open source so the community is broad and continuously expanding. Part of that community is an awesome online forum (several, actually) of people asking and answering R-related questions. I prefer [StackOverflow](https://stackoverflow.com/questions/tagged/r).\
\
The biggest part of asking questions online is understanding that the people answering are taking time out of their day to respond, for free, just for fun. For this reason, it is helpful to be as clear as possible and make sure that the question follows general 'reprex' guidance. A reprex is a reproducible example. [Here](https://community.rstudio.com/t/faq-whats-a-reproducible-example-reprex-and-how-do-i-do-one/5219) is a link for how to do that effectively.\
\
In general, make sure that your question is clear and concise. Next, reproduce your error by putting all relevant code in the question. Then, show all of the things you have tried/places you have looked for the answer. Finally, if you asked a question that could have been solved by typing `?function`, people are going to be less inclined to help (I know because I've done that... ). Do what you can to help yourself first.\
\
See you in the next lesson!











