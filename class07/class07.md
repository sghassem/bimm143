Class 7: R functions and packages
================
Saeed Ghassemzadeh
4/23/2019

Functions revisited
===================

We will source a file from online with our functions from last day

``` r
source("http://tinyurl.com/rescale-R")
```

Try out the last day's rescale() function

``` r
rescale(1:10)
```

    ##  [1] 0.0000000 0.1111111 0.2222222 0.3333333 0.4444444 0.5555556 0.6666667
    ##  [8] 0.7777778 0.8888889 1.0000000

``` r
## test fail 
## rescale(c(1:10, "string"))
```

Try the rescale2() function that catches string inputs

``` r
#rescale2(c(1:10, "string"))
```

Find missing NA values in two vectors
=====================================

Start with a simple example of the larger problem I am trying to solve

``` r
x <- c( 1, 2, NA, 3, NA) 
y<-c(NA, 3, NA, 3, 4)
```

``` r
is.na(x)
```

    ## [1] FALSE FALSE  TRUE FALSE  TRUE

``` r
is.na(y)
```

    ## [1]  TRUE FALSE  TRUE FALSE FALSE

``` r
which(is.na(x))
```

    ## [1] 3 5

Try putting these together with an AND

``` r
is.na(x) & is.na(y)
```

    ## [1] FALSE FALSE  TRUE FALSE FALSE

Take the sum() to find out how many TRUE values we have and thus how many NAs we had in both x and y

``` r
sum(is.na(x) & is.na(y))
```

    ## [1] 1

Now I can make this into our first function...

``` r
both_na <- function(x, y) {
  sum(is.na(x) & is.na(y))
}
```

``` r
both_na(x, y)
```

    ## [1] 1

``` r
both_na(x, c(NA, 3, NA, 2, NA))
```

    ## [1] 2

``` r
x <-  c(NA, NA, NA)
y1 <- c( 1, NA, NA)
y2 <- c( 1, NA, NA, NA)
```

``` r
both_na(x, y2)
```

    ## Warning in is.na(x) & is.na(y): longer object length is not a multiple of
    ## shorter object length

    ## [1] 3

``` r
# will recycle length of shorter vector to match length of longer vector
# essentially gives x an extra NA at the end
```

``` r
y3 <- c(1, NA, NA, NA, NA, NA, NA)
# compare with y1 (run below)
# x = match
#     (1), x,  x, (1), x,  x, (1)
# result is 4 (see below)
```

``` r
both_na(y1, y3)
```

    ## Warning in is.na(x) & is.na(y): longer object length is not a multiple of
    ## shorter object length

    ## [1] 4

``` r
3 == 3
```

    ## [1] TRUE

``` r
3 < 2
```

    ## [1] FALSE

``` r
3 == 2
```

    ## [1] FALSE

``` r
3 != 2
```

    ## [1] TRUE

``` r
length(y2)
```

    ## [1] 4

``` r
length(x) != length(y2)
```

    ## [1] TRUE

``` r
# see above, x has 3 things in it, y2 has 4 things in it
```

Now let's try the both\_na2() function on our different length input vactors

``` r
# both_na2(x, y2)
# there is a stop; unlike both_na(), this function STOPS the code early
```

``` r
which(c(F,F,T,F,T))
```

    ## [1] 3 5

``` r
# tells you which slot is TRUE
# can also write out FALSE or TRUE
```

``` r
which(is.na(c(1,2,NA,4)))
```

    ## [1] 3

``` r
# tells you which slot is an NA
```

``` r
x <- c( 1, 2, NA, 3, NA) 
y<-c(NA, 3, NA, 3, 4)

both_na3(x,y)
```

    ## Found 1 NA's at position(s):3

    ## $number
    ## [1] 1
    ## 
    ## $which
    ## [1] 3

``` r
# $number tells you the count of how many both NA there is 
# $which tells you which slot they both have an NA
```

One last example
================

``` r
df1
```

    ##     IDs exp
    ## 1 gene1   2
    ## 2 gene2   1
    ## 3 gene3   1

