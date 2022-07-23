## Introduction to R

This article goes through some basic `R` operations and a common data
analysis workflow using `tidyverse`. First of all, make sure you have
[R](https://cran.microsoft.com/) and [RStudio](https://www.rstudio.com/)
installed. No prior knowledge of coding or R is needed!

The first step is to load in the required packages. Also, don’t forget
to set your working directory (basically the folder the code will live
in) with the `setwd` function! You can do this easily on RStudio with
Session -\> Set Working Directory

``` r
#you can  set working directory using setwd. This is the path to my directory, so it won't work on your console!!
#setwd("~/Dropbox (MIT)/MIT Stuff/Fall 2022/quant_1_ta")

#loading required packages - use install.packages() to install these packages on your R
library(tidyverse)
library(palmerpenguins)
library(gt)
library(data.table)
library(WDI)
library(broom)
```

## Basic R Operations

Here’s a short overview of basic operations in `R`

Arithmetic:

``` r
1+1
```

    ## [1] 2

Logical operations:

``` r
1:10>4
```

    ##  [1] FALSE FALSE FALSE FALSE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE

``` r
#1:10 means 1 to 10
```

Print text

``` r
print('hello world')
```

    ## [1] "hello world"

Assign values to objects with `<-`

``` r
x1 <- sample(1:10, 100, replace = T) 
```

Inspect objects

``` r
head(x1)
```

    ## [1] 6 9 1 5 9 5

``` r
tail(x1)
```

    ## [1] 8 4 1 9 5 2

Subset elements within an object, in this case from the vector `x1`

``` r
x1[3] 
```

    ## [1] 1

``` r
x1[2:3]
```

    ## [1] 9 1

Combine subsetting and assigning to assign new values

``` r
#assign new values to subsetted data
x1[3] <- 8

#inspecting to check that we did asssign the third element of x1 as 8
head(x1)
```

    ## [1] 6 9 8 5 9 5

What did these lines of code do? First we sampled 100 numbers from 1 to
10 with replacement with the `sample` function, and assigned it as a
vector `x1`. We then inspected the start and end of that vector using
the `head` and `tail` functions. We then subsetted for specific elements
in `x1`, and reassigned the third element as 8. It’s always helpful to
spell out exactly what you’re trying to do first, break it down into
steps, and then convert it to code! Also, it’s always a good idea to
look up the documentation of an unfamiliar function by calling ? plus
the function name. Try `?head` in your console!

Another common operation is `if` `else` statements. `if` statements can
be standalone:

``` r
x <- 5
if (x >3){ #condition in brackets
  print("Larger than 3") #what to do if condition is fulfilled
}
```

    ## [1] "Larger than 3"

Or they can be combined with `else` statements

``` r
if (x >= 4){ #condition
  print('large') #what to do if condition fulfilled 
} else{ #what to do it condition not fulfilled
  print('small')
}
```

    ## [1] "large"

For simple logic, you can combine `if` and `else` statements into
`ifelse`. Look up `?ifelse` in your console to see how it works!

``` r
ifelse(x >= 4, 'large', 'small')
```

    ## [1] "large"

``` r
ifelse(x > 9, 'large', 'small')
```

    ## [1] "small"

## Loops

You’ll hear people talk about loops a lot - it’s one of the most common
structures used in `R`, and it might seem a bit confusing at first.
Don’t worry!

At its base, a loop is a set of instructions to keep doing to something
each element in an object.

``` r
x2 <- c() #creating empty vector outside of loop to fill up
for (i in 1:100){
  x2[i] <- x1[i] + 1
}

#inspecting
head(x1)
```

    ## [1] 6 9 8 5 9 5

``` r
head(x2)
```

    ## [1]  7 10  9  6 10  6

In this loop, we first created an empty vector `x2` , and created a
`for` loop, saying for each element `i` (you don’t have to name it `i`
necessarily, could be any letter) from 1 to 100, take the `i`th element
of `x1`. add 1 to it, and save it as the `i`th element of `x2`.

Of course, we didn’t need a `for` loop to do this! We could have simply
done `x1 + 1`, which would have added 1 to each element of `x1`.

``` r
x3 <- x1 + 1

identical(x2, x3) 
```

    ## [1] TRUE

``` r
#identical function checks whether two objects are identical
```

Sometimes we use nested loops (loop inside a loop):

``` r
for (k in 1:3) { #first layer loops through k
for (l in 1:2) { #for each element in k, loop through l 
print(paste("k =", k, "l= ",l))
}
}
```

    ## [1] "k = 1 l=  1"
    ## [1] "k = 1 l=  2"
    ## [1] "k = 2 l=  1"
    ## [1] "k = 2 l=  2"
    ## [1] "k = 3 l=  1"
    ## [1] "k = 3 l=  2"

## Apply Functions

The `apply` family of functions are also very common in `R`. These
examples are taken from this great
[introduction](https://bookdown.org/manishpatwal/bookdown-demo/apply-family-functions.html)
to R, which you are encouraged to dig further into.

``` r
## Lets create a matrix M1 with 5 rows and 3 columns
M1 <- matrix(data = (1:15),nrow=5)
M1
```

    ##      [,1] [,2] [,3]
    ## [1,]    1    6   11
    ## [2,]    2    7   12
    ## [3,]    3    8   13
    ## [4,]    4    9   14
    ## [5,]    5   10   15

We created a 5x3 matrix — which is a type of `R` object — consisting of
numbers from 1 to 15. If we wanted to get the column sums of `M1`, we
can easily do this with an `apply` function.

First, let’s see how to subset from a matrix and check its dimensions
(always a good idea to check dimensions when working with data)

``` r
#dimensions of M1
dim(M1)
```

    ## [1] 5 3

``` r
#subsetting first row, keeping all columns
M1[1,]
```

    ## [1]  1  6 11

``` r
#subsetting first col, keeping all rows
M1[,1]
```

    ## [1] 1 2 3 4 5

Now using the `apply` function, we can get the column sums of `M1`.

``` r
M1_colsum<-apply(M1,2,sum)
M1_colsum
```

    ## [1] 15 40 65

What happened here? Call `?apply` to look at the documentation. Notice
that the function takes 3 arguments, `X`, `MARGIN`, and `FUN`. Here, `X`
is just the matrix `M1`. `MARGIN` refers to how the function should be
applied - 1 for row-wise, 2 for column-wise. `FUN` refers to the
function to be applied, which is `sum` in this example.

Moving onto the next member of the family `lapply` - the l stands for
*list* apply - applies a function to each element of a list, and returns
a list. Let’s create a list first:

``` r
tas <- list(list(name = '   rEgGrEssIon coNnoisSeuR ', fields = c('SS, PE')), 
                 list(name = ' sTat cAt  ', fields = c('PE, MM')),
                 c(name = 'tIm the beAver'),
                 rnorm(10))
```

Here we have a `list` containing information about your imaginary TAs,
but your TA (me) was distracted and input the names with whitespace (the
spaces before and after a string) and with incomprehensible
capitalization, and threw in some irrelevant data for good measure!
Notice that a `list` object can take elements of different types and
dimensions - you can inspect it in RStudio.

First, let’s take a look at the structure of `tas` with the `str`
function:

``` r
str(tas)
```

    ## List of 4
    ##  $ :List of 2
    ##   ..$ name  : chr "   rEgGrEssIon coNnoisSeuR "
    ##   ..$ fields: chr "SS, PE"
    ##  $ :List of 2
    ##   ..$ name  : chr " sTat cAt  "
    ##   ..$ fields: chr "PE, MM"
    ##  $ : Named chr "tIm the beAver"
    ##   ..- attr(*, "names")= chr "name"
    ##  $ : num [1:10] 0.9458 -0.617 0.2547 -0.8487 -0.0758 ...

We only want the first 2 elements of the list! Since we want the output
to still be a list, we subset it with single square brackets `[` . This
will became important later.

``` r
tas_2 <- tas[1:2]
class(tas_2)
```

    ## [1] "list"

But we still need to clean the data! This is where `lapply` comes in.
`lapply` takes a list and applies a function to each element. It
*always* returns a list. We encourage you to go through the following
code line by line and look up any unfamiliar functions, and translate
what’s going on into plain English. The `$` operator is used to extract
data from lists or dataframes. As a bonus exercise, check out the
documentation for `sapply` (*simple* apply) and try to understand what
happens when we use `sapply` instead of `lapply` in the cleaning step.

``` r
#cleaning names
ta_names_clean <- lapply(tas_2, function(x){
  ta_names <- tolower(x$name) #converting names to lowercase
  ta_names_trim <- str_trim(ta_names) #trimming whitespace
  ta_names_trim
})

#replacing with cleaned names
for (i in 1:2){
  tas_2[[i]]$name = ta_names_clean[[i]]
} 

str(tas_2)
```

    ## List of 2
    ##  $ :List of 2
    ##   ..$ name  : chr "reggression connoisseur"
    ##   ..$ fields: chr "SS, PE"
    ##  $ :List of 2
    ##   ..$ name  : chr "stat cat"
    ##   ..$ fields: chr "PE, MM"

Notice the function syntax - we will be writing a lot of our own
functions in `R`. For example, the following (pretty useless) function
takes in two arguments `x` and `y` and sums them.

``` r
useless <- function(x, y){ #function that takes two inputs x and y
  x + y #what should the function do with arguments
}
useless(2,4) 
```

    ## [1] 6

As a more advanced extension, we could have achieved the same result
using `purrr` with more succinct (and more readable) code. `purrr` is
part of the `tidyverse` and is a package for functional programming —
you can read more about it
[here](https://purrr.tidyverse.org/index.html). No need to worry about
this for Quant 1!

``` r
#Writing helper function to combine tolower and trim
f_helper <- function(x){
  x %>% tolower() %>% 
    str_trim()
}
#modifying nested lists using modify family from purrr
tas_2 %>% modify_depth(1, ~modify_at(., 'name', f_helper))
```

    ## [[1]]
    ## [[1]]$name
    ## [1] "reggression connoisseur"
    ## 
    ## [[1]]$fields
    ## [1] "SS, PE"
    ## 
    ## 
    ## [[2]]
    ## [[2]]$name
    ## [1] "stat cat"
    ## 
    ## [[2]]$fields
    ## [1] "PE, MM"

When subsetting lists, remember to distinguish between the single and
double brackets `[]` and `[[]]`. Single brackets return a list, while
the double brackets return the element in the list in its original
object type. This might sound pedantic, but it matters because the type
of object affects what type of operation you can do to it:

``` r
tas_2[1]$name
```

    ## NULL

``` r
tas_2[[1]]$name
```

    ## [1] "reggression connoisseur"

Finally, after working with lists, we often like to convert lists into a
dataframe! `rbindlist` is a nifty function for this. Don’t worry about
the pipe operator `%>%` just yet, we’ll cover it later.

``` r
df <- rbindlist(tas_2)
gt(df) %>% tab_header(title = 'TA info')
```

<div id="zpjujqdfqi" style="overflow-x:auto;overflow-y:auto;width:auto;height:auto;">
<style>html {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Helvetica Neue', 'Fira Sans', 'Droid Sans', Arial, sans-serif;
}

#zpjujqdfqi .gt_table {
  display: table;
  border-collapse: collapse;
  margin-left: auto;
  margin-right: auto;
  color: #333333;
  font-size: 16px;
  font-weight: normal;
  font-style: normal;
  background-color: #FFFFFF;
  width: auto;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #A8A8A8;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #A8A8A8;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
}

#zpjujqdfqi .gt_heading {
  background-color: #FFFFFF;
  text-align: center;
  border-bottom-color: #FFFFFF;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#zpjujqdfqi .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#zpjujqdfqi .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 0;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#zpjujqdfqi .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#zpjujqdfqi .gt_col_headings {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#zpjujqdfqi .gt_col_heading {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  overflow-x: hidden;
}

#zpjujqdfqi .gt_column_spanner_outer {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  padding-top: 0;
  padding-bottom: 0;
  padding-left: 4px;
  padding-right: 4px;
}

