Tidy Data
================

## pivot longer

Load the PULSE data.

``` r
pulse_df = 
  read_sas("data/public_pulse_data.sas7bdat") %>% 
  janitor::clean_names()
```

Let’s try to pivot

``` r
pulse_tidy = 
  pulse_df %>% 
  pivot_longer(
    bdi_score_bl:bdi_score_12m,
    names_to = "visit", 
    names_prefix = "bdi_score_",  ## getting rid off the name in the column
    values_to = "bdi"
  ) %>% 
  mutate(
    visit = replace(visit, visit == "bl", "00m"),
    visit = factor(visit) ## converting visit to factor variable
  )
```

## pivot\_wider

make up a results data table

``` r
analysis_df = 
  tibble(
    group = c("treatment", "treatment", "control", "control"),
    time = c("a", "b", "a", "b"),
    group_mean = c(4, 8, 3, 6)
  )

analysis_df %>% 
  pivot_wider(
    names_from = "time", 
    values_from = "group_mean"
  ) %>% 
  knitr::kable() ## knitr make it into a table
```

| group     |   a |   b |
|:----------|----:|----:|
| treatment |   4 |   8 |
| control   |   3 |   6 |

## bind\_rows

import the LotR movie words stuff

``` r
fellowship_df = 
  read_excel("data/LotR_Words.xlsx", range = "B3:D6") %>% 
  mutate(movie = "fellowship_rings") 

two_towers_df = 
  read_excel("data/LotR_Words.xlsx", range = "F3:H6") %>% 
  mutate(movie = "two_towers")

return_df = 
  read_excel("data/LotR_Words.xlsx", range = "J3:L6") %>% 
  mutate(movie = "return_king")

lotr_df = 
  bind_rows(fellowship_df, two_towers_df, return_df) %>% 
  janitor::clean_names() %>% 
  pivot_longer(
    female:male,
    names_to = "sex",
    values_to = "words"
  ) %>% 
  mutate(race = str_to_lower(race)) %>% 
  relocate(movie)
```

`rbind` is notably worse than `bind_rows`. never use `rbind()`, always
use `bind_rows`.

## joins

Look at FAS data. This imports and cleans litters and pups data.

``` r
# a problem with litters dataset is the group column, there are two variables in the column that are squished together

litters_df = 
  read_csv("data/FAS_litters.csv") %>% 
  janitor::clean_names() %>% 
  separate(group, into = c("dose", "day_of_tx"), 3) %>% 
  relocate(litter_number) %>% 
  mutate(dose = str_to_lower(dose))
```

    ## Rows: 49 Columns: 8

    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## chr (2): Group, Litter Number
    ## dbl (6): GD0 weight, GD18 weight, GD of Birth, Pups born alive, Pups dead @ ...

    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
pups_df = 
  read_csv("data/FAS_pups.csv") %>% 
  janitor::clean_names() %>% 
  mutate(
    sex = recode(sex, `1` = "male", `2` = "female"), ## putting `` makes r realize it's variable not a number
    sex = factor(sex)) 
```

    ## Rows: 313 Columns: 6

    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## chr (1): Litter Number
    ## dbl (5): Sex, PD ears, PD eyes, PD pivot, PD walk

    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

Let’s join these up!

``` r
fas_df = 
  left_join(pups_df, litters_df, by = "litter_number") %>% 
  relocate(litter_number, dose, day_of_tx)
```

`gather` and `spread` instead of `pivot_longer` and `pivot_wider`. The
new functions were updated for good reasons; `gather` and `spread` will
still exist, but they’re going to be less common over time.

## Learning Assignments

This code chunk will import `surv_os` and `surv_program_git` data and
cleans both datasets, and then joins them.

``` r
surv_os = read_csv("survey_results/surv_os.csv") %>% 
  janitor::clean_names() %>% 
  rename(id = what_is_your_uni, os = what_operating_system_do_you_use)
```

    ## Rows: 173 Columns: 2

    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## chr (2): What is your UNI?, What operating system do you use?

    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
surv_pr_git = read_csv("survey_results/surv_program_git.csv") %>% 
  janitor::clean_names() %>% 
  rename(
    id = what_is_your_uni, 
    prog = what_is_your_degree_program,
    git_exp = which_most_accurately_describes_your_experience_with_git)
```

    ## Rows: 135 Columns: 3

    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## chr (3): What is your UNI?, What is your degree program?, Which most accurat...

    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
