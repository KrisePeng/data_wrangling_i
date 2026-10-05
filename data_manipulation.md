data_manipulation
================

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.2.1     ✔ readr     2.2.0
    ## ✔ forcats   1.0.1     ✔ stringr   1.6.0
    ## ✔ ggplot2   4.0.3     ✔ tibble    3.3.1
    ## ✔ lubridate 1.9.5     ✔ tidyr     1.3.2
    ## ✔ purrr     1.2.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
litters_df = read_csv("./data_import_examples/FAS_litters.csv",
                      na = c("NA","."," "))
```

    ## Warning: One or more parsing issues, call `problems()` on your data frame for details,
    ## e.g.:
    ##   dat <- vroom(...)
    ##   problems(dat)

    ## Rows: 49 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (2): Group, Litter Number
    ## dbl (6): GD0 weight, GD18 weight, GD of Birth, Pups born alive, Pups dead @ ...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
litters_df = janitor::clean_names(litters_df)
pups_df = read_csv("./data_import_examples/FAS_pups.csv",
                   skip = 3,
                   na = c("NA","."))
```

    ## Rows: 313 Columns: 6
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): Litter Number
    ## dbl (5): Sex, PD ears, PD eyes, PD pivot, PD walk
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
pups_df = janitor::clean_names(pups_df)
names(pups_df)
```

    ## [1] "litter_number" "sex"           "pd_ears"       "pd_eyes"      
    ## [5] "pd_pivot"      "pd_walk"

# Select

You can specify the columns you want to keep by naming all of them:

``` r
select(litters_df, group, litter_number, gd0_weight, pups_born_alive)
```

    ## # A tibble: 49 × 4
    ##    group litter_number   gd0_weight pups_born_alive
    ##    <chr> <chr>                <dbl>           <dbl>
    ##  1 Con7  #85                   19.7               3
    ##  2 Con7  #1/2/95/2             27                 8
    ##  3 Con7  #5/5/3/83/3-3         26                 6
    ##  4 Con7  #5/4/2/95/2           28.5               5
    ##  5 Con7  #4/2/95/3-3           NA                 6
    ##  6 Con7  #2/2/95/3-2           NA                 6
    ##  7 Con7  #1/5/3/83/3-3/2       NA                 9
    ##  8 Con8  #3/83/3-3             NA                 9
    ##  9 Con8  #2/95/3               NA                 8
    ## 10 Con8  #3/5/2/2/95           28.5               8
    ## # ℹ 39 more rows

You can specify the specify a range of columns to keep:

``` r
select(litters_df, group:gd_of_birth)
```

    ## # A tibble: 49 × 5
    ##    group litter_number   gd0_weight gd18_weight gd_of_birth
    ##    <chr> <chr>                <dbl>       <dbl>       <dbl>
    ##  1 Con7  #85                   19.7        34.7          20
    ##  2 Con7  #1/2/95/2             27          42            19
    ##  3 Con7  #5/5/3/83/3-3         26          41.4          19
    ##  4 Con7  #5/4/2/95/2           28.5        44.1          19
    ##  5 Con7  #4/2/95/3-3           NA          NA            20
    ##  6 Con7  #2/2/95/3-2           NA          NA            20
    ##  7 Con7  #1/5/3/83/3-3/2       NA          NA            20
    ##  8 Con8  #3/83/3-3             NA          NA            20
    ##  9 Con8  #2/95/3               NA          NA            20
    ## 10 Con8  #3/5/2/2/95           28.5        NA            20
    ## # ℹ 39 more rows

You can also specify columns you’d like to remove:

``` r
select(litters_df, -pups_survive)
```

    ## # A tibble: 49 × 7
    ##    group litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>                <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Con7  #85                   19.7        34.7          20               3
    ##  2 Con7  #1/2/95/2             27          42            19               8
    ##  3 Con7  #5/5/3/83/3-3         26          41.4          19               6
    ##  4 Con7  #5/4/2/95/2           28.5        44.1          19               5
    ##  5 Con7  #4/2/95/3-3           NA          NA            20               6
    ##  6 Con7  #2/2/95/3-2           NA          NA            20               6
    ##  7 Con7  #1/5/3/83/3-3/2       NA          NA            20               9
    ##  8 Con8  #3/83/3-3             NA          NA            20               9
    ##  9 Con8  #2/95/3               NA          NA            20               8
    ## 10 Con8  #3/5/2/2/95           28.5        NA            20               8
    ## # ℹ 39 more rows
    ## # ℹ 1 more variable: pups_dead_birth <dbl>

You can rename variables as part of this process:

``` r
select(litters_df, GROUP = group, LiTtEr_NuMbEr = litter_number)
```

    ## # A tibble: 49 × 2
    ##    GROUP LiTtEr_NuMbEr  
    ##    <chr> <chr>          
    ##  1 Con7  #85            
    ##  2 Con7  #1/2/95/2      
    ##  3 Con7  #5/5/3/83/3-3  
    ##  4 Con7  #5/4/2/95/2    
    ##  5 Con7  #4/2/95/3-3    
    ##  6 Con7  #2/2/95/3-2    
    ##  7 Con7  #1/5/3/83/3-3/2
    ##  8 Con8  #3/83/3-3      
    ##  9 Con8  #2/95/3        
    ## 10 Con8  #3/5/2/2/95    
    ## # ℹ 39 more rows

If all you want to do is rename something, you can use rename instead of
select. This will rename the variables you care about, and keep
everything else:

``` r
rename(litters_df, GROUP = group, LiTtEr_NuMbEr = litter_number)
```

    ## # A tibble: 49 × 8
    ##    GROUP LiTtEr_NuMbEr   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>                <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Con7  #85                   19.7        34.7          20               3
    ##  2 Con7  #1/2/95/2             27          42            19               8
    ##  3 Con7  #5/5/3/83/3-3         26          41.4          19               6
    ##  4 Con7  #5/4/2/95/2           28.5        44.1          19               5
    ##  5 Con7  #4/2/95/3-3           NA          NA            20               6
    ##  6 Con7  #2/2/95/3-2           NA          NA            20               6
    ##  7 Con7  #1/5/3/83/3-3/2       NA          NA            20               9
    ##  8 Con8  #3/83/3-3             NA          NA            20               9
    ##  9 Con8  #2/95/3               NA          NA            20               8
    ## 10 Con8  #3/5/2/2/95           28.5        NA            20               8
    ## # ℹ 39 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

everything(), which is handy for reorganizing columns without discarding
anything:

``` r
select(litters_df, litter_number, pups_survive, everything())
```

    ## # A tibble: 49 × 8
    ##    litter_number   pups_survive group gd0_weight gd18_weight gd_of_birth
    ##    <chr>                  <dbl> <chr>      <dbl>       <dbl>       <dbl>
    ##  1 #85                        3 Con7        19.7        34.7          20
    ##  2 #1/2/95/2                  7 Con7        27          42            19
    ##  3 #5/5/3/83/3-3              5 Con7        26          41.4          19
    ##  4 #5/4/2/95/2                4 Con7        28.5        44.1          19
    ##  5 #4/2/95/3-3                6 Con7        NA          NA            20
    ##  6 #2/2/95/3-2                4 Con7        NA          NA            20
    ##  7 #1/5/3/83/3-3/2            9 Con7        NA          NA            20
    ##  8 #3/83/3-3                  8 Con8        NA          NA            20
    ##  9 #2/95/3                    8 Con8        NA          NA            20
    ## 10 #3/5/2/2/95                8 Con8        28.5        NA            20
    ## # ℹ 39 more rows
    ## # ℹ 2 more variables: pups_born_alive <dbl>, pups_dead_birth <dbl>

``` r
# put litter_number and pups_survive in first two columns, and then keep all the other columns in their original order  without repeating.
```

‘relocate’ does a similar thing (and is sort of like rename in that it’s
handy but not critical):

if your only goal is reordering columns, relocate() is more direct.

``` r
relocate(litters_df, litter_number, pups_survive)
```

    ## # A tibble: 49 × 8
    ##    litter_number   pups_survive group gd0_weight gd18_weight gd_of_birth
    ##    <chr>                  <dbl> <chr>      <dbl>       <dbl>       <dbl>
    ##  1 #85                        3 Con7        19.7        34.7          20
    ##  2 #1/2/95/2                  7 Con7        27          42            19
    ##  3 #5/5/3/83/3-3              5 Con7        26          41.4          19
    ##  4 #5/4/2/95/2                4 Con7        28.5        44.1          19
    ##  5 #4/2/95/3-3                6 Con7        NA          NA            20
    ##  6 #2/2/95/3-2                4 Con7        NA          NA            20
    ##  7 #1/5/3/83/3-3/2            9 Con7        NA          NA            20
    ##  8 #3/83/3-3                  8 Con8        NA          NA            20
    ##  9 #2/95/3                    8 Con8        NA          NA            20
    ## 10 #3/5/2/2/95                8 Con8        28.5        NA            20
    ## # ℹ 39 more rows
    ## # ℹ 2 more variables: pups_born_alive <dbl>, pups_dead_birth <dbl>

``` r
# You can also move columns somewhere else, for example:
relocate(litters_df, pups_survive, .after = litter_number)
```

    ## # A tibble: 49 × 8
    ##    group litter_number   pups_survive gd0_weight gd18_weight gd_of_birth
    ##    <chr> <chr>                  <dbl>      <dbl>       <dbl>       <dbl>
    ##  1 Con7  #85                        3       19.7        34.7          20
    ##  2 Con7  #1/2/95/2                  7       27          42            19
    ##  3 Con7  #5/5/3/83/3-3              5       26          41.4          19
    ##  4 Con7  #5/4/2/95/2                4       28.5        44.1          19
    ##  5 Con7  #4/2/95/3-3                6       NA          NA            20
    ##  6 Con7  #2/2/95/3-2                4       NA          NA            20
    ##  7 Con7  #1/5/3/83/3-3/2            9       NA          NA            20
    ##  8 Con8  #3/83/3-3                  8       NA          NA            20
    ##  9 Con8  #2/95/3                    8       NA          NA            20
    ## 10 Con8  #3/5/2/2/95                8       28.5        NA            20
    ## # ℹ 39 more rows
    ## # ℹ 2 more variables: pups_born_alive <dbl>, pups_dead_birth <dbl>

``` r
# means:
# move pups_survive to immediately after litter_number.
```

## Learning Assessment:

``` r
select(pups_df, litter_number,sex,pd_ears)
```

    ## # A tibble: 313 × 3
    ##    litter_number   sex pd_ears
    ##    <chr>         <dbl>   <dbl>
    ##  1 #85               1       4
    ##  2 #85               1       4
    ##  3 #1/2/95/2         1       5
    ##  4 #1/2/95/2         1       5
    ##  5 #5/5/3/83/3-3     1       5
    ##  6 #5/5/3/83/3-3     1       5
    ##  7 #5/4/2/95/2       1      NA
    ##  8 #4/2/95/3-3       1       4
    ##  9 #4/2/95/3-3       1       4
    ## 10 #2/2/95/3-2       1       4
    ## # ℹ 303 more rows

# Filter

``` r
pups_df <- pups_df%>%
  dplyr::filter(
    sex == 1
)
dplyr::filter(pups_df, sex == 1)
```

    ## # A tibble: 155 × 6
    ##    litter_number   sex pd_ears pd_eyes pd_pivot pd_walk
    ##    <chr>         <dbl>   <dbl>   <dbl>    <dbl>   <dbl>
    ##  1 #85               1       4      13        7      11
    ##  2 #85               1       4      13        7      12
    ##  3 #1/2/95/2         1       5      13        7       9
    ##  4 #1/2/95/2         1       5      13        8      10
    ##  5 #5/5/3/83/3-3     1       5      13        8      10
    ##  6 #5/5/3/83/3-3     1       5      14        6       9
    ##  7 #5/4/2/95/2       1      NA      14        5       9
    ##  8 #4/2/95/3-3       1       4      13        6       8
    ##  9 #4/2/95/3-3       1       4      13        7       9
    ## 10 #2/2/95/3-2       1       4      NA        8      10
    ## # ℹ 145 more rows

``` r
dplyr::filter(pups_df, sex == 2, pd_walk < 11)
```

    ## # A tibble: 0 × 6
    ## # ℹ 6 variables: litter_number <chr>, sex <dbl>, pd_ears <dbl>, pd_eyes <dbl>,
    ## #   pd_pivot <dbl>, pd_walk <dbl>

# Mutate

Sometimes you need to select columns; sometimes you need to change them
or create new ones. You can do this using mutate.

``` r
mutate(litters_df,
  wt_gain = gd18_weight - gd0_weight,
  group = str_to_lower(group)
)
```

    ## # A tibble: 49 × 9
    ##    group litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>                <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 con7  #85                   19.7        34.7          20               3
    ##  2 con7  #1/2/95/2             27          42            19               8
    ##  3 con7  #5/5/3/83/3-3         26          41.4          19               6
    ##  4 con7  #5/4/2/95/2           28.5        44.1          19               5
    ##  5 con7  #4/2/95/3-3           NA          NA            20               6
    ##  6 con7  #2/2/95/3-2           NA          NA            20               6
    ##  7 con7  #1/5/3/83/3-3/2       NA          NA            20               9
    ##  8 con8  #3/83/3-3             NA          NA            20               9
    ##  9 con8  #2/95/3               NA          NA            20               8
    ## 10 con8  #3/5/2/2/95           28.5        NA            20               8
    ## # ℹ 39 more rows
    ## # ℹ 3 more variables: pups_dead_birth <dbl>, pups_survive <dbl>, wt_gain <dbl>

## Learning Assessment

``` r
mutate(pups_df,
  pd_pivote_minus_7 = pd_pivot - 7,
  pd_sum = pd_ears + pd_eyes + pd_pivot + pd_walk
)
```

    ## # A tibble: 155 × 8
    ##    litter_number   sex pd_ears pd_eyes pd_pivot pd_walk pd_pivote_minus_7 pd_sum
    ##    <chr>         <dbl>   <dbl>   <dbl>    <dbl>   <dbl>             <dbl>  <dbl>
    ##  1 #85               1       4      13        7      11                 0     35
    ##  2 #85               1       4      13        7      12                 0     36
    ##  3 #1/2/95/2         1       5      13        7       9                 0     34
    ##  4 #1/2/95/2         1       5      13        8      10                 1     36
    ##  5 #5/5/3/83/3-3     1       5      13        8      10                 1     36
    ##  6 #5/5/3/83/3-3     1       5      14        6       9                -1     34
    ##  7 #5/4/2/95/2       1      NA      14        5       9                -2     NA
    ##  8 #4/2/95/3-3       1       4      13        6       8                -1     31
    ##  9 #4/2/95/3-3       1       4      13        7       9                 0     33
    ## 10 #2/2/95/3-2       1       4      NA        8      10                 1     NA
    ## # ℹ 145 more rows

# Arrange

‘arrange(litters_df, group, pups_born_alive)’ means sort litters_df
first by group, and then within each group, sort by pups_born_alive.

``` r
head(arrange(litters_df, group, pups_born_alive), 10)
```

    ## # A tibble: 10 × 8
    ##    group litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>                <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Con7  #85                   19.7        34.7          20               3
    ##  2 Con7  #5/4/2/95/2           28.5        44.1          19               5
    ##  3 Con7  #5/5/3/83/3-3         26          41.4          19               6
    ##  4 Con7  #4/2/95/3-3           NA          NA            20               6
    ##  5 Con7  #2/2/95/3-2           NA          NA            20               6
    ##  6 Con7  #1/2/95/2             27          42            19               8
    ##  7 Con7  #1/5/3/83/3-3/2       NA          NA            20               9
    ##  8 Con8  #2/2/95/2             NA          NA            19               5
    ##  9 Con8  #1/6/2/2/95-2         NA          NA            20               7
    ## 10 Con8  #3/6/2/2/95-3         NA          NA            20               7
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

# \|\>

The following is an example of the first option:

``` r
litters_df_raw = 
    read_csv("./data_import_examples/FAS_litters.csv", na = c("NA", ".", ""))