#zpjujqdfqi .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#zpjujqdfqi .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#zpjujqdfqi .gt_column_spanner {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 5px;
  overflow-x: hidden;
  display: inline-block;
  width: 100%;
}

#zpjujqdfqi .gt_group_heading {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
}

#zpjujqdfqi .gt_empty_group_heading {
  padding: 0.5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: middle;
}

#zpjujqdfqi .gt_from_md > :first-child {
  margin-top: 0;
}

#zpjujqdfqi .gt_from_md > :last-child {
  margin-bottom: 0;
}

#zpjujqdfqi .gt_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  margin: 10px;
  border-top-style: solid;
  border-top-width: 1px;
  border-top-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  overflow-x: hidden;
}

#zpjujqdfqi .gt_stub {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
}

#zpjujqdfqi .gt_stub_row_group {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
  vertical-align: top;
}

#zpjujqdfqi .gt_row_group_first td {
  border-top-width: 2px;
}

#zpjujqdfqi .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#zpjujqdfqi .gt_first_summary_row {
  border-top-style: solid;
  border-top-color: #D3D3D3;
}

#zpjujqdfqi .gt_first_summary_row.thick {
  border-top-width: 2px;
}

#zpjujqdfqi .gt_last_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#zpjujqdfqi .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#zpjujqdfqi .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#zpjujqdfqi .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#zpjujqdfqi .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#zpjujqdfqi .gt_footnotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#zpjujqdfqi .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding-left: 4px;
  padding-right: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#zpjujqdfqi .gt_sourcenotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#zpjujqdfqi .gt_sourcenote {
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#zpjujqdfqi .gt_left {
  text-align: left;
}

#zpjujqdfqi .gt_center {
  text-align: center;
}