``` r
df2
```

    ##     IDs exp
    ## 1 gene2  -2
    ## 2 gene4  NA
    ## 3 gene3   1
    ## 4 gene5   2

``` r
df3
```

    ##     IDs exp
    ## 1 gene2  -2
    ## 2 gene2  NA
    ## 3 gene5   1
    ## 4 gene5   2

Make things simple

``` r
x <- df1$IDs
y <- df2$IDs

x
```

    ## [1] "gene1" "gene2" "gene3"

``` r
y
```

    ## [1] "gene2" "gene4" "gene3" "gene5"

``` r
intersect(x,y)
```

    ## [1] "gene2" "gene3"

``` r
x %in% y
```

    ## [1] FALSE  TRUE  TRUE

``` r
# which position elements in x are seen anywhere in y
```

``` r
which(x %in% y)
```

    ## [1] 2 3

``` r
# which specific position elements in x are sene in y
```

``` r
(x %in% y)
```

    ## [1] FALSE  TRUE  TRUE

``` r
x[x %in% y]
```

    ## [1] "gene2" "gene3"

``` r
# something about "indices"...
x[x %in% y]
```

    ## [1] "gene2" "gene3"

``` r
y[ y %in% x ]
```

    ## [1] "gene2" "gene3"

Use the R Studio shortcut 'Code -&gt; Extract Function' to turn our snippet into a working function
===================================================================================================

``` r
# We can now cbind() these these results...
gene_intersect <- function(x, y) {
  cbind( x[ x %in% y ], y[ y %in% x ] )
} 

# code -> extract function -> name function
# it tries to generate you a function, check it to make sure everything is right
```

``` r
gene_intersect(df1$IDs, df2$IDs)
```

    ##      [,1]    [,2]   
    ## [1,] "gene2" "gene2"
    ## [2,] "gene3" "gene3"

``` r
gene_intersect2(df1, df2)
```

    ##     IDs exp df2[df2$IDs %in% df1$IDs, "exp"]
    ## 2 gene2   1                               -2
    ## 3 gene3   1                                1

``` r
# ugly printing of column name at the end
```

``` r
gene_intersect3(df1, df2)
```

    ##     IDs exp exp2
    ## 2 gene2   1   -2
    ## 3 gene3   1    1

``` r
# this function cleans up the column name
```

``` r
merge(df1, df2, by="IDs")
```

    ##     IDs exp.x exp.y
    ## 1 gene2     1    -2
    ## 2 gene3     1     1

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">## Grade Function</td>
</tr>
<tr class="even">
<td align="left">Write a function called grade() to calculate the mean score, dropping the lowest single score</td>
</tr>
<tr class="odd">
<td align="left"><code>r x &lt;- c(100, 100, 100, 100, 100, 100, 100, 90) # set2 &lt;- c(100, 90, 90, 90, 90, 90, 97, 80)</code></td>
</tr>
<tr class="even">
<td align="left">Find out what's the worst score. Subtract from class. Then, do average of that set.</td>
</tr>
<tr class="odd">
<td align="left"><code>r min(x)</code></td>
</tr>
<tr class="even">
<td align="left"><code>## [1] 90</code></td>
</tr>
<tr class="odd">
<td align="left"><code>r sum(x) - min(x)</code></td>
</tr>
<tr class="even">
<td align="left"><code>## [1] 700</code></td>
</tr>
<tr class="odd">
<td align="left"><code>r (sum(x) - min(x))/(length(x)-1)</code></td>
</tr>
<tr class="even">
<td align="left"><code>## [1] 100</code></td>
</tr>
<tr class="odd">
<td align="left">Create the function</td>
</tr>
<tr class="even">
<td align="left"><code>r grade &lt;- function(x) { (sum(x) - min(x))/(length(x)-1) }</code></td>
</tr>
<tr class="odd">
<td align="left">Test the function</td>
</tr>
<tr class="even">
<td align="left"><code>r # grade(set1)</code></td>
</tr>
</tbody>
</table>
