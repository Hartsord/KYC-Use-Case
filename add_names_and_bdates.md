Add names and dates
================

Sample data comibination
------------------------

Let's load up the first file

``` r
file_loc <- "inputs/sample data raw/"
MOCK_DATA <- read_csv(paste0(file_loc,"MOCK_DATA.csv"))
```

    ## Parsed with column specification:
    ## cols(
    ##   id = col_double(),
    ##   first_name = col_character(),
    ##   last_name = col_character(),
    ##   email = col_character(),
    ##   gender = col_character(),
    ##   birth_date = col_character()
    ## )

``` r
summary(MOCK_DATA)
```

    ##        id          first_name         last_name            email          
    ##  Min.   :   1.0   Length:1000        Length:1000        Length:1000       
    ##  1st Qu.: 250.8   Class :character   Class :character   Class :character  
    ##  Median : 500.5   Mode  :character   Mode  :character   Mode  :character  
    ##  Mean   : 500.5                                                           
    ##  3rd Qu.: 750.2                                                           
    ##  Max.   :1000.0                                                           
    ##     gender           birth_date       
    ##  Length:1000        Length:1000       
    ##  Class :character   Class :character  
    ##  Mode  :character   Mode  :character  
    ##                                       
    ##                                       
    ## 

Let's continue adding the next file

``` r
MOCK_DATA <- rbind(MOCK_DATA, read_csv(paste0(file_loc,"MOCK_DATA (1).csv")))
```

    ## Parsed with column specification:
    ## cols(
    ##   id = col_double(),
    ##   first_name = col_character(),
    ##   last_name = col_character(),
    ##   email = col_character(),
    ##   gender = col_character(),
    ##   birth_date = col_character()
    ## )

``` r
summary(MOCK_DATA)
```

    ##        id          first_name         last_name            email          
    ##  Min.   :   1.0   Length:2000        Length:2000        Length:2000       
    ##  1st Qu.: 250.8   Class :character   Class :character   Class :character  
    ##  Median : 500.5   Mode  :character   Mode  :character   Mode  :character  
    ##  Mean   : 500.5                                                           
    ##  3rd Qu.: 750.2                                                           
    ##  Max.   :1000.0                                                           
    ##     gender           birth_date       
    ##  Length:2000        Length:2000       
    ##  Class :character   Class :character  
    ##  Mode  :character   Mode  :character  
    ##                                       
    ##                                       
    ## 

Let's add in all the other files

``` r
MOCK_DATA <- rbind(MOCK_DATA, read_csv(paste0(file_loc,"MOCK_DATA (2).csv")))
```

    ## Parsed with column specification:
    ## cols(
    ##   id = col_double(),
    ##   first_name = col_character(),
    ##   last_name = col_character(),
    ##   email = col_character(),
    ##   gender = col_character(),
    ##   birth_date = col_character()
    ## )

``` r
MOCK_DATA <- rbind(MOCK_DATA, read_csv(paste0(file_loc,"MOCK_DATA (3).csv")))
```

    ## Parsed with column specification:
    ## cols(
    ##   id = col_double(),
    ##   first_name = col_character(),
    ##   last_name = col_character(),
    ##   email = col_character(),
    ##   gender = col_character(),
    ##   birth_date = col_character()
    ## )

``` r
MOCK_DATA <- rbind(MOCK_DATA, read_csv(paste0(file_loc,"MOCK_DATA (4).csv")))
```

    ## Parsed with column specification:
    ## cols(
    ##   id = col_double(),
    ##   first_name = col_character(),
    ##   last_name = col_character(),
    ##   email = col_character(),
    ##   gender = col_character(),
    ##   birth_date = col_character()
    ## )

``` r
MOCK_DATA <- rbind(MOCK_DATA, read_csv(paste0(file_loc,"MOCK_DATA (5).csv")))
```

    ## Parsed with column specification:
    ## cols(
    ##   id = col_double(),
    ##   first_name = col_character(),
    ##   last_name = col_character(),
    ##   email = col_character(),
    ##   gender = col_character(),
    ##   birth_date = col_character()
    ## )

