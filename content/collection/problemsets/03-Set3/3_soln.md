---
title: "Solutions for PS 3"
author: "Brennan T. Beal"
date: 2020-06-28T21:13:14-05:00
categories: ["R"]
draft: FALSE
tags: ["R", "Solutions"]
---


### Function Concepts
  
Answer 1: The general structure of a function if `function(~arguments~){do something to arguments}`. Of course, that is an informal definition so anything resembling that counts!  
  
Answer 2: Neither the second or third option will run.  
  
Answer 3: The two mentioned produce errors because there is no specified `y` value. The `x` value is pre-specified and so as long as we tell R what the `y` value should be, it will use the default x.  
  
Answer 4: No because R takes arguments in order specified in the function unless otherwise specified.  
  
### Writing Functions

```r
# Question 1:
squaring_numbers <- function(x){
  x^2
}

# Question 2:
vec_subtractor <- function(x_vec, y_vec){
  y_vec[2] - x_vec[1]
}

# Question 3:
mean_function <- function(any_vector){
  sum(any_vector)/length(any_vector)
}
```
  
### For Loops and If Statements

```r
# Question 1
# -- Pull wt values from mt cars
hp_values <- mtcars$hp

# -- Store the numbers to loop over (this makes the code a bit neater)
len_hp <- length(hp_values)

# open a vector to store the values in 
updated_hp_vec <- c()
for(hp in 1:len_hp){
  # store our looped value by indexing our wt vector
  value <- hp_values[hp]
  
  # if its divisible by two...
  if(value %% 2 == 0){
    # update our vector (make sure you're putting the og vector in as well)
    # if you don't place updated vec inside c() you'll just rewrite your value
    updated_hp_vec <- c(updated_hp_vec, value)
  } else {
    # otherwise, next value
    next
  }
}

# now, find how many values are in our vector
length(updated_hp_vec)

# Question 2
# Store the length of the vector
l_length <- length(letters)


for(l in 1:l_length){
  # pull value from vector
  value <- letters[l]
  
  # str_detect returns T/F so...
  if(str_detect(value, "[AaEeIiOoUu]")){
    
    # print the value
    # the output should tell you if you did this right
    print(paste(value))
  } else {
    next
  }
}


# Question 3
random_words <- c("Brennan", "is", "mean", "for", "this", "question")

vec_length <- length(random_words)

new_vec <- c()
for(w in 1:vec_length){
  # index value from vector
  word <- random_words[w]
  # find length of that word
  word_length <- str_length(word)
  
  if(word_length > 7){
    new_word <- str_c(word, "!")
  } else {
    new_word <- word
  }
  new_vec <- c(new_vec, new_word)
}

str_c(new_vec, collapse = " ")
```

Congrats! You're finished with the crash course!


