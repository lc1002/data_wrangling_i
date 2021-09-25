Data Import
================

``` r
library(tidyverse)
```

    ## -- Attaching packages --------------------------------------- tidyverse 1.3.1 --

    ## v ggplot2 3.3.5     v purrr   0.3.4
    ## v tibble  3.1.4     v dplyr   1.0.7
    ## v tidyr   1.1.3     v stringr 1.4.0
    ## v readr   2.0.1     v forcats 0.5.1

    ## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library(readxl)
library(haven)
```

## Import some data

I want to import `FAS_litters.csv`.

``` r
litters_df = read.csv("data/FAS_litters.csv")
```

Yay! I imported the dataset. Now I want better names.

janitor::clean\_names(litters\_df) –&gt; using the function without
loading the entire package. Or load janitor package using library

``` r
names(litters_df)
```

    ## [1] "Group"             "Litter.Number"     "GD0.weight"       
    ## [4] "GD18.weight"       "GD.of.Birth"       "Pups.born.alive"  
    ## [7] "Pups.dead...birth" "Pups.survive"

``` r
litters_df = janitor::clean_names(litters_df)
```

Yay! Now I have better names. Let’s look at the dataset.

## Take a look at the data

printing in the console.

``` r
litters_df
```

    ##    group   litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ## 1   Con7             #85       19.7        34.7          20               3
    ## 2   Con7       #1/2/95/2       27.0        42.0          19               8
    ## 3   Con7   #5/5/3/83/3-3       26.0        41.4          19               6
    ## 4   Con7     #5/4/2/95/2       28.5        44.1          19               5
    ## 5   Con7     #4/2/95/3-3         NA          NA          20               6
    ## 6   Con7     #2/2/95/3-2         NA          NA          20               6
    ## 7   Con7 #1/5/3/83/3-3/2         NA          NA          20               9
    ## 8   Con8       #3/83/3-3         NA          NA          20               9
    ## 9   Con8         #2/95/3         NA          NA          20               8
    ## 10  Con8     #3/5/2/2/95       28.5          NA          20               8
    ## 11  Con8     #5/4/3/83/3       28.0          NA          19               9
    ## 12  Con8   #1/6/2/2/95-2         NA          NA          20               7
    ## 13  Con8 #3/5/3/83/3-3-2         NA          NA          20               8
    ## 14  Con8       #2/2/95/2         NA          NA          19               5
    ## 15  Con8   #3/6/2/2/95-3         NA          NA          20               7
    ## 16  Mod7             #59       17.0        33.4          19               8
    ## 17  Mod7            #103       21.4        42.1          19               9
    ## 18  Mod7       #1/82/3-2         NA          NA          19               6
    ## 19  Mod7       #3/83/3-2         NA          NA          19               8
    ## 20  Mod7       #2/95/2-2         NA          NA          20               7
    ## 21  Mod7       #3/82/3-2       28.0        45.9          20               5
    ## 22  Mod7       #4/2/95/2       23.5          NA          19               9
    ## 23  Mod7     #5/3/83/5-2       22.6        37.0          19               5
    ## 24  Mod7      #8/110/3-2         NA          NA          20               9
    ## 25  Mod7            #106       21.7        37.8          20               5
    ## 26  Mod7           #94/2       24.4        42.9          19               7
    ## 27  Mod7             #62       19.5        35.9          19               7
    ## 28  Low7           #84/2       24.3        40.8          20               8
    ## 29  Low7            #107       22.6        42.4          20               9
    ## 30  Low7           #85/2       22.2        38.5          20               8
    ## 31  Low7             #98       23.8        43.8          20               9
    ## 32  Low7            #102       22.6        43.3          20              11
    ## 33  Low7            #101       23.8        42.7          20               9
    ## 34  Low7            #111       25.5        44.6          20               3
    ## 35  Low7            #112       23.9        40.5          19               6
    ## 36  Mod8             #97       24.5        42.8          20               8
    ## 37  Mod8           #5/93         NA        41.1          20              11
    ## 38  Mod8         #5/93/2         NA          NA          19               8
    ## 39  Mod8       #7/82-3-2       26.9        43.2          20               7
    ## 40  Mod8      #7/110/3-2       27.5        46.0          19               8
    ## 41  Mod8         #2/95/2       28.5        44.5          20               9
    ## 42  Mod8           #82/4       33.4        52.7          20               8
    ## 43  Low8             #53       21.8        37.2          20               8
    ## 44  Low8             #79       25.4        43.8          19               8
    ## 45  Low8            #100       20.0        39.2          20               8
    ## 46  Low8           #4/84       21.8        35.2          20               4
    ## 47  Low8            #108       25.6        47.5          20               8
    ## 48  Low8             #99       23.5        39.0          20               6
    ## 49  Low8            #110       25.5        42.7          20               7
    ##    pups_dead_birth pups_survive
    ## 1                4            3
    ## 2                0            7
    ## 3                0            5
    ## 4                1            4
    ## 5                0            6
    ## 6                0            4
    ## 7                0            9
    ## 8                1            8
    ## 9                0            8
    ## 10               0            8
    ## 11               0            8
    ## 12               0            6
    ## 13               0            8
    ## 14               0            4
    ## 15               0            7
    ## 16               0            5
    ## 17               1            9
    ## 18               0            6
    ## 19               0            8
    ## 20               0            7
    ## 21               0            5
    ## 22               0            7
    ## 23               0            5
    ## 24               0            9
    ## 25               0            2
    ## 26               1            3
    ## 27               2            4
    ## 28               0            8
    ## 29               0            8
    ## 30               0            6
    ## 31               0            9
    ## 32               0            7
    ## 33               0            9
    ## 34               2            3
    ## 35               1            1
    ## 36               1            8
    ## 37               0            9
    ## 38               0            8
    ## 39               0            7
    ## 40               1            8
    ## 41               0            9
    ## 42               0            6
    ## 43               1            7
    ## 44               0            7
    ## 45               0            7
    ## 46               0            4
    ## 47               0            7
    ## 48               0            5
    ## 49               0            6

``` r
# print out the first few rows of the data.
head(litters_df)
```

    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ## 1  Con7           #85       19.7        34.7          20               3
    ## 2  Con7     #1/2/95/2       27.0        42.0          19               8
    ## 3  Con7 #5/5/3/83/3-3       26.0        41.4          19               6
    ## 4  Con7   #5/4/2/95/2       28.5        44.1          19               5
    ## 5  Con7   #4/2/95/3-3         NA          NA          20               6
    ## 6  Con7   #2/2/95/3-2         NA          NA          20               6
    ##   pups_dead_birth pups_survive
    ## 1               4            3
    ## 2               0            7
    ## 3               0            5
    ## 4               1            4
    ## 5               0            6
    ## 6               0            4

``` r
#print out the last few rows of the data.
tail(litters_df)
```

    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ## 44  Low8           #79       25.4        43.8          19               8
    ## 45  Low8          #100       20.0        39.2          20               8
    ## 46  Low8         #4/84       21.8        35.2          20               4
    ## 47  Low8          #108       25.6        47.5          20               8
    ## 48  Low8           #99       23.5        39.0          20               6
    ## 49  Low8          #110       25.5        42.7          20               7
    ##    pups_dead_birth pups_survive
    ## 44               0            7
    ## 45               0            7
    ## 46               0            4
    ## 47               0            7
    ## 48               0            5
    ## 49               0            6

One I use a lot is:

``` r
view(litters_df) ## put it in the console often not the code chunk, might make knitting process a little difficult.
```

Here’s `skimr`:

``` r
skimr::skim(litters_df)
```

|                                                  |             |
|:-------------------------------------------------|:------------|
| Name                                             | litters\_df |
| Number of rows                                   | 49          |
| Number of columns                                | 8           |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_   |             |
| Column type frequency:                           |             |
| character                                        | 2           |
| numeric                                          | 6           |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ |             |
| Group variables                                  | None        |

Data summary

**Variable type: character**

| skim\_variable | n\_missing | complete\_rate | min | max | empty | n\_unique | whitespace |
|:---------------|-----------:|---------------:|----:|----:|------:|----------:|-----------:|
| group          |          0 |              1 |   4 |   4 |     0 |         6 |          0 |
| litter\_number |          0 |              1 |   3 |  15 |     0 |        49 |          0 |

**Variable type: numeric**

| skim\_variable    | n\_missing | complete\_rate |  mean |   sd |   p0 |   p25 |   p50 |   p75 | p100 | hist  |
|:------------------|-----------:|---------------:|------:|-----:|-----:|------:|------:|------:|-----:|:------|
| gd0\_weight       |         15 |           0.69 | 24.38 | 3.28 | 17.0 | 22.30 | 24.10 | 26.67 | 33.4 | ▃▇▇▆▁ |
| gd18\_weight      |         17 |           0.65 | 41.52 | 4.05 | 33.4 | 38.88 | 42.25 | 43.80 | 52.7 | ▃▃▇▂▁ |
| gd\_of\_birth     |          0 |           1.00 | 19.65 | 0.48 | 19.0 | 19.00 | 20.00 | 20.00 | 20.0 | ▅▁▁▁▇ |
| pups\_born\_alive |          0 |           1.00 |  7.35 | 1.76 |  3.0 |  6.00 |  8.00 |  8.00 | 11.0 | ▁▃▂▇▁ |
| pups\_dead\_birth |          0 |           1.00 |  0.33 | 0.75 |  0.0 |  0.00 |  0.00 |  0.00 |  4.0 | ▇▂▁▁▁ |
| pups\_survive     |          0 |           1.00 |  6.41 | 2.05 |  1.0 |  5.00 |  7.00 |  8.00 |  9.0 | ▁▃▂▇▇ |

## Arguments in`read_csv`

``` r
litters_df = 
  read_csv(
    "data/FAS_litters.csv",
    skip = 5,
    col_names = FALSE, 
    show_col_types = FALSE, 
    na = "Low8")
