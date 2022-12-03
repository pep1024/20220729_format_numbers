Format Numbers
================
Pep Porrà
2022-07-29

``` r
library(dplyr)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

## Goal

Explain different ways to format numbers

## Context

Often we want to control the way numbers are printed. Common examples
are the number of decimals of numbers in tables or include a currency
with the numbers. As always, there are multiple ways to reach this goal.
We summarize many of the methods available here

## Format cases

We will concentrate in five format cases that can be combined among
them:

1.  Decimals
2.  Width
3.  Units
4.  Prefix
5.  Percentage

### Decimals

``` r
# example
n_ex<-  c(pi, 0.01, -2345.6789, 10000, -1/3)
n_ex
```

    ## [1]     3.1415927     0.0100000 -2345.6789000 10000.0000000    -0.3333333

Numbers are printed using default options in R:

``` r
all_options <- options()
```

``` r
all_options$digits
```

    ## [1] 7

``` r
all_options$scipen
```

    ## [1] 0

``` r
all_options$width
```

    ## [1] 80

First way to get a given number of decimals is function `round()`

``` r
round(n_ex, digits = 2)
```

    ## [1]     3.14     0.01 -2345.68 10000.00    -0.33

``` r
n_sci <- 100000000
n_sci
```

    ## [1] 1e+08

``` r
round(n_sci, 2)
```

    ## [1] 1e+08

Rounding numbers stills keep them as numbers:

``` r
2.345 + 4
```

    ## [1] 6.345

``` r
round(2.345, 1) + 4
```

    ## [1] 6.3

``` r
2.345 + 4 == round(2.345, 1) + 4
```

    ## [1] FALSE

Another possibility to control number of decimals is to convert numbers
into characters. This opens all possibilities related to number
formatting. Nevertheless, we cannot operate anymore with the result.

Main function to format numbers is `format()`:

``` r
format(n_ex, digits = 2)
```

    ## [1] "    3.14" "    0.01" "-2345.68" "10000.00" "   -0.33"

Note that the same format is applied to all numbers in a vector. We can
get rid off the scientific notation

``` r
format(n_ex, digits = 2, scientific = FALSE)
```

    ## [1] "    3.14" "    0.01" "-2345.68" "10000.00" "   -0.33"

Note that all numbers have the same length as characters which is of
interest when printing columns of numbers (data frames)

``` r
x <- format(n_ex, digits = 2, scientific = FALSE)
df_x <- as.data.frame(x = x)
df_x
```

    ##          x
    ## 1     3.14
    ## 2     0.01
    ## 3 -2345.68
    ## 4 10000.00
    ## 5    -0.33

Another possibility is `sprintf()`:

``` r
sapply(n_ex, function(x) sprintf(fmt = "%.2f", x))
```

    ## [1] "3.14"     "0.01"     "-2345.68" "10000.00" "-0.33"

``` r
sapply(n_ex, function(x) sprintf(fmt = "%5.2f", x))
```

    ## [1] " 3.14"    " 0.01"    "-2345.68" "10000.00" "-0.33"

``` r
sapply(n_ex, function(x) sprintf(fmt = "%06.1f", x))
```

    ## [1] "0003.1"  "0000.0"  "-2345.7" "10000.0" "-000.3"

``` r
sapply(n_ex, function(x) sprintf(fmt = "% .1f", x))
```

    ## [1] " 3.1"     " 0.0"     "-2345.7"  " 10000.0" "-0.3"

``` r
sapply(n_ex, function(x) sprintf(fmt = "%+.1f", x))
```

    ## [1] "+3.1"     "+0.0"     "-2345.7"  "+10000.0" "-0.3"

Equivalently, we can use `formatC()`

``` r
sapply(n_ex, function(x) sprintf(fmt = "%.2f", x))
```

    ## [1] "3.14"     "0.01"     "-2345.68" "10000.00" "-0.33"

``` r
formatC(n_ex, digits = 2)
```

    ## [1] "3.1"      "0.01"     "-2.3e+03" "1e+04"    "-0.33"

``` r
sapply(n_ex, function(x) sprintf(fmt = "%.2f", x))
```

    ## [1] "3.14"     "0.01"     "-2345.68" "10000.00" "-0.33"

``` r
formatC(n_ex, digits = 2, format = "f")
```

    ## [1] "3.14"     "0.01"     "-2345.68" "10000.00" "-0.33"

``` r
sapply(n_ex, function(x) sprintf(fmt = "%5.2f", x))
```

    ## [1] " 3.14"    " 0.01"    "-2345.68" "10000.00" "-0.33"

``` r
formatC(n_ex, digits = 2, width = 5, format = "f")
```

    ## [1] " 3.14"    " 0.01"    "-2345.68" "10000.00" "-0.33"

``` r
sapply(n_ex, function(x) sprintf(fmt = "%06.1f", x))
```

    ## [1] "0003.1"  "0000.0"  "-2345.7" "10000.0" "-000.3"

``` r
formatC(n_ex, digits = 1, width = 6, format = "f", flag = "0")
```

    ## [1] "0003.1"  "0000.0"  "-2345.7" "10000.0" "-000.3"

``` r
sapply(n_ex, function(x) sprintf(fmt = "% .1f", x))
```

    ## [1] " 3.1"     " 0.0"     "-2345.7"  " 10000.0" "-0.3"

``` r
formatC(n_ex, digits = 1, format = "f", flag = " ")
```

    ## [1] " 3.1"     " 0.0"     "-2345.7"  " 10000.0" "-0.3"

``` r
sapply(n_ex, function(x) sprintf(fmt = "%+.1f", x))
```

    ## [1] "+3.1"     "+0.0"     "-2345.7"  "+10000.0" "-0.3"

``` r
formatC(n_ex, digits = 1, format = "f", flag = "+")
```

    ## [1] "+3.1"     "+0.0"     "-2345.7"  "+10000.0" "-0.3"

``` r
formatC(n_ex, digits = 1, format = "f", big.mark = ",")
```

    ## [1] "3.1"      "0.0"      "-2,345.7" "10,000.0" "-0.3"

### Width

When using `format()`, all number have the same width in characters

``` r
format(n_ex, digits = 2, scientific = FALSE)
```

    ## [1] "    3.14" "    0.01" "-2345.68" "10000.00" "   -0.33"

``` r
format(n_ex, digits = 2)
```

    ## [1] "    3.14" "    0.01" "-2345.68" "10000.00" "   -0.33"

We can eliminate it using trim

``` r
format(n_ex, digits = 2, scientific = FALSE, trim = TRUE)
```

    ## [1] "3.14"     "0.01"     "-2345.68" "10000.00" "-0.33"

``` r
formatC(n_ex, digits = 2, format = "f")
```

    ## [1] "3.14"     "0.01"     "-2345.68" "10000.00" "-0.33"

We can extend the number of characters included using width

``` r
formatC(n_ex, digits = 2, format = "f", width = 10)
```

    ## [1] "      3.14" "      0.01" "  -2345.68" "  10000.00" "     -0.33"

``` r
as.matrix(formatC(n_ex, digits = 2, format = "f", width = 10), 5,1)
```

    ##      [,1]        
    ## [1,] "      3.14"
    ## [2,] "      0.01"
    ## [3,] "  -2345.68"
    ## [4,] "  10000.00"
    ## [5,] "     -0.33"

### Units

When reporting numbers is common to report them in as multiples or
fractions: Thousands (K), millions (M) or 1 per 10,000 as examples. To
get this, we first divide/multiple the numbers by the factor, format the
resulting number and finally, add the unit.

``` r
f_scale <- 1e-3
n_ex * f_scale
```

    ## [1]  0.0031415927  0.0000100000 -2.3456789000 10.0000000000 -0.0003333333

``` r
format(n_ex * f_scale, digits = 1, scientific = FALSE)
```

    ## [1] " 0.00314" " 0.00001" "-2.34568" "10.00000" "-0.00033"

``` r
paste0(formatC(n_ex * f_scale, digits = 1, format = "f", 
  drop0trailing = TRUE, flag = "+"), "K")
