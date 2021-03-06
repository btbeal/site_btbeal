---
title: "Lesson 5 - Functions and Loops in R"
author: "Brennan T. Beal, PharmD, MS"
date: 2020-06-30T21:13:14-02:00
categories: ["R"]
output:
  blogdown::html_page
draft: FALSE
tags: ["R Markdown", "Lessons", "Intro"]
---



Now that you've mastered the basics of R, and data manipulation with dplyr, you're ready for an introduction to functions and loops. Hopefully, the time you'll need for loops is minimal since dplyer has solved some grouping issues for us. But, you should still learn them just in case (and so you can understand them when reading others' code).\
\
Here I want to cover:  
  
  1. Functions and if statements
  2. Loops


### Functions and if statements
Functions are a key part of R. Regarding R, John Chambers is quoted saying that 'Everything that exists is an object and everything that happens is a function call'. You may not have noticed but everything we have done thus far has used a function. The assignment operator, `<-`, and the piping operator, ` %>% ` are functions. So are all the dplyr verbs we discussed. In this sense, even though we may not need to create our own, we should still understand how best to interact with the ones we are using day-to-day.\
\
In general, a function is constructed by using the `function` command and assignment arguments (or variables) that you'd like the function to rely on. I think that the [R for Data Science](https://r4ds.had.co.nz/functions.html) book teaches it best. Paraphrasing:

>There are three key steps in creating a function:
>
> First, You need to pick a name for the function.
>
> Second, you list the arguments for a function inside `function()`
>
> Finally, place the code for the arguments inside {} after `function()`


So, for our first function, we want to take any two variables, multiply the first variable by 2 and then add the second to the product.\


```r
# We want to name our function: my_first_function
# and we specified that it would need two numbers supplied. We will just call them 'x' and 'y'
my_first_function <- function(x, y){
  # simply telling the function that it should multiply x by 2 and add y - this inside brackets {}
  x*2 + y   
} # close the bracket

# Now, test it out:
my_first_function(x = 892, y = 1764)
```

Notice that when we called our function, we told it that the x value should equal 892 and that y should be 1764. What happens if you don't specify x or y? What happens if you type `my_first_function(892, 1764)`? Notice that the code still runs the same. R understands that you want those variables assigned. Try mixing it up and calling `my_first_function(1764, 892)`. See that the result is different? Because the first number is assigned to x. However, one final caveat is that we could rearrange the order, as long as we specified the variable. For example, now look at `my_first_function(y = 1764, x = 892)`. Now we have our original answer.\
\
In general, especially as you get better, it is good practice to specify the variables explicitly when using code. This can be good for you to begin to memorize arguments that some functions use, as well as helping your reader that may be less familiar with a given function.\
\
Since we are learning the basics of functions, I should add one more important thing: often times functions make use of hidden arguments, or arguments within a function that have a pre-assigned value.\
\
Let me show you want I mean. Suppose we want a function that just requires 3 variables, x, y, and z, and returns their sum. But, imagine that z was almost always the same, z = 4. We could specify that within our function so we don't need to type it everytime.\


```r
# we will set z = to four from the beginning
add_three <- function(x, y, z = 4){
  x + y + z
}

# now, try calling this function with just two numbers:
add_three(4, 3)
```


Notice that R placed 4 and 3 as the x and y variable and assumed that z was four. Just like the other functions, we could always override this value by calling something like `add_three(4,3,z = 6)` or `add_three(4,3,6)`.\
\
Finally, since we can specify one value, we could specify all of them and call an empty function. For example, suppose I want to have a function to spit out "1 apple a day keeps 1 doctor away" - the true golden ratio. To do so, I could simply create:


```r
cheesy_function <- function(apple_num = 1, figure_num = 1, profession = "doctor"){
  
  # first, turn their number into a character
  apples <- as.character(apple_num) 
  figure <- as.character(figure_num)
  
  print(paste(apples, "apple a day keeps", figure, profession, "away", sep = " "))
  
}

# now I can call this function without giving any arguments
cheesy_function()
```

```
## [1] "1 apple a day keeps 1 doctor away"
```


Cool. Now I have a REALLY useful function that people are going to want to use. And, it is flexible. We could have used any number of apples, or professionals, and we can pick our own profession. For example, try `cheesy_function(9, 9, "zookeeper")`.\
\
This works, but it bothers me that the function spits out "9 apple a day" as if plural words do not exist. If the number was anything but singular (1), then it should say "9 *apples* a day". Same should apply for the profession. To do this, I can introduce you to if statements in R.\
\
If statements are exactly what they sound like and are constructed similar to functions: `if(logical condition is TRUE){do something}`.\
\
We can use these within our `cheesy_function()`. We will also use the `paste()` function like above. Remember to type `?paste` if you're unfamiliar.


```r
# let's modify the code a bit
cheesy_function <- function(apple_num = 1, figure_num = 1, profession = "doctor"){
  
  # leave the first part the same
  # first, turn their number into a character with as.character
  apples <- as.character(apple_num) 
  figure <- as.character(figure_num)
  
  # check to see if apples is greater than 1
  if(apples > 1){
    # then the word we should use is "apples"
    fruit_word <- "apples" 
  } else {           
    # otherwise...
    fruit_word <- "apple"
  }
  
  # should do the same thing for profession
  if(figure > 1){
    # paste an "s" onto profession
    profession_word <- paste(profession,"s", sep = "")
  } else {
    # otherwise leave it
    profession_word <- profession
  }
  
  # now enter these variables into your print out
  print(paste(apples, fruit_word, "a day keeps", figure, profession_word, "away", sep = " "))
  
}


# now let's try it:
cheesy_function(9, 2)
```