#zpjujqdfqi .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#zpjujqdfqi .gt_font_normal {
  font-weight: normal;
}

#zpjujqdfqi .gt_font_bold {
  font-weight: bold;
}

#zpjujqdfqi .gt_font_italic {
  font-style: italic;
}

#zpjujqdfqi .gt_super {
  font-size: 65%;
}

#zpjujqdfqi .gt_two_val_uncert {
  display: inline-block;
  line-height: 1em;
  text-align: right;
  font-size: 60%;
  vertical-align: -0.25em;
  margin-left: 0.1em;
}

#zpjujqdfqi .gt_footnote_marks {
  font-style: italic;
  font-weight: normal;
  font-size: 75%;
  vertical-align: 0.4em;
}

#zpjujqdfqi .gt_asterisk {
  font-size: 100%;
  vertical-align: 0;
}

#zpjujqdfqi .gt_slash_mark {
  font-size: 0.7em;
  line-height: 0.7em;
  vertical-align: 0.15em;
}

#zpjujqdfqi .gt_fraction_numerator {
  font-size: 0.6em;
  line-height: 0.6em;
  vertical-align: 0.45em;
}

#zpjujqdfqi .gt_fraction_denominator {
  font-size: 0.6em;
  line-height: 0.6em;
  vertical-align: -0.05em;
}
</style>
<table class="gt_table">
  <thead class="gt_header">
    <tr>
      <th colspan="2" class="gt_heading gt_title gt_font_normal gt_bottom_border" style>TA info</th>
    </tr>
    
  </thead>
  <thead class="gt_col_headings">
    <tr>
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1">name</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1">fields</th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr><td class="gt_row gt_left">reggression connoisseur</td>
<td class="gt_row gt_left">SS, PE</td></tr>
    <tr><td class="gt_row gt_left">stat cat</td>
<td class="gt_row gt_left">PE, MM</td></tr>
  </tbody>
  
  
</table>
</div>

We can easily extract data from dataframes using the `$` operator. You
can also combine the `$` with the `[]` operator to subset further
(conditional subsetting):

``` r
df$name
```

    ## [1] "reggression connoisseur" "stat cat"

``` r
df$fields[df$name == 'stat cat']
```

    ## [1] "PE, MM"

Quick recap on what we’ve covered so far

1.  Common objects such as vectors, lists, matrices and dataframes

2.  How to subset from these objects

3.  Loops

4.  Some of the `apply` family

You might have noticed when looking up `?apply` that there is another
member of the family - `tapply`. Before we get to that, let’s get some
data!

## Loading Data

Here we will use data from the `palmerpenguins` package, which we can
directly access by calling `penguins`, since we already installed the
package:

``` r
data("penguins")
```

Most of the time, we have to load our own data, usually in either a
`csv` or `xlsx` format. You can do so by using the `read_csv` or
`read_excel`. RStudio does this automatically when you use “Import
Dataset”. Conversely, you can write csv files from dataframes in `R`.

``` r
## NOT RUN 
write.csv(penguins, file = 'penguins.csv') #writing csv files from df into working directory
same_penguin <- read_csv('penguins.csv', show_col_types = F) #reading
```

Other common data types include `RData` or `RDS` files, which contain
`R` objects (`RData` can store multiple `R` objects while `RDS` only
stores one).

``` r
## NOT RUN
save(tas, tas_2, file = "ta_stuff.RData") 
load("ta_stuff.RData")
```

It is always a good idea to first inspect a dataset before using it,
even if you compiled it yourself:

``` r
str(penguins)
```

    ## tibble [344 × 8] (S3: tbl_df/tbl/data.frame)
    ##  $ species          : Factor w/ 3 levels "Adelie","Chinstrap",..: 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ island           : Factor w/ 3 levels "Biscoe","Dream",..: 3 3 3 3 3 3 3 3 3 3 ...
    ##  $ bill_length_mm   : num [1:344] 39.1 39.5 40.3 NA 36.7 39.3 38.9 39.2 34.1 42 ...
    ##  $ bill_depth_mm    : num [1:344] 18.7 17.4 18 NA 19.3 20.6 17.8 19.6 18.1 20.2 ...
    ##  $ flipper_length_mm: int [1:344] 181 186 195 NA 193 190 181 195 193 190 ...
    ##  $ body_mass_g      : int [1:344] 3750 3800 3250 NA 3450 3650 3625 4675 3475 4250 ...
    ##  $ sex              : Factor w/ 2 levels "female","male": 2 1 1 NA 1 2 1 2 NA NA ...
    ##  $ year             : int [1:344] 2007 2007 2007 2007 2007 2007 2007 2007 2007 2007 ...

We can see that there are 344 observations with 8 covariates, with some
missing values. We can also see that three variables are factor
(categorical) variables. Always ask what each column means (What unit?
What scale? etc.) This isn’t a problem with the `penguins` dataset, but
sometimes things can go wrong when importing data, such as `R` thinking
a number is a character!

``` r
x <- 1:3
class(x)
```

    ## [1] "integer"

``` r
x+1
```

    ## [1] 2 3 4

``` r
x <- c(1.0, 2.0, 3.0)
class(x)
```

    ## [1] "numeric"

``` r
x+1
```

    ## [1] 2 3 4

``` r
x <- c('1', '2', '3')
class(x) 
```

    ## [1] "character"

``` r
x+1 #this won't work!
```

    ## Error in x + 1: non-numeric argument to binary operator

``` r
#converting between different formats
x <- as.numeric(x) #taking character vector and converting to numeric
x+1
```

    ## [1] 2 3 4

``` r
#forcing integer
as.integer(c(1.1, 2.2, 3.3))
```

    ## [1] 1 2 3

We can also find out how many incomplete entries (cells with `NA`) and
where they are:

``` r
which(is.na(penguins), arr.ind = T) #index of nas
```

    ##       row col
    ##  [1,]   4   3
    ##  [2,] 272   3
    ##  [3,]   4   4
    ##  [4,] 272   4
    ##  [5,]   4   5
    ##  [6,] 272   5
    ##  [7,]   4   6
    ##  [8,] 272   6
    ##  [9,]   4   7
    ## [10,]   9   7
    ## [11,]  10   7
    ## [12,]  11   7
    ## [13,]  12   7
    ## [14,]  48   7
    ## [15,] 179   7
    ## [16,] 219   7
    ## [17,] 257   7
    ## [18,] 269   7
    ## [19,] 272   7

``` r
sum(is.na(penguins)) #how many cells w/ nas
```

    ## [1] 19

We can also drop rows containing `NA` with `drop_na`.

``` r
dim(drop_na(penguins)) #333 rows instead of 344
```

    ## [1] 333   8

## Manipulating Data

Having inspected the data, let’s try to generate some descriptive
statistics. Here is where `tapply` comes in handy. Let’s say we want to
find the average bill length of each species. `tapply` applies a
function to each group that is identified by the `INDEX` argument, which
in this case is a vector of each penguin’s species.