``` r
MOCK_DATA <- rbind(MOCK_DATA, read_csv(paste0(file_loc,"MOCK_DATA (6).csv")))
```

    ## Parsed with column specification:
    ## cols(
    ##   id = col_double(),
    ##   first_name = col_character(),
    ##   last_name = col_character(),
    ##   email = col_character(),
    ##   gender = col_character(),
    ##   birth_date = col_character()
    ## )

``` r
MOCK_DATA <- rbind(MOCK_DATA, read_csv(paste0(file_loc,"MOCK_DATA (7).csv")))
```

    ## Parsed with column specification:
    ## cols(
    ##   id = col_double(),
    ##   first_name = col_character(),
    ##   last_name = col_character(),
    ##   email = col_character(),
    ##   gender = col_character(),
    ##   birth_date = col_character()
    ## )

``` r
MOCK_DATA <- rbind(MOCK_DATA, read_csv(paste0(file_loc,"MOCK_DATA (8).csv")))
```

    ## Parsed with column specification:
    ## cols(
    ##   id = col_double(),
    ##   first_name = col_character(),
    ##   last_name = col_character(),
    ##   email = col_character(),
    ##   gender = col_character(),
    ##   birth_date = col_character()
    ## )

``` r
MOCK_DATA <- rbind(MOCK_DATA, read_csv(paste0(file_loc,"MOCK_DATA (9).csv")))
```

    ## Parsed with column specification:
    ## cols(
    ##   id = col_double(),
    ##   first_name = col_character(),
    ##   last_name = col_character(),
    ##   email = col_character(),
    ##   gender = col_character(),
    ##   birth_date = col_character()
    ## )

``` r
summary(MOCK_DATA)
```

    ##        id          first_name         last_name            email          
    ##  Min.   :   1.0   Length:10000       Length:10000       Length:10000      
    ##  1st Qu.: 250.8   Class :character   Class :character   Class :character  
    ##  Median : 500.5   Mode  :character   Mode  :character   Mode  :character  
    ##  Mean   : 500.5                                                           
    ##  3rd Qu.: 750.2                                                           
    ##  Max.   :1000.0                                                           
    ##     gender           birth_date       
    ##  Length:10000       Length:10000      
    ##  Class :character   Class :character  
    ##  Mode  :character   Mode  :character  
    ##                                       
    ##                                       
    ## 

``` r
head(MOCK_DATA)
```

    ## # A tibble: 6 x 6
    ##      id first_name last_name email                       gender birth_date
    ##   <dbl> <chr>      <chr>     <chr>                       <chr>  <chr>     
    ## 1     1 Christan   Tomkinson ctomkinson0@hugedomains.com Female 2/17/2018 
    ## 2     2 Martie     Boddam    mboddam1@vistaprint.com     Female 7/9/2018  
    ## 3     3 Sidney     Koch      skoch2@imgur.com            Male   3/16/2018 
    ## 4     4 Arabelle   Tremble   atremble3@topsy.com         Female 9/14/2018 
    ## 5     5 Ave        Sam       asam4@msu.edu               Male   1/28/2018 
    ## 6     6 Victoir    Dunsford  vdunsford5@icq.com          Male   11/6/2018

We don't need the id column, so let's drop it

``` r
MOCK_DATA$id <- NULL
head(MOCK_DATA)
```

    ## # A tibble: 6 x 5
    ##   first_name last_name email                       gender birth_date
    ##   <chr>      <chr>     <chr>                       <chr>  <chr>     
    ## 1 Christan   Tomkinson ctomkinson0@hugedomains.com Female 2/17/2018 
    ## 2 Martie     Boddam    mboddam1@vistaprint.com     Female 7/9/2018  
    ## 3 Sidney     Koch      skoch2@imgur.com            Male   3/16/2018 
    ## 4 Arabelle   Tremble   atremble3@topsy.com         Female 9/14/2018 
    ## 5 Ave        Sam       asam4@msu.edu               Male   1/28/2018 
    ## 6 Victoir    Dunsford  vdunsford5@icq.com          Male   11/6/2018

Let's write the new data frame to a file

``` r
write_csv(MOCK_DATA,paste0(file_loc,"MOCK_DATA_10k_r.csv"))
```