```
## [1] "9 apples a day keeps 2 doctors away"
```


Great. Play around with that function. Try changing some things in it and seeing what happens. At this point, you should be ready to create some of your own!

### Loops
The dreaded loops. Tidyverse has optimized a LOT of work for data scientists, which means that we really don't need to use loops as often as we did. Though, when you need them, you'll know what to do. Loops can take several different forms. For our purposes, we can look at two cases: `for` loops and `while` loops.

#### For Loops
For loops are as their name suggests. They are used to iterate over certain values within a data structure and perform some operation. They're called for loops because they're read aloud as "**For** all values in this structure, perform some operation." For loops rely on our memory of indexing. If you're unfamiliar, check our indexing module.\
\
The basic structure of for loops are `for(some_value in 1:all_values){ do something }`. This reads "For an indexed value in 1 through all the values, do something".\
\
Let's suppose that we want to loop over all values of some vector, and print their value.\
\

```r
new_vector <- c(10, 40, 42, 19, 36, 11)

# open our for loop, and call our index 'i' - we could've called it anything but 'i' is convention
for(i in 1:6){
  # pull the value from new_vector and store it in 'value'
  value <- new_vector[i]
  
  print(paste("This particular value is", value))
}
```

```
## [1] "This particular value is 10"
## [1] "This particular value is 40"
## [1] "This particular value is 42"
## [1] "This particular value is 19"
## [1] "This particular value is 36"
## [1] "This particular value is 11"
```

I told R that it should loop 6 times `1:6` but that isn't very flexible. A new function, `length()` can tell us how long our vector is so we don't have to count.\


```r
# open our for loop, and call our index 'i' - we could've called it anything but 'i' is convention
for(i in 1:length(new_vector)){
  # pull the value from new_vector and store it in 'value'
  value <- new_vector[i]
  
  print(paste("This particular value is", value))
}
```

To tie these into our if statements, we could write a loop to print only the even numbers. To do this, we will have to think of a criteria that our even numbers fit that the others don't. We know that even numbers can be divided by two with a 0 remainder. Luckily, in R there is a special function to provide the remainder of a quotient, `%%`. We can take advantage of this logic within our loop.\


```r
# open our for loop, and call our index 'i' - we could've called it anything but 'i' is convention
for(i in 1:length(new_vector)){
  # pull the value from new_vector and store it in 'value'
  value <- new_vector[i]
  
  if(value %% 2 == 0){ # if the remainder is == 0
    print(paste("This particular value is", value))
  } else {
    next # else go to the next number
  }
  
}
```

```
## [1] "This particular value is 10"
## [1] "This particular value is 40"
## [1] "This particular value is 42"
## [1] "This particular value is 36"
```

See? Not so bad. But in most cases, we don't want to simply print a value to the console. Most of the time we want to store a value for further use. In this module, I'm only going to cover storage in vectors. But, because data frames are just bundled vectors, it translates easily.\
\
In keeping with the example above, instead of printing the values that were even, let us just store them in a vector. To do this, we will first need to define an empty vector. Then, upon each iteration, we will concatenate that vector with the newest value. At the end, we can just return the vector for use. Note that things assigned within the vector are saved into memory so... don't name things inside loops values that are assigned elsewhere. for example, we can see that if we type `value` into our console, the most recent number (the last in our loop) will print: 11.\
\

```r
# open an empty vector
even_vec <- c()
# open our for loop, and call our index 'i' - we could've called it anything but 'i' is convention
for(i in 1:length(new_vector)){
  # pull the value from new_vector and store it in 'value'
  value <- new_vector[i]
  
  if(value %% 2 == 0){ # if the remainder is == 0
    # override the even_vec with a combination of itseslf plus the new value
    even_vec <- c(even_vec, value)
  } else {
    next # else go to the next number
  }

}

# now we have access to that variable
even_vec
```

```
## [1] 10 40 42 36
```


I would encourage you to try this yourself!

#### While Loops
Our introduction to while loops will be brief, but they can be useful so I still wanted to provide some information. While loops, similar to for loops, do exactly what they sound like they do: **while** a logical expresssion evaluates to true, do something.\
\
Suppose we want to print all the values up to 10 multiplied by the number previous in the sequence? For example, if the number is 1, we would multiply 1 * (1-1), which would give us 0. The basic structure is `while(logical expression){do something}`. Because it implies iteration (while), we must assign next values of `i` on our own.\


```r
# tell the loop what the first value will be
i <- 1
while(i < 11){
  num <- i * (i-1)
  print(paste("Product: ", num))
  i <- i + 1
}
```

```
## [1] "Product:  0"
## [1] "Product:  2"
## [1] "Product:  6"
## [1] "Product:  12"
## [1] "Product:  20"
## [1] "Product:  30"
## [1] "Product:  42"
## [1] "Product:  56"
## [1] "Product:  72"
## [1] "Product:  90"
```

So this was pretty straightforward. Notice that I had to update `i` each loop. If I would not have, the loop would've extended forever. If this happens (and you will forget occassionally), there is a "stop" icon at the top right of the console pane in RStudio. Press that and your function will stop.\
\
Hopefully you now have a good understand of what functions are and how to use them, along with two different types of loops!


















