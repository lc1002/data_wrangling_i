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