```

check out `?read_csv()` for more information.

## Parsing columns

Not listing every single column. Specify a single column

``` r
litters_data = read_csv(file = "./data/FAS_litters.csv",
  col_types = cols(`GD of Birth` = col_integer()))
```

## Reading from Excel

Reading MLB data.

range –&gt; reading from A1 through F7 subtable.

``` r
mlb11_df = read_excel("data/mlb11.xlsx", range = "A1:F7")
mlb11_df
```

    ## # A tibble: 6 x 6
    ##   team                 runs at_bats  hits homeruns bat_avg
    ##   <chr>               <dbl>   <dbl> <dbl>    <dbl>   <dbl>
    ## 1 Texas Rangers         855    5659  1599      210   0.283
    ## 2 Boston Red Sox        875    5710  1600      203   0.28 
    ## 3 Detroit Tigers        787    5563  1540      169   0.277
    ## 4 Kansas City Royals    730    5672  1560      129   0.275
    ## 5 St. Louis Cardinals   762    5532  1513      162   0.273
    ## 6 New York Mets         718    5600  1477      108   0.264

LorR Words is next.

``` r
fellow_df = read_excel("data/LotR_Words.xlsx",range = "B3:D6")
```

## Read a SAS file.

This is where I tried to read a SAS file. Given out first 10 rows.

``` r
pulse_df = read_sas("data/public_pulse_data.sas7bdat")
pulse_df
```

    ## # A tibble: 1,087 x 7
    ##       ID   age Sex    BDIScore_BL BDIScore_01m BDIScore_06m BDIScore_12m
    ##    <dbl> <dbl> <chr>        <dbl>        <dbl>        <dbl>        <dbl>
    ##  1 10003  48.0 male             7            1            2            0
    ##  2 10015  72.5 male             6           NA           NA           NA
    ##  3 10022  58.5 male            14            3            8           NA
    ##  4 10026  72.7 male            20            6           18           16
    ##  5 10035  60.4 male             4            0            1            2
    ##  6 10050  84.7 male             2           10           12            8
    ##  7 10078  31.3 male             4            0           NA           NA
    ##  8 10088  56.9 male             5           NA            0            2
    ##  9 10091  76.0 male             0            3            4            0
    ## 10 10092  74.2 female          10            2           11            6
    ## # ... with 1,077 more rows

## Comparison with Base R

what about `read.csv` …?WHY TO NEVER `read.csv`

read.csv –&gt; read the data out as data frame not tibble.

`read.csv` doesn’t give `tibble` and that’s really bad.

``` r
litters_df_base = read.csv("data/FAS_litters.csv")
```
