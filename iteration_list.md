iteration and list columns
================
Alexa Burko
2026-11-05

``` r
source("source/mean_and_sd.R")
source("source/nsduh_process.R")

set.seed(1236)
```

## Lists…

``` r
l = list(
  vec_numeric = 5:17,
  mat = matrix(1:8, nrow = 2, ncol = 4, byrow = TRUE),
  vec_logical = c(TRUE),
  summary_table = summary(rnorm(1000))
)

# 2 brackets pull something out of a list
l[[1]]
```

    ##  [1]  5  6  7  8  9 10 11 12 13 14 15 16 17

``` r
l[[2]]
```

    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    2    3    4
    ## [2,]    5    6    7    8

``` r
l[["vec_logical"]]
```

    ## [1] TRUE

## ‘for’ loops

``` r
list_norm = list(
  a = rnorm(20, 3, 4),
    b = rnorm(20, -14, 20),
    c = rnorm(20, 1000, 1000),
    d = rnorm(20, 0, 1)
)

list_norm
```

    ## $a
    ##  [1]  0.9159114  3.9351002  0.4542432 -2.2204479  6.8932190  5.1244742
    ##  [7]  3.0167339  1.2722880  2.2738587  1.3975934  0.2412513  8.7954751
    ## [13]  5.4102001  5.6806398  1.4349576  7.7910554  1.3815394  3.9662007
    ## [19]  5.5049290 -4.4350801
    ## 
    ## $b
    ##  [1] -38.8273125 -17.3048465 -37.0491490 -10.9662495 -13.8820017 -42.4692825
    ##  [7]  24.9015607  10.7117280  -1.2350787 -11.9174419  16.2479180  23.7679930
    ## [13] -10.8558799  -2.3840318 -27.5021477 -11.0160255  -0.9578981 -54.7284315
    ## [19]  -0.6934327  21.7843104
    ## 
    ## $c
    ##  [1]  194.20600 1389.65948  974.76668 -330.89439 1383.06295 1501.20091
    ##  [7]  646.02521 1207.44952 1008.42877 2020.73223 1390.50109  534.72544
    ## [13] 2288.73611 1046.34838  895.34461 1777.98451   11.45126 1344.37097
    ## [19]  517.57690  507.37506
    ## 
    ## $d
    ##  [1]  1.95387846 -0.05245400 -0.46954531  0.07402405 -1.58979325 -1.30688268
    ##  [7] -1.10632794 -0.56723500 -0.41408053  0.92099945  1.44702754  1.99236409
    ## [13] -0.55596915 -0.28278326  0.08300015  1.09988202 -1.89214254  0.10662779
    ## [19]  0.16717212 -0.72687919

Can I get mean and SD for each sample?

``` r
mean_and_sd(list_norm[[1]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.94  3.32

``` r
mean_and_sd(list_norm[[2]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -9.22  22.7

``` r
mean_and_sd(list_norm[[3]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 1015.  665.

``` r
mean_and_sd(list_norm[[4]])
```

    ## # A tibble: 1 × 2
    ##      mean    sd
    ##     <dbl> <dbl>
    ## 1 -0.0560  1.09

``` r
# this is good, but can lose track of these
```

Can I do better?

``` r
output = vector("list", length = 4)
# creating a list called output with a length of 4
for (i in 1:4) {
  
  output[[i]] = mean_and_sd(list_norm[[i]])
  
}

output
```

    ## [[1]]
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.94  3.32
    ## 
    ## [[2]]
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -9.22  22.7
    ## 
    ## [[3]]
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 1015.  665.
    ## 
    ## [[4]]
    ## # A tibble: 1 × 2
    ##      mean    sd
    ##     <dbl> <dbl>
    ## 1 -0.0560  1.09

## ‘map’ instead

This is better!

``` r
output = map(list_norm, mean_and_sd)

# i think this one is not giving a list and it giving dfs instead bc the mean_and_sd function tells it to output a df

output = map(list_norm, median)

# map(input list, function you want to apply)

# map gives a list
```

What about map variants

``` r
output = map_dbl(list_norm, median)
# makes it a vector 

output = map_df(list_norm, median)
# makes it a data frame
```