``` r
tapply(penguins$bill_length_mm, penguins$species, mean, na.rm = T) 
```

    ##    Adelie Chinstrap    Gentoo 
    ##  38.79139  48.83382  47.50488

``` r
#na.rm = T means we drop observations with NA
```

Alternatively, we can do this using `dplyr`, which is part of the
`tidyverse`.

``` r
penguins %>% group_by(species) %>% summarise(m_bill_length = mean(bill_length_mm, na.rm = T))
```

    ## # A tibble: 3 × 2
    ##   species   m_bill_length
    ##   <fct>             <dbl>
    ## 1 Adelie             38.8
    ## 2 Chinstrap          48.8
    ## 3 Gentoo             47.5

The pipe operator `%>%` is widely used in `tidyverse`, roughly meaning
“take the output and ‘pipe’ it into this next step”. The [keyboard
shortcut](https://support.rstudio.com/hc/en-us/articles/200711853-Keyboard-Shortcuts-in-the-RStudio-IDE)
is Cmd + Shift + M. For example, in the code above, we took the
`penguins` dataframe and piped it into the `group_by` function, which
groups the data according to a specified variable of interest (in this
case species), and that gets piped into the `summarise` function, which
we use to calculate the mean of bill length.

Other common operations include `mutate`, which creates new variables;
`filter`, `rename`, and `select`. It is also useful to combine these
with the `across` function, which applies the same transformation to
multiple columns. We encourage you to check out the documentation and
examples [here](https://dplyr.tidyverse.org/).

A simple example for `select` and `mutate`, where we get the ratio of
bill length to depth:

``` r
df_ratio <- penguins %>% select(species, bill_length_mm, bill_depth_mm) %>% 
  mutate(l_d_ratio = bill_length_mm/bill_depth_mm) %>% 
  drop_na()
head(df_ratio)
```

    ## # A tibble: 6 × 4
    ##   species bill_length_mm bill_depth_mm l_d_ratio
    ##   <fct>            <dbl>         <dbl>     <dbl>
    ## 1 Adelie            39.1          18.7      2.09
    ## 2 Adelie            39.5          17.4      2.27
    ## 3 Adelie            40.3          18        2.24
    ## 4 Adelie            36.7          19.3      1.90
    ## 5 Adelie            39.3          20.6      1.91
    ## 6 Adelie            38.9          17.8      2.19

You can also create a new column using the `$` operator

``` r
df_ratio$new <- 1 #adding a new column of 1s
head(df_ratio)
```

    ## # A tibble: 6 × 5
    ##   species bill_length_mm bill_depth_mm l_d_ratio   new
    ##   <fct>            <dbl>         <dbl>     <dbl> <dbl>
    ## 1 Adelie            39.1          18.7      2.09     1
    ## 2 Adelie            39.5          17.4      2.27     1
    ## 3 Adelie            40.3          18        2.24     1
    ## 4 Adelie            36.7          19.3      1.90     1
    ## 5 Adelie            39.3          20.6      1.91     1
    ## 6 Adelie            38.9          17.8      2.19     1

Now for a slightly more complicated example:

``` r
df_out <- penguins %>% filter(sex == 'male') %>% group_by(island) %>% 
  summarize(across(.cols = starts_with('bill'), .fns = list(mean = mean, sd = sd), .names = "{.fn}_{.col}"))
gt(df_out)
```

<div id="svpvbgybor" style="overflow-x:auto;overflow-y:auto;width:auto;height:auto;">
<style>html {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Helvetica Neue', 'Fira Sans', 'Droid Sans', Arial, sans-serif;
}

#svpvbgybor .gt_table {
  display: table;
  border-collapse: collapse;
  margin-left: auto;
  margin-right: auto;
  color: #333333;
  font-size: 16px;
  font-weight: normal;
  font-style: normal;
  background-color: #FFFFFF;
  width: auto;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #A8A8A8;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #A8A8A8;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
}

#svpvbgybor .gt_heading {
  background-color: #FFFFFF;
  text-align: center;
  border-bottom-color: #FFFFFF;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#svpvbgybor .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#svpvbgybor .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 0;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#svpvbgybor .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#svpvbgybor .gt_col_headings {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#svpvbgybor .gt_col_heading {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  overflow-x: hidden;
}

#svpvbgybor .gt_column_spanner_outer {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  padding-top: 0;
  padding-bottom: 0;
  padding-left: 4px;
  padding-right: 4px;
}

#svpvbgybor .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#svpvbgybor .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#svpvbgybor .gt_column_spanner {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 5px;
  overflow-x: hidden;
  display: inline-block;
  width: 100%;
}

#svpvbgybor .gt_group_heading {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
}

#svpvbgybor .gt_empty_group_heading {
  padding: 0.5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: middle;
}

#svpvbgybor .gt_from_md > :first-child {
  margin-top: 0;
}

#svpvbgybor .gt_from_md > :last-child {
  margin-bottom: 0;
}

#svpvbgybor .gt_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  margin: 10px;
  border-top-style: solid;
  border-top-width: 1px;
  border-top-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  overflow-x: hidden;
}

#svpvbgybor .gt_stub {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
}

#svpvbgybor .gt_stub_row_group {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
  vertical-align: top;
}

#svpvbgybor .gt_row_group_first td {
  border-top-width: 2px;
}

#svpvbgybor .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#svpvbgybor .gt_first_summary_row {
  border-top-style: solid;
  border-top-color: #D3D3D3;
}

#svpvbgybor .gt_first_summary_row.thick {
  border-top-width: 2px;
}

#svpvbgybor .gt_last_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#svpvbgybor .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#svpvbgybor .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#svpvbgybor .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#svpvbgybor .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#svpvbgybor .gt_footnotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#svpvbgybor .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding-left: 4px;
  padding-right: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#svpvbgybor .gt_sourcenotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#svpvbgybor .gt_sourcenote {
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#svpvbgybor .gt_left {
  text-align: left;
}

#svpvbgybor .gt_center {
  text-align: center;
}

#svpvbgybor .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#svpvbgybor .gt_font_normal {
  font-weight: normal;
}

#svpvbgybor .gt_font_bold {
  font-weight: bold;
}

#svpvbgybor .gt_font_italic {
  font-style: italic;
}

#svpvbgybor .gt_super {
  font-size: 65%;
}

#svpvbgybor .gt_two_val_uncert {
  display: inline-block;
  line-height: 1em;
  text-align: right;
  font-size: 60%;
  vertical-align: -0.25em;
  margin-left: 0.1em;
}

#svpvbgybor .gt_footnote_marks {
  font-style: italic;
  font-weight: normal;
  font-size: 75%;
  vertical-align: 0.4em;
}

#svpvbgybor .gt_asterisk {
  font-size: 100%;
  vertical-align: 0;
}

#svpvbgybor .gt_slash_mark {
  font-size: 0.7em;
  line-height: 0.7em;
  vertical-align: 0.15em;
}