left_join(surv_os, surv_pr_git)
```

    ## Joining, by = "id"

    ## # A tibble: 175 x 4
    ##    id          os         prog  git_exp                                         
    ##    <chr>       <chr>      <chr> <chr>                                           
    ##  1 student_87  <NA>       MS    Pretty smooth: needed some work to connect Git,~
    ##  2 student_106 Windows 10 Other Pretty smooth: needed some work to connect Git,~
    ##  3 student_66  Mac OS X   MPH   Smooth: installation and connection with GitHub~
    ##  4 student_93  Windows 10 MS    Smooth: installation and connection with GitHub~
    ##  5 student_99  Mac OS X   MS    Smooth: installation and connection with GitHub~
    ##  6 student_115 Mac OS X   MS    Smooth: installation and connection with GitHub~
    ##  7 student_15  Windows 10 MPH   Pretty smooth: needed some work to connect Git,~
    ##  8 student_15  Windows 10 MPH   Pretty smooth: needed some work to connect Git,~
    ##  9 student_21  Windows 10 MPH   Pretty smooth: needed some work to connect Git,~
    ## 10 student_86  Mac OS X   <NA>  <NA>                                            
    ## # ... with 165 more rows

``` r
inner_join(surv_os, surv_pr_git)
```

    ## Joining, by = "id"

    ## # A tibble: 129 x 4
    ##    id          os         prog  git_exp                                         
    ##    <chr>       <chr>      <chr> <chr>                                           
    ##  1 student_87  <NA>       MS    Pretty smooth: needed some work to connect Git,~
    ##  2 student_106 Windows 10 Other Pretty smooth: needed some work to connect Git,~
    ##  3 student_66  Mac OS X   MPH   Smooth: installation and connection with GitHub~
    ##  4 student_93  Windows 10 MS    Smooth: installation and connection with GitHub~
    ##  5 student_99  Mac OS X   MS    Smooth: installation and connection with GitHub~
    ##  6 student_115 Mac OS X   MS    Smooth: installation and connection with GitHub~
    ##  7 student_15  Windows 10 MPH   Pretty smooth: needed some work to connect Git,~
    ##  8 student_15  Windows 10 MPH   Pretty smooth: needed some work to connect Git,~
    ##  9 student_21  Windows 10 MPH   Pretty smooth: needed some work to connect Git,~
    ## 10 student_59  Windows 10 MPH   Smooth: installation and connection with GitHub~
    ## # ... with 119 more rows

``` r
anti_join(surv_os, surv_pr_git)
```

    ## Joining, by = "id"

    ## # A tibble: 46 x 2
    ##    id          os                                     
    ##    <chr>       <chr>                                  
    ##  1 student_86  Mac OS X                               
    ##  2 student_91  Windows 10                             
    ##  3 student_24  Mac OS X                               
    ##  4 student_103 Mac OS X                               
    ##  5 student_163 Mac OS X                               
    ##  6 student_68  Other (Linux, Windows, 95, TI-89+, etc)
    ##  7 student_158 Mac OS X                               
    ##  8 student_19  Windows 10                             
    ##  9 student_43  Mac OS X                               
    ## 10 student_78  Mac OS X                               
    ## # ... with 36 more rows

``` r
anti_join(surv_pr_git, surv_os)
```

    ## Joining, by = "id"

    ## # A tibble: 15 x 3
    ##    id         prog  git_exp                                                     
    ##    <chr>      <chr> <chr>                                                       
    ##  1 <NA>       MPH   "Pretty smooth: needed some work to connect Git, GitHub, an~
    ##  2 student_17 PhD   "Pretty smooth: needed some work to connect Git, GitHub, an~
    ##  3 <NA>       MPH   "Pretty smooth: needed some work to connect Git, GitHub, an~
    ##  4 <NA>       MPH   "Pretty smooth: needed some work to connect Git, GitHub, an~
    ##  5 <NA>       MS    "Pretty smooth: needed some work to connect Git, GitHub, an~
    ##  6 student_53 MS    "Pretty smooth: needed some work to connect Git, GitHub, an~
    ##  7 <NA>       MS    "Smooth: installation and connection with GitHub was easy"  
    ##  8 student_80 PhD   "Pretty smooth: needed some work to connect Git, GitHub, an~
    ##  9 student_16 MPH   "Smooth: installation and connection with GitHub was easy"  
    ## 10 student_98 MS    "Smooth: installation and connection with GitHub was easy"  
    ## 11 <NA>       MS    "Pretty smooth: needed some work to connect Git, GitHub, an~
    ## 12 <NA>       MS    "What's \"Git\" ...?"                                       
    ## 13 <NA>       MS    "Smooth: installation and connection with GitHub was easy"  
    ## 14 <NA>       MPH   "Pretty smooth: needed some work to connect Git, GitHub, an~
    ## 15 <NA>       MS    "Pretty smooth: needed some work to connect Git, GitHub, an~