```

    ## [1] "+0K"   "+0K"   "-2.3K" "+10K"  "-0K"

``` r
sapply(n_ex * f_scale, function(x) sprintf("%+.1fK",x))
```

    ## [1] "+0.0K"  "+0.0K"  "-2.3K"  "+10.0K" "-0.0K"

### Prefix

When working with currencies, we want to include the currency symbol as
a prefix

``` r
paste0("$", formatC(n_ex , digits = 1, format = "f", 
  drop0trailing = TRUE, flag = ""))
```

    ## [1] "$3.1"     "$0"       "$-2345.7" "$10000"   "$-0.3"

``` r
sapply(n_ex, function(x) {
  if(x<0) {
    sprintf("$\U2212%.1f",abs(x))
  } else {
    sprintf("$%+.1f",x)
  }
})
```

    ## [1] "$+3.1"     "$+0.0"     "$-2345.7"  "$+10000.0" "$-0.3"

Note the minus sign employed is larger than the default

### Percentage

To report percentage, we combine previous examples

``` r
f_scale <- 1e2
n_ex * f_scale
```

    ## [1]     314.15927       1.00000 -234567.89000 1000000.00000     -33.33333

``` r
format(n_ex * f_scale, digits = 7, scientific = FALSE, trim= TRUE)
```

    ## [1] "314.15927"     "1.00000"       "-234567.89000" "1000000.00000"
    ## [5] "-33.33333"

``` r
paste0(formatC(n_ex * f_scale, digits = 1, format = "f", 
  flag = ""), "%")
