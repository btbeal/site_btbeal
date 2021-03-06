---
title: "Problem Set 2"
author: "Brennan T. Beal"
date: 2020-06-29T21:13:14-05:00
categories: ["R"]
draft: FALSE
tags: ["Practice", "R"]
---



For problem set two, we will be building on modules 0 - 3 and testing your skills of tidyverse, dates, strings, and plotting. So, if you are unfamiliar with those, take a look at the modules and come back.  
  
The entirety of this problem set will build off of the [COVID Apple Mobility data](https://www.apple.com/covid19/mobility). This data shows the percent of daily direction requests sent to the Apple servers for thousands of local regions by different types of transportation (walking, biking, and driving). To complete this lesson, you will need to download the data by clicking [this link](../../data/applemobilitytrends.csv).  
  
Our ultimate question today is two-fold:  
  
  1. What is the overall trend in mobility for Charlotte, NC?
  2. What day of the week are travelers most likely to travel, on average?

Let's get started!  
  
### Question 1
First, create a new project, save the data as "applemobilitytrends.csv", and store the data within the same folder that your project is located in. Then, read in the data like so:  

```r
# Your path will be different, remember
mobility <- read.csv("../../data/applemobilitytrends.csv")
```
  
Hopefully that part wasn't so bad. Now, let's take a look using our `str()` function.  
  

```r
str(mobility)
```
   
Yuck. Okay, don't panic. We've seen this before (remember our graphics tutorial and the income data?).
  
  1. Filter the mobility data so that the `region` is "Charlotte" and the `sub.region` is "North Carolina".
  2. How many transportation types are represented (hint: our `table()` function may be handy)
  3. Filter for the `transportation_type` "driving" (note, you can just place this within the first filter)

Now you've got what we know as a "wide" data set with (surprise) dates as columns and volumes (relative to the county baseline of 100%) as the column values.  
  
  4. Recalling what we learned in our graphics tutorial, use the `pivot_longer()` function to create a new `date` column and store the values from the previous columns in a new `volume` column. Remember that you can use `?pivot_longer()` to remind yourself of the arguments the function takes and that you can use `starts_with()` to select columns beginning with "X".  
  
 Awesome!  
   
Now we notice that this date column is actually a character vector. We need to change that character vector to a date vector. Though, to do that, we need to get rid of the "X" and "." values.
  
  5. Using what we learned with the stringr package, replace all the "X" and "." with "".
  6. Remembering the lubridate package, what does form is the date character in? y? mdy? ymd? Using your answer to that question, `mutate()` the date from a character class to a date class.
  
That was a lot of work. Normally, you would've done this all in one clean pipe so it didn't appear so messy. Small steps are better starting off so you can make sure each step is working properly. The final thing to do here is plot it!  
  
  7. Using `ggplot` and `geom_line()`, plot the line graph of volume over time (hint: place `aes(x = date, y = volume)` within `geom_line()`).
  
Pretty clear trend here... any guesses why?  
  
Now... about those those fluctuations.  
  
### Question 2
Now that we already have our data formatted in a way that is usable for ggplot, all that's left is a couple more data manipulation steps.  
  
  1. Using lubridate's `wday()` function, create another column labeled `weekday` from your `date` column.
  2. Create a new data frame of volume averages by day of the week using your new column.
  3. With your new data frame, create a bar graph with geom_bar and days of the week on the x-axis (remember that since geom_bars default stat is "count", you must specify `stat = "identity").
  4. Which day has the highest direction requests? The lowest?
  
This was a lot of work. So, if you made it this far, nice job! You should be really happy. If you struggled, don't worry. We all do. I still do. Remember that the `?` function is your friend as well as google!