#svpvbgybor .gt_fraction_numerator {
  font-size: 0.6em;
  line-height: 0.6em;
  vertical-align: 0.45em;
}

#svpvbgybor .gt_fraction_denominator {
  font-size: 0.6em;
  line-height: 0.6em;
  vertical-align: -0.05em;
}
</style>
<table class="gt_table">
  
  <thead class="gt_col_headings">
    <tr>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1">island</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1">mean_bill_length_mm</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1">sd_bill_length_mm</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1">mean_bill_depth_mm</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1">sd_bill_depth_mm</th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr><td class="gt_row gt_center">Biscoe</td>
<td class="gt_row gt_right">47.11928</td>
<td class="gt_row gt_right">4.691000</td>
<td class="gt_row gt_right">16.59759</td>
<td class="gt_row gt_right">1.6646963</td></tr>
    <tr><td class="gt_row gt_center">Dream</td>
<td class="gt_row gt_right">46.11613</td>
<td class="gt_row gt_right">5.767211</td>
<td class="gt_row gt_right">19.06613</td>
<td class="gt_row gt_right">0.9105832</td></tr>
    <tr><td class="gt_row gt_center">Torgersen</td>
<td class="gt_row gt_right">40.58696</td>
<td class="gt_row gt_right">3.027496</td>
<td class="gt_row gt_right">19.39130</td>
<td class="gt_row gt_right">1.0824690</td></tr>
  </tbody>
  
  
</table>
</div>

The code above calculates, across each island and only for males, the
means and standard deviation of beak length and depth, and names the
output variables in the format {function name}\_{var name}. Read through
the documentation for these functions and make sure you understand each
step.

Another common task is pivoting between long and wide data when working
with panel datasets, where we have subjects that are observed across
multiple time periods. (E.g. GDP of each country-year)

Long data means each country-year (e.g., US-2010) is one observation (a
row), whereas wide data means each country is a row and each column is a
year.

In the following example we pull GDP (current USD) from the World Bank
from 2010-2020 for the US, China, and Brazil. This code is generalizable
to all countries and for all available WB indicators. We do so using the
`WDI` package, which is handy for pulling data from the World Bank via
its API. The WB is always a good source to check for data! (Side note:
sometimes the World Bank API doesn’t work, which means you can’t use the
`WDI` package and will have to manually download it as a csv and import
it)

``` r
df_gdp <- WDI(country = c('US', 'CN', "BR"),
    indicator = 'NY.GDP.MKTP.CD', #variable name looked up either on the World Bank indicators website or the WDIsearch() function
    start = 2010,
    end = 2020) %>% drop_na()

#To wide format
df_gdp_wide <- pivot_wider(df_gdp,
                    names_from = year, #where to get names for new columns
                    values_from = NY.GDP.MKTP.CD) #where to get values
head(df_gdp_wide)
```

    ## # A tibble: 3 × 13
    ##   iso2c country   `2020`  `2019`  `2018`  `2017`  `2016`  `2015`  `2014`  `2013`
    ##   <chr> <chr>      <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>
    ## 1 BR    Brazil   1.45e12 1.87e12 1.92e12 2.06e12 1.80e12 1.80e12 2.46e12 2.47e12
    ## 2 CN    China    1.47e13 1.43e13 1.39e13 1.23e13 1.12e13 1.11e13 1.05e13 9.57e12
    ## 3 US    United … 2.09e13 2.14e13 2.05e13 1.95e13 1.87e13 1.82e13 1.76e13 1.68e13
    ## # … with 3 more variables: `2012` <dbl>, `2011` <dbl>, `2010` <dbl>
    ## # ℹ Use `colnames()` to see all variable names

``` r
#And back to long format
df_gdp_long <- pivot_longer(df_gdp_wide,
                            cols = 3:13, #which cols to pivot
                            names_to = 'year') # name of new var
#inspecting to check same as original data
head(df_gdp_long)
```

    ## # A tibble: 6 × 4
    ##   iso2c country year    value
    ##   <chr> <chr>   <chr>   <dbl>
    ## 1 BR    Brazil  2020  1.45e12
    ## 2 BR    Brazil  2019  1.87e12
    ## 3 BR    Brazil  2018  1.92e12
    ## 4 BR    Brazil  2017  2.06e12
    ## 5 BR    Brazil  2016  1.80e12
    ## 6 BR    Brazil  2015  1.80e12

As you can see, the `pivot` functions allow us to go easily between wide
and long data formats. Read the documentation for `pivot_wider` and
`pivot_longer`!

## Plotting Data

Plotting your data is a great way to understand it - let’s go back to
our penguins:

``` r
ggplot(penguins, aes(x = flipper_length_mm, y = bill_length_mm)) + geom_point(aes(color = species, shape = species)) + labs(title = "Flipper and bill length",
       x = "Flipper length (mm)",
       y = "Bill length (mm)",
       color = "Penguin species",
       shape = "Penguin species") +
  geom_smooth(method = 'lm') #adding regression line 
```

    ## `geom_smooth()` using formula 'y ~ x'

    ## Warning: Removed 2 rows containing non-finite values (stat_smooth).

    ## Warning: Removed 2 rows containing missing values (geom_point).

![](coding_intro_rmd_files/figure-markdown_github/unnamed-chunk-32-1.png)