```

    ## [1] "314.2%"     "1.0%"       "-234567.9%" "1000000.0%" "-33.3%"

``` r
sapply(n_ex * f_scale, function(x) sprintf("%.1f%%",x))
```

    ## [1] "314.2%"     "1.0%"       "-234567.9%" "1000000.0%" "-33.3%"

## Kable

``` r
df <- data.frame(x = runif(4), y = 10^seq(1,7,2), z = runif(4, 0, 1), t=rnorm(4, 0, 40))
```

``` r
knitr::kable(df,digits = 2, format.args = list(big.mark = ",", scientific = FALSE))
```

|    x |          y |    z |      t |
|-----:|-----------:|-----:|-------:|
| 0.25 |         10 | 0.72 | -31.78 |
| 0.12 |      1,000 | 0.92 |  41.15 |
| 0.76 |    100,000 | 0.85 |  54.86 |
| 0.33 | 10,000,000 | 0.13 | -45.06 |

``` r
knitr::kable(df,digits = 2, format.args = list(big.mark = ","))
```

|    x |     y |    z |      t |
|-----:|------:|-----:|-------:|
| 0.25 | 1e+01 | 0.72 | -31.78 |
| 0.12 | 1e+03 | 0.92 |  41.15 |
| 0.76 | 1e+05 | 0.85 |  54.86 |
| 0.33 | 1e+07 | 0.13 | -45.06 |

``` r
minus_sign <- data.frame(character = c("-", "\U0335", "\U0336"))
```

``` r
minus_sign
```

    ##   character
    ## 1         -
    ## 2  <U+0335>
    ## 3  <U+0336>

``` r
df |> 
  mutate(x=paste0(formatC(x * 100, digits = 1, format = "f", flag = ""), "%"),
    y=paste0("$", formatC(y*1e-3, digits = 1, format= "f", flag = "", big.mark = ","), "K"),
    t = if_else(t<0, sprintf("\U2212$%.1f",abs(t)), sprintf("$%.1f",t))) |> 
knitr::kable(digits = 2, align = "r")
```

|     x |          y |    z |      t |
|------:|-----------:|-----:|-------:|
| 25.4% |      $0.0K | 0.72 | -$31.8 |
| 12.4% |      $1.0K | 0.92 |  $41.2 |
| 75.9% |    $100.0K | 0.85 |  $54.9 |
| 32.8% | $10,000.0K | 0.13 | -$45.1 |

``` r
minus_sign |> knitr::kable()
```

| character  |
|:-----------|
| \-         |
| \<U+0335\> |
| \<U+0336\> |