```

    ## Rows: 49 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (2): Group, Litter Number
    ## dbl (6): GD0 weight, GD18 weight, GD of Birth, Pups born alive, Pups dead @ ...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
litters_df_clean_names = janitor::clean_names(litters_df_raw)
litters_df_selected_cols = select(litters_df_clean_names, -pups_survive)
litters_df_with_vars = 
  mutate(
    litters_df_selected_cols, 
    wt_gain = gd18_weight - gd0_weight,
    group = str_to_lower(group))
litters_df_with_vars_without_missing = 
  drop_na(litters_df_with_vars, wt_gain)
litters_df_with_vars_without_missing
```

    ## # A tibble: 31 × 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 con7  #85                 19.7        34.7          20               3
    ##  2 con7  #1/2/95/2           27          42            19               8
    ##  3 con7  #5/5/3/83/3-3       26          41.4          19               6
    ##  4 con7  #5/4/2/95/2         28.5        44.1          19               5
    ##  5 mod7  #59                 17          33.4          19               8
    ##  6 mod7  #103                21.4        42.1          19               9
    ##  7 mod7  #3/82/3-2           28          45.9          20               5
    ##  8 mod7  #5/3/83/5-2         22.6        37            19               5
    ##  9 mod7  #106                21.7        37.8          20               5
    ## 10 mod7  #94/2               24.4        42.9          19               7
    ## # ℹ 21 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, wt_gain <dbl>