`ggplot` is a powerful data visualization package - at its base, it
needs data, an aesthetic `aes` - the properties that can be perceived
from the graphic (e.g., `x` and `y` values), and geometry `geom` -
geometric objects, e.g. what type of graph. You can combine these
components with `+`. We encourage you to read through the `ggplot`
[website](https://ggplot2-book.org/introduction.html).

It is also useful to check out `facet_wrap`, `facet_grid` for generating
multiple plots based on some variable (usually a categorical one, e.g.,
plot across different species). For some great examples of `ggplot`
graphics, check out the `palmerpenguins` package
[website](https://allisonhorst.github.io/palmerpenguins/articles/examples.html).

You can also use base R for plotting, although as you can see, the code
is a bit clunkier:

``` r
plot(x = penguins$flipper_length_mm,
     y = penguins$bill_length_mm,
     type = 'p',
     xlab = 'Flipper length',
     ylab = 'Bill length',
     main = 'Penguins',
     col = penguins$species)
legend("topleft", legend = levels(penguins$species), pch = 1,
       col = c('black', 'red', 'green'))
```

![](coding_intro_rmd_files/figure-markdown_github/unnamed-chunk-33-1.png)

We’re getting very close to what you will be covering in Quant 1! Linear
regression seeks to recover the relationship between variables. You’ll
notice that there is a positive relationship between flipper and beak
length. `lm` is the function for linear regression, and the tilde `~`
sign separates the left and right hand sides of a model formula:

``` r
mod_penguin <- lm(flipper_length_mm ~ bill_length_mm, data = penguins)
summary(mod_penguin)
```

    ## 
    ## Call:
    ## lm(formula = flipper_length_mm ~ bill_length_mm, data = penguins)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -43.708  -7.896   0.664   8.650  21.179 
    ## 
    ## Coefficients:
    ##                Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)    126.6844     4.6651   27.16   <2e-16 ***
    ## bill_length_mm   1.6901     0.1054   16.03   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 10.63 on 340 degrees of freedom
    ##   (2 observations deleted due to missingness)
    ## Multiple R-squared:  0.4306, Adjusted R-squared:  0.4289 
    ## F-statistic: 257.1 on 1 and 340 DF,  p-value: < 2.2e-16

We won’t go into this further here — that is the point of the class! We
can add the regression line to the original plot:

``` r
plot(x = penguins$flipper_length_mm,
     y = penguins$bill_length_mm,
     type = 'p',
     xlab = 'Flipper length',
     ylab = 'Bill length',
     main = 'Penguins',
     col = penguins$species)
legend("topleft", legend = levels(penguins$species), pch = 1,
       col = c('black', 'red', 'green'))
abline(lm(bill_length_mm ~ flipper_length_mm, data = penguins), col = 'blue')
```

![](coding_intro_rmd_files/figure-markdown_github/unnamed-chunk-35-1.png)

## Writing Functions

We often have to write our own functions. It’s always good practice to
comment in what each argument is and what format it needs to be in —
your future self will thank you! Let’s look at the example below, where
we write a function to generate balance tables which show summary
statistics between treatment and control groups across covariates of
interest. We often use balance tables to show that the two groups are
comparable across attributes that we think are important.

You will notice the embrace `{{}}` operator, which we use to enable the
function to accept column names as inputs. The actual mechanism behind
it is defuse-and-inject (don’t worry about this for Quant 1), which you
can read about
[here](https://search.r-project.org/CRAN/refmans/rlang/html/topic-defuse.html).

``` r
baltable <- function(dat, treat, covars){ 
  #function takes 3 arguments
  #dat for dataframe
  #treat for column name of treatment status
  #covars for column name(s) for covars of interest 
  
  #generating balance table, using the embrace {{}} operator to defuse-and-inject 
  bal <- dat %>% group_by({{treat}}) %>% 
    summarise(across(.cols = {{covars}},.fns = list(mean = ~mean(.x, na.rm = T), sd = ~sd(.x, na.rm = T)),
                     .names = "{.fn}_{.col}"))
  return(bal)
}

#generating treatment status for penguins 
penguins$treatment <- rbinom(nrow(penguins), 1, 0.5)
#baltable
df_bal <- baltable(dat = penguins, treat = treatment, covars = c(flipper_length_mm, bill_length_mm))

gt(df_bal)
```

<div id="dcfsikcwjj" style="overflow-x:auto;overflow-y:auto;width:auto;height:auto;">
<style>html {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Helvetica Neue', 'Fira Sans', 'Droid Sans', Arial, sans-serif;
}

#dcfsikcwjj .gt_table {
  display: table;
  border-collapse: collapse;
  margin-left: auto;
  margin-right: auto;
  color: #333333;
  font-size: 16px;
  font-weight: normal;
  font-style: normal;
  background-color: #FFFFFF;
  width: auto;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #A8A8A8;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #A8A8A8;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
}

#dcfsikcwjj .gt_heading {
  background-color: #FFFFFF;
  text-align: center;
  border-bottom-color: #FFFFFF;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#dcfsikcwjj .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#dcfsikcwjj .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 0;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#dcfsikcwjj .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#dcfsikcwjj .gt_col_headings {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#dcfsikcwjj .gt_col_heading {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  overflow-x: hidden;
}

#dcfsikcwjj .gt_column_spanner_outer {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  padding-top: 0;
  padding-bottom: 0;
  padding-left: 4px;
  padding-right: 4px;
}

#dcfsikcwjj .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#dcfsikcwjj .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#dcfsikcwjj .gt_column_spanner {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 5px;
  overflow-x: hidden;
  display: inline-block;
  width: 100%;
}

#dcfsikcwjj .gt_group_heading {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
}

#dcfsikcwjj .gt_empty_group_heading {
  padding: 0.5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: middle;
}

#dcfsikcwjj .gt_from_md > :first-child {
  margin-top: 0;
}

#dcfsikcwjj .gt_from_md > :last-child {
  margin-bottom: 0;
}

#dcfsikcwjj .gt_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  margin: 10px;
  border-top-style: solid;
  border-top-width: 1px;
  border-top-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  overflow-x: hidden;
}

#dcfsikcwjj .gt_stub {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
}

#dcfsikcwjj .gt_stub_row_group {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
  vertical-align: top;
}

#dcfsikcwjj .gt_row_group_first td {
  border-top-width: 2px;
}

#dcfsikcwjj .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#dcfsikcwjj .gt_first_summary_row {
  border-top-style: solid;
  border-top-color: #D3D3D3;
}

#dcfsikcwjj .gt_first_summary_row.thick {
  border-top-width: 2px;
}

#dcfsikcwjj .gt_last_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#dcfsikcwjj .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#dcfsikcwjj .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#dcfsikcwjj .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#dcfsikcwjj .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#dcfsikcwjj .gt_footnotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#dcfsikcwjj .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding-left: 4px;
  padding-right: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#dcfsikcwjj .gt_sourcenotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#dcfsikcwjj .gt_sourcenote {
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#dcfsikcwjj .gt_left {
  text-align: left;
}

#dcfsikcwjj .gt_center {
  text-align: center;
}

#dcfsikcwjj .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#dcfsikcwjj .gt_font_normal {
  font-weight: normal;
}

#dcfsikcwjj .gt_font_bold {
  font-weight: bold;
}

#dcfsikcwjj .gt_font_italic {
  font-style: italic;
}

#dcfsikcwjj .gt_super {
  font-size: 65%;
}

#dcfsikcwjj .gt_two_val_uncert {
  display: inline-block;
  line-height: 1em;
  text-align: right;
  font-size: 60%;
  vertical-align: -0.25em;
  margin-left: 0.1em;
}

#dcfsikcwjj .gt_footnote_marks {
  font-style: italic;
  font-weight: normal;
  font-size: 75%;
  vertical-align: 0.4em;
}

#dcfsikcwjj .gt_asterisk {
  font-size: 100%;
  vertical-align: 0;
}

#dcfsikcwjj .gt_slash_mark {
  font-size: 0.7em;
  line-height: 0.7em;
  vertical-align: 0.15em;
}

#dcfsikcwjj .gt_fraction_numerator {
  font-size: 0.6em;
  line-height: 0.6em;
  vertical-align: 0.45em;
}

#dcfsikcwjj .gt_fraction_denominator {
  font-size: 0.6em;
  line-height: 0.6em;
  vertical-align: -0.05em;
}
</style>
<table class="gt_table">
  
  <thead class="gt_col_headings">
    <tr>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1">treatment</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1">mean_flipper_length_mm</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1">sd_flipper_length_mm</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1">mean_bill_length_mm</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1">sd_bill_length_mm</th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr><td class="gt_row gt_right">0</td>
