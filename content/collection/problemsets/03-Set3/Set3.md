---
title: "Problem Set 3"
author: "Brennan T. Beal"
date: 2020-06-27T21:13:14-05:00
categories: ["R"]
draft: FALSE
tags: ["Practice", "R"]
---




This problem set will be the last of them. It will build on the 4th module. So, if you've not checked it out, go ahead and do that! We will be building on a lot of components from the other modules as well.  
  
### Function Concepts
Here, I just want to touch on the general concepts of functions.  
  
  1. What is the general structure of a function?
  2. Consider this function: `my_function <- function(x = 2, y){ x + 2*y}`. Which of the following throws an error? (select all that apply... and I know... but this is not for a grade)
    + `my_function(y = 9)`
    + `my_function(x = 12)`
    + `my_function(12)`
    + `my_function(y = 8, x = 4)`
    + `my_function(12, 2)`
  3. Why do the functions you selected above produce an error?
  4. Using the same function as above, does `my_function(3, 7)` give the same answer as `my_function(7, 3)`?

### Writing Functions
Now to actually writing your own functions. Since you're on your third problem set, these will be a little bit more difficult. Remember, we have office hours and they're meant to be challenging! Don't give up. 
  
  1. Create a function that takes a number and multiplies it by itself.
  2. Functions can take any kind of argument, including vectors. Given what you know about vectors, create a function that takes 2 vectors, x_vec and y_vec, and subtracts the second value of y_vec from the first value of x_vec.
  3. Write a function that finds the mean of *any* given length of numbers. Two things on this one... No, you may not use the `mean()` function. Second, it needs to be flexible to take a vector of any length.
  
### For Loops and If Statements
Most of the time you'll know you need a *for loop* because when speaking out loud, you'll say something like "For all of these values, I need to do x,y,z". So, I'll phrase these problems that way.  
  
  1. Recall the mtcars data that we have used. For each value of horsepower (`hp`), check to see if the value is divisible by 2. If it is, store it in a new vector. What is the length of that vector?
  2. Take a look at the `letter` vector that comes pre-loaded in R. For each value in that vector, print the value to the console using `print()` if the letter is a vowel (aeiou).
  3. Use the following vector to answer your question: `random_words <- c("Brennan", "is", "mean", "for", "this", "question")`. For each word in the `random_words` vector, if the word is longer than 7 letters, append an exclamation mark. Else, do nothing. Store all of these values in a new vector, titled "new_vec", so that all the words go into a new vector (appended or not). What is the print out of `str_c(new_vec, collapse = " ")`?

  
  
  
  