Below, we try the second option:

``` r
litters_df_clean = 
  drop_na(
    mutate(
      select(
        janitor::clean_names(
          read_csv("./data_import_examples/FAS_litters.csv", na = c("NA", ".", ""))
          ), 
      -pups_survive
      ),
    wt_gain = gd18_weight - gd0_weight,
    group = str_to_lower(group)
    ),
  wt_gain
  )
```

    ## Rows: 49 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (2): Group, Litter Number
    ## dbl (6): GD0 weight, GD18 weight, GD of Birth, Pups born alive, Pups dead @ ...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
litters_df_clean
```

    ## # A tibble: 31 × 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 con7  #85                 19.7        34.7          20               3
    ##  2 con7  #1/2/95/2           27          42            19               8
    ##  3 con7  #5/5/3/83/3-3       26          41.4          19               6
    ##  4 con7  #5/4/2/95/2         28.5        44.1          19               5
    ##  5 mod7  #59                 17          33.4          19               8
    ##  6 mod7  #103                21.4        42.1          19               9
    ##  7 mod7  #3/82/3-2           28          45.9          20               5
    ##  8 mod7  #5/3/83/5-2         22.6        37            19               5
    ##  9 mod7  #106                21.7        37.8          20               5
    ## 10 mod7  #94/2               24.4        42.9          19               7
    ## # ℹ 21 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, wt_gain <dbl>

These are both *confusing and bad*: the first gets confusing and
clutters our workspace, and the second has to be read inside out.

``` r
litters_df = 
  read_csv("./data_import_examples/FAS_litters.csv", na = c("NA", ".", "")) |> 
  janitor::clean_names() |> 
  select(-pups_survive) |> 
  mutate(
    wt_gain = gd18_weight - gd0_weight,
    group = str_to_lower(group)) |> 
  drop_na(wt_gain)