<td class="gt_row gt_right">200.2866</td>
<td class="gt_row gt_right">14.47237</td>
<td class="gt_row gt_right">43.6414</td>
<td class="gt_row gt_right">5.460312</td></tr>
    <tr><td class="gt_row gt_right">1</td>
<td class="gt_row gt_right">201.4486</td>
<td class="gt_row gt_right">13.72038</td>
<td class="gt_row gt_right">44.1600</td>
<td class="gt_row gt_right">5.462433</td></tr>
  </tbody>
  
  
</table>
</div>

## Short purrr Taster

Here we use `purrr` to perform two common tasks. The `purrr` syntax
takes some getting used to, so don’t worry about this for Quant 1! This
is just here for your reference.

Running regressions by group, in this case for each species we regress
bill length on bill depth and body mass, then present the relevant
coefficients in a table.

``` r
penguins %>% 
  split(.$species) %>% #splitting data according to species
  map(~lm(bill_length_mm~bill_depth_mm + body_mass_g, data = .x)) %>% #running regression on each species
  map(broom::tidy) %>% #tidying reggression output into df
  data.table::rbindlist() %>% #binding dfs together (each element are the results from a species)
  filter(term != '(Intercept)') %>% #dropping intercept
  select(term, estimate) %>% #extracing term and estimate
  mutate(species = rep(levels(penguins$species), each = 2)) %>% #adding species indicator
  gt()
```

<div id="dfajucvqat" style="overflow-x:auto;overflow-y:auto;width:auto;height:auto;">
<style>html {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Helvetica Neue', 'Fira Sans', 'Droid Sans', Arial, sans-serif;
}

#dfajucvqat .gt_table {
  display: table;
  border-collapse: collapse;
  margin-left: auto;
  margin-right: auto;
  color: #333333;
  font-size: 16px;
  font-weight: normal;
  font-style: normal;
  background-color: #FFFFFF;
  width: auto;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #A8A8A8;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #A8A8A8;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
}

#dfajucvqat .gt_heading {
  background-color: #FFFFFF;
  text-align: center;
  border-bottom-color: #FFFFFF;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#dfajucvqat .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#dfajucvqat .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 0;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#dfajucvqat .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#dfajucvqat .gt_col_headings {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#dfajucvqat .gt_col_heading {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  overflow-x: hidden;
}

#dfajucvqat .gt_column_spanner_outer {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  padding-top: 0;
  padding-bottom: 0;
  padding-left: 4px;
  padding-right: 4px;
}

#dfajucvqat .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#dfajucvqat .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#dfajucvqat .gt_column_spanner {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 5px;
  overflow-x: hidden;
  display: inline-block;
  width: 100%;
}

#dfajucvqat .gt_group_heading {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
}

#dfajucvqat .gt_empty_group_heading {
  padding: 0.5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: middle;
}

#dfajucvqat .gt_from_md > :first-child {
  margin-top: 0;
}

#dfajucvqat .gt_from_md > :last-child {
  margin-bottom: 0;
}

#dfajucvqat .gt_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  margin: 10px;
  border-top-style: solid;
  border-top-width: 1px;
  border-top-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  overflow-x: hidden;
}

#dfajucvqat .gt_stub {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
}

#dfajucvqat .gt_stub_row_group {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
  vertical-align: top;
}

#dfajucvqat .gt_row_group_first td {
  border-top-width: 2px;
}

#dfajucvqat .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#dfajucvqat .gt_first_summary_row {
  border-top-style: solid;
  border-top-color: #D3D3D3;
}

#dfajucvqat .gt_first_summary_row.thick {
  border-top-width: 2px;
}

#dfajucvqat .gt_last_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#dfajucvqat .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#dfajucvqat .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#dfajucvqat .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#dfajucvqat .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#dfajucvqat .gt_footnotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#dfajucvqat .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding-left: 4px;
  padding-right: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#dfajucvqat .gt_sourcenotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#dfajucvqat .gt_sourcenote {
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#dfajucvqat .gt_left {
  text-align: left;
}

#dfajucvqat .gt_center {
  text-align: center;
}

#dfajucvqat .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#dfajucvqat .gt_font_normal {
  font-weight: normal;
}

#dfajucvqat .gt_font_bold {
  font-weight: bold;
}

#dfajucvqat .gt_font_italic {
  font-style: italic;
}

#dfajucvqat .gt_super {
  font-size: 65%;
}

#dfajucvqat .gt_two_val_uncert {
  display: inline-block;
  line-height: 1em;
  text-align: right;
  font-size: 60%;
  vertical-align: -0.25em;
  margin-left: 0.1em;
}

#dfajucvqat .gt_footnote_marks {
  font-style: italic;
  font-weight: normal;
  font-size: 75%;
  vertical-align: 0.4em;
}

#dfajucvqat .gt_asterisk {
  font-size: 100%;
  vertical-align: 0;
}

#dfajucvqat .gt_slash_mark {
  font-size: 0.7em;
  line-height: 0.7em;
  vertical-align: 0.15em;
}

#dfajucvqat .gt_fraction_numerator {
  font-size: 0.6em;
  line-height: 0.6em;
  vertical-align: 0.45em;
}

#dfajucvqat .gt_fraction_denominator {
  font-size: 0.6em;
  line-height: 0.6em;
  vertical-align: -0.05em;
}
</style>
<table class="gt_table">
  
  <thead class="gt_col_headings">
    <tr>
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1">term</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1">estimate</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1">species</th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr><td class="gt_row gt_left">bill_depth_mm</td>
<td class="gt_row gt_right">0.246643705</td>
<td class="gt_row gt_left">Adelie</td></tr>
    <tr><td class="gt_row gt_left">body_mass_g</td>
<td class="gt_row gt_right">0.002810859</td>
<td class="gt_row gt_left">Adelie</td></tr>
    <tr><td class="gt_row gt_left">bill_depth_mm</td>
<td class="gt_row gt_right">1.589875965</td>
<td class="gt_row gt_left">Chinstrap</td></tr>
    <tr><td class="gt_row gt_left">body_mass_g</td>
<td class="gt_row gt_right">0.001623499</td>
<td class="gt_row gt_left">Chinstrap</td></tr>
    <tr><td class="gt_row gt_left">bill_depth_mm</td>
<td class="gt_row gt_right">1.054910678</td>
<td class="gt_row gt_left">Gentoo</td></tr>
    <tr><td class="gt_row gt_left">body_mass_g</td>
<td class="gt_row gt_right">0.002614378</td>
<td class="gt_row gt_left">Gentoo</td></tr>
  </tbody>
  
  
</table>
</div>

Another common task is to regress multiple dependent variables on the
same set of independent variables. In the code below we create three DVs
from `x1` and `x2`, and use `map` to iterate through the DVs:

``` r
#creating IVs 
x1 <- rnorm(100)
x2 <- rnorm(100)
#creating three DVs
y1 <- 3 + 2*x1 + 4*x2 +rnorm(100)
y2 <- 9 + 1.5*x1 + 6*x2 + rnorm(100)
y3 <- 7 + 5 * x1 + 2.5 * x2 + rnorm(100)
#creating df
df_mul <- data.frame(cbind(x1, x2, y1, y2, y3))

df_mul %>% select(starts_with('y')) %>% #selecting DV columns
  map(~lm(.x ~ x1 + x2, data = df_mul)) %>% #for each DV run this regression
  map(broom::tidy) %>% #tidying into df
  bind_rows() %>% #binding 
  filter(term != '(Intercept)') %>% #dropping intercept esetimate
  select(term, estimate) %>% #selecting term and estimate
  mutate(dv =rep(c('y1', 'y2', 'y3'), each = 2)) %>% #adding DV indicator
  gt()
```

<div id="yuwwdiegfp" style="overflow-x:auto;overflow-y:auto;width:auto;height:auto;">
<style>html {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Helvetica Neue', 'Fira Sans', 'Droid Sans', Arial, sans-serif;
}

#yuwwdiegfp .gt_table {
  display: table;
  border-collapse: collapse;
  margin-left: auto;
  margin-right: auto;
  color: #333333;
  font-size: 16px;
  font-weight: normal;
  font-style: normal;
  background-color: #FFFFFF;
  width: auto;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #A8A8A8;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #A8A8A8;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
}

#yuwwdiegfp .gt_heading {
  background-color: #FFFFFF;
  text-align: center;
  border-bottom-color: #FFFFFF;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#yuwwdiegfp .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#yuwwdiegfp .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 0;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#yuwwdiegfp .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#yuwwdiegfp .gt_col_headings {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#yuwwdiegfp .gt_col_heading {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  overflow-x: hidden;
}

#yuwwdiegfp .gt_column_spanner_outer {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  padding-top: 0;
  padding-bottom: 0;
  padding-left: 4px;
  padding-right: 4px;
}

#yuwwdiegfp .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#yuwwdiegfp .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#yuwwdiegfp .gt_column_spanner {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 5px;
  overflow-x: hidden;
  display: inline-block;
  width: 100%;
}

#yuwwdiegfp .gt_group_heading {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
}

#yuwwdiegfp .gt_empty_group_heading {
  padding: 0.5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: middle;
}

#yuwwdiegfp .gt_from_md > :first-child {
  margin-top: 0;
}

#yuwwdiegfp .gt_from_md > :last-child {
  margin-bottom: 0;
}

#yuwwdiegfp .gt_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  margin: 10px;
  border-top-style: solid;
  border-top-width: 1px;
  border-top-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  overflow-x: hidden;
}

#yuwwdiegfp .gt_stub {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
}

#yuwwdiegfp .gt_stub_row_group {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
  vertical-align: top;
}

#yuwwdiegfp .gt_row_group_first td {
  border-top-width: 2px;
}

#yuwwdiegfp .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#yuwwdiegfp .gt_first_summary_row {
  border-top-style: solid;
  border-top-color: #D3D3D3;
}

#yuwwdiegfp .gt_first_summary_row.thick {
  border-top-width: 2px;
}

#yuwwdiegfp .gt_last_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#yuwwdiegfp .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#yuwwdiegfp .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#yuwwdiegfp .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#yuwwdiegfp .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#yuwwdiegfp .gt_footnotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#yuwwdiegfp .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding-left: 4px;
  padding-right: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#yuwwdiegfp .gt_sourcenotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#yuwwdiegfp .gt_sourcenote {
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#yuwwdiegfp .gt_left {
  text-align: left;
}

#yuwwdiegfp .gt_center {
  text-align: center;
}

#yuwwdiegfp .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#yuwwdiegfp .gt_font_normal {
  font-weight: normal;
}

#yuwwdiegfp .gt_font_bold {
  font-weight: bold;
}

#yuwwdiegfp .gt_font_italic {
  font-style: italic;
}

#yuwwdiegfp .gt_super {
  font-size: 65%;
}

#yuwwdiegfp .gt_two_val_uncert {
  display: inline-block;
  line-height: 1em;
  text-align: right;
  font-size: 60%;
  vertical-align: -0.25em;
  margin-left: 0.1em;
}

#yuwwdiegfp .gt_footnote_marks {
  font-style: italic;
  font-weight: normal;
  font-size: 75%;
  vertical-align: 0.4em;
}

#yuwwdiegfp .gt_asterisk {
  font-size: 100%;
  vertical-align: 0;
}

#yuwwdiegfp .gt_slash_mark {
  font-size: 0.7em;
  line-height: 0.7em;
  vertical-align: 0.15em;
}

#yuwwdiegfp .gt_fraction_numerator {
  font-size: 0.6em;
  line-height: 0.6em;
  vertical-align: 0.45em;
}

#yuwwdiegfp .gt_fraction_denominator {
  font-size: 0.6em;
  line-height: 0.6em;
  vertical-align: -0.05em;
}
</style>
<table class="gt_table">
  
  <thead class="gt_col_headings">
    <tr>
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1">term</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1">estimate</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1">dv</th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr><td class="gt_row gt_left">x1</td>
<td class="gt_row gt_right">1.986071</td>
<td class="gt_row gt_left">y1</td></tr>
    <tr><td class="gt_row gt_left">x2</td>
<td class="gt_row gt_right">4.086303</td>
<td class="gt_row gt_left">y1</td></tr>
    <tr><td class="gt_row gt_left">x1</td>
<td class="gt_row gt_right">1.588132</td>
<td class="gt_row gt_left">y2</td></tr>
    <tr><td class="gt_row gt_left">x2</td>
<td class="gt_row gt_right">5.979871</td>
<td class="gt_row gt_left">y2</td></tr>
    <tr><td class="gt_row gt_left">x1</td>
<td class="gt_row gt_right">4.916670</td>
<td class="gt_row gt_left">y3</td></tr>
    <tr><td class="gt_row gt_left">x2</td>
<td class="gt_row gt_right">2.455419</td>
<td class="gt_row gt_left">y3</td></tr>
  </tbody>
  
  
</table>
</div>

As these two examples show, `purrr` is extremely powerful! You won’t
need to use it much in Quant 1, but if you’re interested, we encourage
you to check out the [documentation](https://purrr.tidyverse.org/) and
some [examples](https://www.rebeccabarter.com/blog/2019-08-19_purrr/).

## Other Useful Things

Here are some other useful `R` operations:

Checking if something is in a vector:

``` r
4 %in% 1:10
```

    ## [1] TRUE

``` r
65 %in% 1:10
```

    ## [1] FALSE

Getting the remainder of a division:

``` r
9%%2
```

    ## [1] 1

`while` loops - for when you want `R` to keep doing something until a
condition is fulfilled

``` r
i <- 3
while(i < 6){
  print(i)
  i <- i + 1
}
```

    ## [1] 3
    ## [1] 4
    ## [1] 5

Happy coding! Please direct questions and feedback to <raywyf@mit.edu>.
