---
title: "Lesson 3 - Strings and Dates"
author: "Brennan T. Beal"
date: 2020-07-02T21:13:14-05:00
categories: ["R"]
draft: FALSE
tags: ["R Markdown", "Lessons", "Intro"]
---



Some of the most common data-related issues for those new to healthcare research (probably any research) revolve around string and date manipulation. Fortunately for us, R has some really incredible packages to help us deal with them. Like anything in R, it can be done in base R. Though, you'll never use base R again for string/date manipulation so... i'm not going to cover it here. Again, this is meant to be a broad overview of both of the packages so that you can go back to them when you're stuck!  

### Strings with Stringr Package
The first of the two packages we're going to discuss is the stringr package in R. Since you're an R veteran, you know how to install and upload this package using `install.packages()` and `library()`. That said, stringr is part of the tidyverse that we have already been introduced to. So, if you've installed that package, you have access to stringr (once you've called the library).  
  
Before we dive in, know that strings are a bit complex - [here](https://www.gastonsanchez.com/r4strings/) is an entire R book on dedicated to string manipulation in R. Like the rest of R, the most important skill is to understand when stringr can help, and then using their [**incredible** cheat sheet](https://github.com/rstudio/cheatsheets/blob/master/strings.pdf) when needed until you begin to memorize the functions (go to that page now and save it).  
  
Strings are any formulation of characters. Often times, we want to transform our data by a given string (think ICD-10 codes in medical data). To do this, we need to be able to get rid of strings we don't need, keep strings we do need, and sometimes, change all of our strings. To begin, let's look at the `fruit` vector that comes pre-loaded with stringr. Type `fruit` into your console to see.  
  
That's a lot of fruit. All stringr functions take the form of `str_*`. The function I use the most from stringr is the `str_detect()` function, which tells us whether a string contains a specific pattern or not. There is another function, `str_subset()`, that is a wrapper function for `str_detect()`, but I like `str_detect()` because it has more flexible applications. The function takes a string vector (such as fruit) and a pattern and returns TRUE or FALSE. For example, suppose I wanted every fruit that contains a "q"?  


```r
# Note, str_detect returns T/F... 
# Luckily, we know how to index based on the TRUE values
fruit[str_detect(fruit, "q")]
```

```
## [1] "kumquat" "loquat"  "quince"
```

Usually, we want to know more about the string than whether or not it just contains a "q". Often, we want to be more specific about the pattern we are providing. What R is really doing is matching patterns based on regular expressions (commonly read as regexp). These are really intimidating looking patterns that provide more precision about what qualifying strings should look like. For example, suppose we want all strings that *begin* with "y"? We can use the common `^` anchor to specify that.  


```r
fruit[str_detect(fruit, "^q")]
```

```
## [1] "quince"
```

The other anchor is `$`. So, we could also have tried to find all the fruits that end with "q". Since no fruits end in "q", try it with the letter "a": `fruit[str_detect(fruit, "a$")]`. The second page of the [stringr cheat sheet](https://github.com/rstudio/cheatsheets/blob/master/strings.pdf) that I referenced above provides detail on how to construct these values. Just know that you can get very specific with the place and structure of a pattern you want to reference with regular expressions. To test if the pattern you wrtie matches as you'd have hoped, I usually use this [website](https://regex101.com/).  
  
A couple more things about `str_detect()` that may be useful. First, all stringr functions are case specific. This is important because if you ask R to detect all patterns with "y" in them, they will only return lowercase "y" matches. For example, try using our example above: `fruit[str_detect(fruit, "^Q")]`. It tells you that no patterns match. Second, all stringr functions (including detect) can all be used within tidyvese piping to manipulate data frames. Let's see an example of both of these things in action using the mtcars data.  


```r
mtcars %>% 
  # convert the rownames to a column named Car
  rownames_to_column(var = "Car") %>% 
  # return all cars that contain an "m"
  filter(str_detect(Car, "m"))
```

```
##                 Car  mpg cyl disp  hp drat    wt  qsec vs am gear carb
## 1 Chrysler Imperial 14.7   8  440 230 3.23 5.345 17.42  0  0    3    4
## 2        Camaro Z28 13.3   8  350 245 3.73 3.840 15.41  0  0    3    4
```
  
Notice that this is only the values of our new `Car` variable that contains a lowercase "m"? There are two ways to get around this. First, we could use the base R function `tolower()` to convert all the values (temporarily) to lowercase and then match based on that. Then, the line would read something like `filter(str_detect(tolower(Car), "m"))`. This is not very neat - now we have 3 functions nested within each other. Of course, we know about regular expressions and so that is probably the better way to go. Regular expressions allow you to tell R to match any one of what is in brackets, `[]`. Take a look:  
  

```r
mtcars %>% 
  # convert the rownames to a column named Car
  rownames_to_column(var = "Car") %>% 
  # return all cars that contain an "m" or "M"
  filter(str_detect(Car, "[Mm]"))
```

```
##                  Car  mpg cyl  disp  hp drat    wt  qsec vs am gear carb
## 1          Mazda RX4 21.0   6 160.0 110 3.90 2.620 16.46  0  1    4    4
## 2      Mazda RX4 Wag 21.0   6 160.0 110 3.90 2.875 17.02  0  1    4    4
## 3          Merc 240D 24.4   4 146.7  62 3.69 3.190 20.00  1  0    4    2
## 4           Merc 230 22.8   4 140.8  95 3.92 3.150 22.90  1  0    4    2
## 5           Merc 280 19.2   6 167.6 123 3.92 3.440 18.30  1  0    4    4
## 6          Merc 280C 17.8   6 167.6 123 3.92 3.440 18.90  1  0    4    4
## 7         Merc 450SE 16.4   8 275.8 180 3.07 4.070 17.40  0  0    3    3
## 8         Merc 450SL 17.3   8 275.8 180 3.07 3.730 17.60  0  0    3    3
## 9        Merc 450SLC 15.2   8 275.8 180 3.07 3.780 18.00  0  0    3    3
## 10 Chrysler Imperial 14.7   8 440.0 230 3.23 5.345 17.42  0  0    3    4
## 11       AMC Javelin 15.2   8 304.0 150 3.15 3.435 17.30  0  0    3    2
## 12        Camaro Z28 13.3   8 350.0 245 3.73 3.840 15.41  0  0    3    4
## 13     Maserati Bora 15.0   8 301.0 335 3.54 3.570 14.60  0  1    5    8
```

That looks a bit cleaner, right?  
  
Now that you've got that down, I only want to cover one more tool that I use pretty often: `str_replace()`. Again, clue is in the function name - this function is useful when we want to replace one pattern with another. Let's take a look at the first 10 sentences in the `sentences` vector that comes with the strigr package.  
  

```r
sent_subset <- sentences[1:10]
sent_subset
```

```
##  [1] "The birch canoe slid on the smooth planks." 
##  [2] "Glue the sheet to the dark blue background."
##  [3] "It's easy to tell the depth of a well."     
##  [4] "These days a chicken leg is a rare dish."   
##  [5] "Rice is often served in round bowls."       
##  [6] "The juice of lemons makes fine punch."      
##  [7] "The box was thrown beside the parked truck."
##  [8] "The hogs were fed chopped corn and garbage."
##  [9] "Four hours of steady work faced us."        
## [10] "Large size in stockings is hard to sell."
```
  
Now, suppose we want to replace all the "e" values with "-" - we could do something like:  
  

```r
str_replace(sent_subset, "e", "-")
```

```
##  [1] "Th- birch canoe slid on the smooth planks." 
##  [2] "Glu- the sheet to the dark blue background."
##  [3] "It's -asy to tell the depth of a well."     
##  [4] "Th-se days a chicken leg is a rare dish."   
##  [5] "Ric- is often served in round bowls."       
##  [6] "Th- juice of lemons makes fine punch."      
##  [7] "Th- box was thrown beside the parked truck."
##  [8] "Th- hogs were fed chopped corn and garbage."
##  [9] "Four hours of st-ady work faced us."        
## [10] "Larg- size in stockings is hard to sell."
```
  
But this only replaces the first matched pattern. So, we can use `str_replace_all()` to... replace all of them. 
  

```r
str_replace_all(sent_subset, "e", "-")
```

```
##  [1] "Th- birch cano- slid on th- smooth planks." 
##  [2] "Glu- th- sh--t to th- dark blu- background."
##  [3] "It's -asy to t-ll th- d-pth of a w-ll."     
##  [4] "Th-s- days a chick-n l-g is a rar- dish."   
##  [5] "Ric- is oft-n s-rv-d in round bowls."       
##  [6] "Th- juic- of l-mons mak-s fin- punch."      
##  [7] "Th- box was thrown b-sid- th- park-d truck."
##  [8] "Th- hogs w-r- f-d chopp-d corn and garbag-."
##  [9] "Four hours of st-ady work fac-d us."        
## [10] "Larg- siz- in stockings is hard to s-ll."
```

Now what if we wanted to remove, instead of replace, all vowels in any position?  
  

```r
# remember our bracket trick?
# Note that we can remove something by similar convention
str_remove_all(sent_subset, "[aeiou]")
```

```
##  [1] "Th brch cn sld n th smth plnks."  "Gl th sht t th drk bl bckgrnd."  
##  [3] "It's sy t tll th dpth f  wll."    "Ths dys  chckn lg s  rr dsh."    
##  [5] "Rc s ftn srvd n rnd bwls."        "Th jc f lmns mks fn pnch."       
##  [7] "Th bx ws thrwn bsd th prkd trck." "Th hgs wr fd chppd crn nd grbg." 
##  [9] "Fr hrs f stdy wrk fcd s."         "Lrg sz n stckngs s hrd t sll."
```

And now, finally, we can create something that looks like your econometric textbook as so:  


```r
weird_sentences <- str_remove_all(sent_subset, "[aeiou]")
# remove any space, period, comma, or apostrophe
str_remove_all(weird_sentences, "[ .,']")
```

```
##  [1] "Thbrchcnsldnthsmthplnks"  "Glthshttthdrkblbckgrnd"  
##  [3] "Itssyttllthdpthfwll"      "Thsdyschcknlgsrrdsh"     
##  [5] "Rcsftnsrvdnrndbwls"       "Thjcflmnsmksfnpnch"      
##  [7] "Thbxwsthrwnbsdthprkdtrck" "Thhgswrfdchppdcrnndgrbg" 
##  [9] "Frhrsfstdywrkfcds"        "Lrgsznstckngsshrdtsll"
```
  
That is a fairly useles result... but hopefully you've at least seen some of the things that we can do with the stringr package! Of course, there are many more functions that you will find useful. The main goal here was an introduction. You know where to look though, if you need help. If you're having string issues, stringr can likely help.  
  
Now, about those dates.  
  
### Dates with Lubridate Package
Dates, like strings, are fairly complex. But dates are complex due to the nature of the way humans track time, which makes doing arithmetic pretty challenging. The R for Data Science book sums it up pretty well by asking a few simple questions:

  1. Does every year have 365 days?
  2. Does every day have 24 hours?
  3. Does every minute have 60 seconds?

In short, the answer is "no" to all of the above. So, for that reason alone, arithmetic gets complex. Consider if we wanted to look at a date two months from our current date? Well... do we mean 60 days? Do we mean that we just want to add two to our month? Some months are longer than others, remember. Luckily for us, the lubridate package has provided a nice set of functions to help us deal with dates in a sensible way. And even though we won't be digging into all the nuances above, it is really important to keep them in mind while you're working with dates!  
  
#### The Basics
Let's start with the most basic task that you'll have when working with dates: making dates into... dates. Most of the time, you will be provided with some character or numeric variable that is meant to represent dates but doesn't. So, to do any date manipulation, we must first specify to R that we are actually working with them. To do this, lubridate provides some pretty useful tools. To specify that you want a value to become a date, simply supply what form the values take as a function. Here are a few examples. Type these into your console:  
  

```r
# these are in year-month-date format
# So, we use ymd()
ymd("2011-12-30")
ymd(20111230)
ymd("11 Dec. 30th")


# These are in date month year format
# So...
dmy("30-12-11")
dmy(30122011)

# and then this wonky thing...
dmy("30th Dec        2011")
```

There are others as well, which you can find on the [lubridate cheat sheet](https://rawgit.com/rstudio/cheatsheets/master/lubridate.pdf). Notice that the functions can take some pretty wild structures and return something sensible. Once you have a date object, you can then pull relevant information from it by specifying the part of the date you're concerned with.


```r
my_date <- ymd("2011-12-30")
year(my_date)
```

```
## [1] 2011
```

  
This works for other units as well. Try `month(my_date)`. Or, if we wanted which day of the week: `wday(my_date)`. Lubridate, like specifying a date, has a lot of neat functions for pulling information from a date object similar to what we just did.  
  
#### Arithmetic
Dates can be strange given all the intricacies we noted at the beginning. For this reason, lubridate has provided three different time structures for us to use so that we can specify how we want R to handle our dates.  
  
The first, and most simple, is a **duration**. Durations help us understand the exact time elapsed, in seconds, between two dates or during a number of dates. Lubridate specifies seconds because it is the only consistent time unit that we reference (as opposed to years, months, or days). To see the basic form of this, we can do something similar to above by typing:  


```r
ddays(12)
dhours(12)

#... etc.
```
  
You can use this to add or subtract some unit of dates. For example, suppose we want to add 12 days to our date, `my_date`, which was created above:  


```r
my_date + ddays(12)
```

```
## [1] "2012-01-11"
```

That is about as easy as it comes. You can do this with other units of time as well. But, as mentioned above, there are times (literally) where this may not be what you're looking for. From the R for Data Science book, consider that our date of interest was March 12th at 1pm - let's take a look:  


```r
weird_date <- ymd_hms("2016-03-12 13:00:00", tz = "America/New_York")
weird_date + ddays(1)
```

```
## [1] "2016-03-13 14:00:00 EDT"
```

Somehow, we added a day's worth of seconds but gained an extra hour. We can also see that our time zones have changed. The duration functions are literal... and so if you want a full days worth of seconds, that is what you'll get. To account for things like this (and maybe fix some problems you may run into), lubridate also has **periods**.  
  
Periods work in a slightly more intuitive way and try to get at what most people mean when they simply want to add a day. From the example provided above:  


```r
weird_date + days(1)
```

```
## [1] "2016-03-13 13:00:00 EDT"
```

These functions work in a similar manner... but are just more likely to produce the results anticipated. In general, you should use the simplest functions to produce what you need.  
  
Finally, we have **intervals**. Intervals are exactly what they sound like and are useful for determining how many "durations" or "periods" fall within specific intervals. Usually, we can get intuitive results from dividing the classes above. But, intervalls can help to be more specific and can be specified by the `%--%` operator. For example, suppose we want to know how many days (inclusive) are between December 22nd, 2018, and August 13th, 2020? We would first specify an interval:  


```r
# Specify the interval with the %--% operator
my_interval <- ymd("2018-12-22") %--% dmy("13-08-2020")

# then divide:
my_interval/days(1)
```

```
## [1] 600
```


There are many more impressive functions that we can use with dates and strings in R but this will serve as a crash course. Now you know the basics of stringr and lubridate as well as where to find the full information should you need it!