```

    ## Rows: 49 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (2): Group, Litter Number
    ## dbl (6): GD0 weight, GD18 weight, GD of Birth, Pups born alive, Pups dead @ ...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
litters_df
```

    ## # A tibble: 31 × 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 con7  #85                 19.7        34.7          20               3
    ##  2 con7  #1/2/95/2           27          42            19               8
    ##  3 con7  #5/5/3/83/3-3       26          41.4          19               6
    ##  4 con7  #5/4/2/95/2         28.5        44.1          19               5
    ##  5 mod7  #59                 17          33.4          19               8
    ##  6 mod7  #103                21.4        42.1          19               9
    ##  7 mod7  #3/82/3-2           28          45.9          20               5
    ##  8 mod7  #5/3/83/5-2         22.6        37            19               5
    ##  9 mod7  #106                21.7        37.8          20               5
    ## 10 mod7  #94/2               24.4        42.9          19               7
    ## # ℹ 21 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, wt_gain <dbl>

In the majority of cases (and everywhere in the tidyverse) you can trust
that the first argument is the right one and be happy with life, but
there are some cases where what you’re piping isn’t going into the first
argument. Here, using the placeholder \_ is necessary to indicate where
the object being piped should go. For example, to regress wt_gain on
pups_born_alive, you might use:

``` r
litters_df |>
  lm(wt_gain ~ pups_born_alive, data = _) |>
  broom::tidy()
```

    ## # A tibble: 2 × 5
    ##   term            estimate std.error statistic  p.value
    ##   <chr>              <dbl>     <dbl>     <dbl>    <dbl>
    ## 1 (Intercept)       13.1       1.27      10.3  3.39e-11
    ## 2 pups_born_alive    0.605     0.173      3.49 1.55e- 3

For here \|\> always put pipe thing in first argument. eg: ‘litters_df
\|\> head()’ equal to ‘head(litters_df)’

but for lm(), the first argument isn’t data, but the formula. Thus, if
we use \|\> here, it will become thing like: ‘lm(litters_df, wt_gain ~
pups_born_alive)’

The thing we want is

``` r
lm(
  wt_gain ~ pups_born_alive,
  data = litters_df
)
```

    ## 
    ## Call:
    ## lm(formula = wt_gain ~ pups_born_alive, data = litters_df)
    ## 
    ## Coefficients:
    ##     (Intercept)  pups_born_alive  
    ##         13.0833           0.6051

Then use placeholder, it will become

``` r
litters_df |>
  lm(wt_gain ~ pups_born_alive, data = _)
```

    ## 
    ## Call:
    ## lm(formula = wt_gain ~ pups_born_alive, data = litters_df)
    ## 
    ## Coefficients:
    ##     (Intercept)  pups_born_alive  
    ##         13.0833           0.6051

