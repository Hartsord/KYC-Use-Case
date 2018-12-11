KYC Lookup
================

KYC Lookup
----------

This is a report on accessing a file of KYC clients, and comparing them to the OFAC database to check if there are any matches.

Including Code
--------------

This report includes the following packages:

``` r
# include i_don't_know
library(readr)
```

### KYC Analysis proper

Load data
---------

First, we need to locate the data provided for analysis.

``` r
input_loc <- "./inputs/"
screen_file <- "screening_list_consolidated_2018-12-04.csv"
credit_file <- "credit_test.csv"
```

Let's load and look at the screening list:

``` r
screen <- read.csv(paste0(input_loc, screen_file))
str(screen)
```

    ## 'data.frame':    10085 obs. of  28 variables:
    ##  $ source                  : Factor w/ 10 levels "Denied Persons List (DPL) - Bureau of Industry and Security",..: 1 1 9 9 9 9 9 9 9 9 ...
    ##  $ entity_number           : int  NA NA 21944 24904 22084 26095 9935 17291 17292 12562 ...
    ##  $ type                    : Factor w/ 5 levels "","Aircraft",..: 1 1 4 4 4 4 4 4 4 4 ...
    ##  $ programs                : Factor w/ 148 levels "","561List","BALKANS",..: 1 1 123 123 123 102 102 102 102 102 ...
    ##  $ name                    : Factor w/ 10000 levels " ADRIAN MANUEL HERNANDEZ",..: 1 2 3 4 5 6 7 8 9 10 ...
    ##  $ title                   : Factor w/ 572 levels "","Acted in the Capacity of the Head of the Office of the National Treasury",..: 1 1 508 1 508 1 1 1 1 1 ...
    ##  $ addresses               : Factor w/ 4952 levels ""," Xunyang Chem. Bldg., Materials Factory, Xunyang District, Jiujiang City, China, CN",..: 541 4778 4574 1 4574 4574 1 1 4574 1 ...
    ##  $ federal_register_notice : Factor w/ 480 levels "","46 F.R. 19290 3/30/81",..: 385 79 1 1 1 1 1 1 1 1 ...
    ##  $ start_date              : Factor w/ 247 levels "","0097-06-30",..: 223 41 1 1 1 1 1 1 1 1 ...
    ##  $ end_date                : Factor w/ 178 levels "","2018-12-12",..: 23 178 1 1 1 1 1 1 1 1 ...
    ##  $ standard_order          : Factor w/ 4 levels ""," ","N","Y": 4 4 1 1 1 1 1 1 1 1 ...
    ##  $ license_requirement     : Factor w/ 22 levels "","For all items subject to the EAR",..: 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ license_policy          : Factor w/ 15 levels "","Case-by-case basis",..: 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ call_sign               : Factor w/ 63 levels "","5IM 592","5IM 593",..: 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ vessel_type             : Factor w/ 19 levels "","Bulk Carrier",..: 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ gross_tonnage           : int  NA NA NA NA NA NA NA NA NA NA ...
    ##  $ gross_registered_tonnage: int  NA NA NA NA NA NA NA NA NA NA ...
    ##  $ vessel_flag             : Factor w/ 20 levels "","Cambodia",..: 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ vessel_owner            : Factor w/ 5 levels "","Compania Navegacion Golfo S.A.",..: 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ remarks                 : Factor w/ 878 levels "","(Afri-Belg Supermercados, Cash & Carry Retail Stores, Afri-belg Construction and Afri-Belg Agriculture are subs"| __truncated__,..: 734 682 1 1 1 1 1 1 1 811 ...
    ##  $ source_list_url         : Factor w/ 7 levels "http://bit.ly/1I7ipyR",..: 5 5 1 1 1 1 1 1 1 1 ...
    ##  $ alt_names               : Factor w/ 4479 levels "","'ABBAS, Yasir 'Aziz",..: 1 1 1 2 1 53 56 6 7 8 ...
    ##  $ citizenships            : Factor w/ 106 levels "","AE","AF","AF; DE",..: 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ dates_of_birth          : Factor w/ 3016 levels "","1916-09-20",..: 1 1 2118 2488 1451 1649 1116 2919 2744 2613 ...
    ##  $ nationalities           : Factor w/ 126 levels "","AF","AU","AU; EG",..: 1 1 105 105 105 50 94 54 54 2 ...
    ##  $ places_of_birth         : Factor w/ 1478 levels "","'Adlun, Lebanon",..: 1 1 1 1 1 1295 56 1 593 1 ...
    ##  $ source_information_url  : Factor w/ 10 levels "http://bit.ly/1iwxiF0",..: 1 1 5 5 5 5 5 5 5 5 ...
    ##  $ ids                     : Factor w/ 4291 levels "","00044434, Government Gazette Number; 1027700035769, Registration ID; 7708004767, Tax ID No.; Subject to Directi"| __truncated__,..: 1 1 1 2601 1 1 688 3562 3561 3500 ...

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

Prepare screen data
-------------------

Now let's extract the important columns from the training CSV. For our purposes, these are the "name" column and the "dates\_of\_birth" column

``` r
screen <- screen[,c("name","dates_of_birth")]
head(screen, 10)
```

    ##                                                     name
    ## 1                                ADRIAN MANUEL HERNANDEZ
    ## 2                                                 I. ASH
    ## 3                                            'ABBAS, Rim
    ## 4                                          'ABBAS, Yasir
    ## 5                                         'ABBUD, Hikmat
    ## 6                                   'ABD AL-NASIR, Hajji
    ## 7  'ABD AL-RAZZIQ, Abu Sufian al-Salamabi Muhammed Ahmed
    ## 8    'ABD AL-SALAM, 'Abd al-Malik Muhammad Yusuf 'Uthman
    ## 9           'ABD AL-SALAM, Ashraf Muhammad Yusuf 'Uthman
    ## 10                               'ABD AL-SALAM, Said Jan
    ##            dates_of_birth
    ## 1                        
    ## 2                        
    ## 3              1973-03-25
    ## 4              1978-08-22
    ## 5              1966-01-01
    ## 6        1967; 1966; 1968
    ## 7              1962-08-06
    ## 8              1989-07-13
    ## 9              1984-01-01
    ## 10 1981-02-05; 1972-01-01

``` r
summary(screen)
```

    ##                       name       dates_of_birth
    ##  Gunther Migeotte       :    5          :6372  
    ##  AL-AQSA FOUNDATION     :    4   1964   :  22  
    ##  AL NASER WINGS AIRLINES:    3   1953   :  19  
    ##  ATIA, Hachim K.        :    3   1956   :  19  
    ##  'ALI, Muhammad         :    2   1958   :  17  
    ##  ABDULAH AL NASSER      :    2   1962   :  17  
    ##  (Other)                :10066   (Other):3619

``` r
str(screen)
```

    ## 'data.frame':    10085 obs. of  2 variables:
    ##  $ name          : Factor w/ 10000 levels " ADRIAN MANUEL HERNANDEZ",..: 1 2 3 4 5 6 7 8 9 10 ...
    ##  $ dates_of_birth: Factor w/ 3016 levels "","1916-09-20",..: 1 1 2118 2488 1451 1649 1116 2919 2744 2613 ...

Looking at the contents of the dates\_of\_birth field, we can see that it is in several date formats, which we will have to decide on later

Prepare test data
-----------------

Similar to the train, data, let's extract the relevant columns, and see what the initial structure, values, and summary looks like. In this case, the pertinent columns are "Customer.Name" and "Date.of.Birth"

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

For the credit test data, we can see that the names and dates of birth have all been removed, so we have to replace these with dummy data.
