Add mock data
================

Setup the environment
---------------------

### KYC Analysis proper

Load data
---------

First, we need to locate the data provided for analysis.

``` r
input_loc <- "./inputs/"
credit_file <- "credit_test.csv"
```

Now, let's load and look at the credit list:

``` r
credit <- read.csv(paste0(input_loc, credit_file))
str(credit)
```

    ## 'data.frame':    10000 obs. of  23 variables:
    ##  $ Loan.ID                     : Factor w/ 10000 levels "0004f37b-5859-40f6-98d0-367aa3b3f3f1",..: 9681 4326 9690 5138 334 6396 2659 5710 439 8147 ...
    ##  $ Customer.ID                 : Factor w/ 10000 levels "00010876-7972-40c5-8238-e1f187ca1396",..: 8715 887 1726 738 2222 4008 2774 1019 4075 2490 ...
    ##  $ Current.Loan.Amount         : int  611314 266662 153494 176242 321992 202928 621786 266794 202466 266288 ...
    ##  $ Term                        : Factor w/ 2 levels "Long Term","Short Term": 2 2 2 2 2 2 1 1 2 1 ...
    ##  $ Credit.Score                : int  747 734 709 727 744 741 733 NA 736 683 ...
    ##  $ Annual.Income               : int  2074116 1919190 871112 780083 1761148 760380 1783606 NA 1068617 2031518 ...
    ##  $ Years.in.current.job        : Factor w/ 12 levels "< 1 year","1 year",..: 3 3 4 3 3 2 3 1 7 4 ...
    ##  $ Home.Ownership              : Factor w/ 4 levels "HaveMortgage",..: 2 2 4 4 2 4 2 3 4 4 ...
    ##  $ Purpose                     : Factor w/ 16 levels "Business Loan",..: 4 4 4 4 4 4 4 4 4 4 ...
    ##  $ Monthly.Debt                : num  42001 36624 8392 16772 39479 ...
    ##  $ Years.of.Credit.History     : num  21.8 19.4 12.5 16.5 26 13.8 15.3 5.8 20.5 24.4 ...
    ##  $ Months.since.last.delinquent: int  NA NA 10 27 44 NA NA NA NA 56 ...
    ##  $ Number.of.Open.Accounts     : int  9 11 10 16 14 6 42 9 2 8 ...
    ##  $ Number.of.Credit.Problems   : int  0 0 0 1 0 0 0 0 0 2 ...
    ##  $ Current.Credit.Balance      : int  621908 679573 38532 156940 359765 258647 281599 233206 0 31445 ...
    ##  $ Maximum.Open.Credit         : int  1058970 904442 388036 531322 468072 476872 1449162 342232 0 251130 ...
    ##  $ Bankruptcies                : int  0 0 0 1 0 0 0 0 0 2 ...
    ##  $ Tax.Liens                   : int  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ Customer.Name               : logi  NA NA NA NA NA NA ...
    ##  $ Date.of.Birth               : logi  NA NA NA NA NA NA ...
    ##  $ Nationality                 : logi  NA NA NA NA NA NA ...
    ##  $ ID                          : logi  NA NA NA NA NA NA ...
    ##  $ Address                     : logi  NA NA NA NA NA NA ...

Prepare test data
-----------------

Let's extract the relevant columns, and see what the initial structure, values, and summary looks like. In this case, the pertinent columns are "Customer.Name" and "Date.of.Birth"

``` r
credit <- credit[,c("Customer.Name","Date.of.Birth")]
head(credit, 10)
```

    ##    Customer.Name Date.of.Birth
    ## 1             NA            NA
    ## 2             NA            NA
    ## 3             NA            NA
    ## 4             NA            NA
    ## 5             NA            NA
    ## 6             NA            NA
    ## 7             NA            NA
    ## 8             NA            NA
    ## 9             NA            NA
    ## 10            NA            NA

``` r
summary(credit)
```

    ##  Customer.Name  Date.of.Birth 
    ##  Mode:logical   Mode:logical  
    ##  NA's:10000     NA's:10000

``` r
str(credit)
```

    ## 'data.frame':    10000 obs. of  2 variables:
    ##  $ Customer.Name: logi  NA NA NA NA NA NA ...
    ##  $ Date.of.Birth: logi  NA NA NA NA NA NA ...

Now let's load the mock data

``` r
file_loc <- "inputs/sample data raw/"
mock_data <- read_csv(paste0(file_loc,"MOCK_DATA_10k_r.csv"))
```

    ## Parsed with column specification:
    ## cols(
    ##   first_name = col_character(),
    ##   last_name = col_character(),
    ##   email = col_character(),
    ##   gender = col_character(),
    ##   birth_date = col_character()
    ## )

``` r
summary(mock_data)
```

    ##   first_name         last_name            email          
    ##  Length:10000       Length:10000       Length:10000      
    ##  Class :character   Class :character   Class :character  
    ##  Mode  :character   Mode  :character   Mode  :character  
    ##     gender           birth_date       
    ##  Length:10000       Length:10000      
    ##  Class :character   Class :character  
    ##  Mode  :character   Mode  :character

Now let's combine the name fields into a full name field

``` r
mock_data$fullname <- paste0(mock_data$last_name,", ",mock_data$first_name)
str(mock_data$fullname)
```

    ##  chr [1:10000] "Tomkinson, Christan" "Boddam, Martie" "Koch, Sidney" ...

Let's put the mock data into our credit file

``` r
credit$Customer.Name <- mock_data$fullname
credit$Date.of.Birth <- mock_data$birth_date
head(credit)
```

    ##         Customer.Name Date.of.Birth
    ## 1 Tomkinson, Christan     2/17/2018
    ## 2      Boddam, Martie      7/9/2018
    ## 3        Koch, Sidney     3/16/2018
    ## 4   Tremble, Arabelle     9/14/2018
    ## 5            Sam, Ave     1/28/2018
    ## 6   Dunsford, Victoir     11/6/2018

``` r
summary(credit)
```

    ##  Customer.Name      Date.of.Birth     
    ##  Length:10000       Length:10000      
    ##  Class :character   Class :character  
    ##  Mode  :character   Mode  :character

``` r
str(credit)
```

    ## 'data.frame':    10000 obs. of  2 variables:
    ##  $ Customer.Name: chr  "Tomkinson, Christan" "Boddam, Martie" "Koch, Sidney" "Tremble, Arabelle" ...
    ##  $ Date.of.Birth: chr  "2/17/2018" "7/9/2018" "3/16/2018" "9/14/2018" ...