## Learning Assesment

``` r
pups_df = 
  read_csv("./data_import_examples/FAS_pups.csv",
           skip = 3,
           na = c("NA"," ","."))|>
  janitor::clean_names()|>
  dplyr::filter(sex == 1) |>
  select(-pd_ears)|>
  mutate(pd_pivot_gt7 = pd_pivot>=7)
```

    ## Rows: 313 Columns: 6
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): Litter Number
    ## dbl (5): Sex, PD ears, PD eyes, PD pivot, PD walk
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
pups_df
```

    ## # A tibble: 155 × 6
    ##    litter_number   sex pd_eyes pd_pivot pd_walk pd_pivot_gt7
    ##    <chr>         <dbl>   <dbl>    <dbl>   <dbl> <lgl>       
    ##  1 #85               1      13        7      11 TRUE        
    ##  2 #85               1      13        7      12 TRUE        
    ##  3 #1/2/95/2         1      13        7       9 TRUE        
    ##  4 #1/2/95/2         1      13        8      10 TRUE        
    ##  5 #5/5/3/83/3-3     1      13        8      10 TRUE        
    ##  6 #5/5/3/83/3-3     1      14        6       9 FALSE       
    ##  7 #5/4/2/95/2       1      14        5       9 FALSE       
    ##  8 #4/2/95/3-3       1      13        6       8 FALSE       
    ##  9 #4/2/95/3-3       1      13        7       9 TRUE        
    ## 10 #2/2/95/3-2       1      NA        8      10 TRUE        
    ## # ℹ 145 more rows
