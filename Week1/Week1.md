Introduction to R
================

# **Outline** 

1.Writing and Running Code  
2.Import and save dateset  
3.basic functions of R  
4.basic manipulations of data in R  

## 1.writing and Running code

### 1.1 Writing code

Always write code in script or markdown so that your work can be
reproduced. You may want to click the button to write code in an r file
to have a larger space for writing code. ![alt text
here](C:/Users/hansh/OneDrive%20-%20University%20of%20Maryland/611/R/figures/week1/figure1.png)
\#\#\# 1.2 Running code The code you run in the top will display below
in the console You run the code by highlighting it and clicking the run
button The shortcut on mac is Cmd+Return & the shortcun on Window is
crtl+alt+enter ![alt text
here](C:/Users/hansh/OneDrive%20-%20University%20of%20Maryland/611/R/figures/week1/figure2.png)
\#\#\# 1.3 \#You can write comments by using the hashtag \#Commented
code will not be written print(‘The commented code will display in the
console but will not be read as code’)

## 2Import and save dataset

### 2.1 Import CSV

CSV stands for comma separated variable and its one of the most common
ways we’ll be working with data throughout this course. The basic format
of a csv file is the first line indicating the column names and the rest
of the rows/lines being data points separated by commas. One of the most
basic ways to read in csv files in R is to use read.csv() which is
built-in to R.

When using read.csv() you’ll need to either pass in the entire path of
the file or have the file be in the same directory as your R script.
Make sure to account for possible spaces in the file path name, you may
need to use backslashes to account for this. This is often a point of
confusion for people new to programming, so make sure you understand the
above before continuing!

``` r
    #record the time
    ptm1 <- proc.time()
    # Pass in the entire file path if not in same directory
    ex <- read.csv('sales.csv')
    ## Stop the clock
    proc.time() - ptm1
```

    ##    user  system elapsed 
    ##       0       0       0

``` r
    # Check structure
    str(ex)
```

    ## 'data.frame':    390 obs. of  5 variables:
    ##  $ ï..Postcode   : int  2121 2092 2128 2073 2134 2162 2093 2042 2198 2043 ...
    ##  $ Sales_Rep_ID  : int  456 789 456 123 789 123 456 789 123 789 ...
    ##  $ Sales_Rep_Name: chr  "Jane" "Ashish" "Jane" "John" ...
    ##  $ Year          : int  2011 2012 2013 2011 2012 2013 2011 2012 2013 2011 ...
    ##  $ Value         : chr  "$84,219" "$28,322" "$81,879" "$44,491" ...

``` r
    # Check the class 
    class(ex)
```

    ## [1] "data.frame"

``` r
    # Check column names
    colnames(ex)
```

    ## [1] "ï..Postcode"    "Sales_Rep_ID"   "Sales_Rep_Name" "Year"          
    ## [5] "Value"

``` r
    # Check the observations
    head(ex)
```

    ##   ï..Postcode Sales_Rep_ID Sales_Rep_Name Year   Value
    ## 1        2121          456           Jane 2011 $84,219
    ## 2        2092          789         Ashish 2012 $28,322
    ## 3        2128          456           Jane 2013 $81,879
    ## 4        2073          123           John 2011 $44,491
    ## 5        2134          789         Ashish 2012 $71,838
    ## 6        2162          123           John 2013 $64,532

### 2.2 Export CSV

``` r
    #Export the sample file as csv
   write.csv(ex, file = "example.csv")
```

## 3Basic functions

``` r
    #Sum of one variable
    #first need to convert the string variable to numeric 
    ex$Value<-as.numeric(gsub('[$,]', '',ex$Value))
    sum(ex$Value)    
```

    ## [1] 19199465

``` r
    #Standard deviation of one variable
    sd(ex$Value)
```

    ## [1] 28251.27

``` r
    #Summary statistics of the dataset
    summary(ex)
```

    ##   ï..Postcode    Sales_Rep_ID Sales_Rep_Name          Year          Value      
    ##  Min.   :2000   Min.   :123   Length:390         Min.   :2011   Min.   :  106  
    ##  1st Qu.:2044   1st Qu.:123   Class :character   1st Qu.:2011   1st Qu.:26102  
    ##  Median :2098   Median :456   Mode  :character   Median :2012   Median :47448  
    ##  Mean   :2098   Mean   :456                      Mean   :2012   Mean   :49229  
    ##  3rd Qu.:2142   3rd Qu.:789                      3rd Qu.:2013   3rd Qu.:72278  
    ##  Max.   :2206   Max.   :789                      Max.   :2013   Max.   :99878

## 4 Basic manipulation

``` r
    options(repos = list(CRAN="http://cran.rstudio.com/"))
    #install package
    install.packages("dplyr")
```

    ## package 'dplyr' successfully unpacked and MD5 sums checked

    ## Warning: cannot remove prior installation of package 'dplyr'

    ## Warning in file.copy(savedcopy, lib, recursive = TRUE):
    ## problem copying C:\Users\hansh\OneDrive\Documents\R\win-
    ## library\4.1\00LOCK\dplyr\libs\x64\dplyr.dll to C:
    ## \Users\hansh\OneDrive\Documents\R\win-library\4.1\dplyr\libs\x64\dplyr.dll:
    ## Permission denied

    ## Warning: restored 'dplyr'

    ## 
    ## The downloaded binary packages are in
    ##  C:\Users\hansh\AppData\Local\Temp\RtmpekZII1\downloaded_packages

``` r
    #import the package
    library(dplyr)
```

### 4.1 Sort

``` r
    #Sort the dataset by Sale Rep & year in rising order
    ex2<-ex%>%
         arrange(Sales_Rep_Name,Year)
    head(ex2)
```

    ##   ï..Postcode Sales_Rep_ID Sales_Rep_Name Year Value
    ## 1        2043          789         Ashish 2011 49546
    ## 2        2032          789         Ashish 2011 98780
    ## 3        2021          789         Ashish 2011 24550
    ## 4        2142          789         Ashish 2011  2356
    ## 5        2049          789         Ashish 2011 87702
    ## 6        2087          789         Ashish 2011   106

### 4.2 Calculate the group average

``` r
 #calculate the yearly average and sum sale for each employee
    summary<-ex2%>%
          group_by(Sales_Rep_Name,Year)%>%
          summarise_at(vars(Value), list(Avg_value=mean,Tot_value=sum))

    print(summary)
```

    ## # A tibble: 9 x 4
    ## # Groups:   Sales_Rep_Name [3]
    ##   Sales_Rep_Name  Year Avg_value Tot_value
    ##   <chr>          <int>     <dbl>     <dbl>
    ## 1 Ashish          2011    46829.   2107314
    ## 2 Ashish          2012    50142.   2356655
    ## 3 Ashish          2013    50581.   1922073
    ## 4 Jane            2011    56206.   2135824
    ## 5 Jane            2012    47334.   2082684
    ## 6 Jane            2013    50974.   2446757
    ## 7 John            2011    49644.   2333253
    ## 8 John            2012    43913.   1712600
    ## 9 John            2013    47780.   2102305

``` r
  #calculate the representative sale average
    summary2<-ex2%>%
          group_by(Sales_Rep_Name)%>%
          summarise_at(vars(Value),list(Rep_avg=mean))
    
    print(summary2)
```

    ## # A tibble: 3 x 2
    ##   Sales_Rep_Name Rep_avg
    ##   <chr>            <dbl>
    ## 1 Ashish          49123.
    ## 2 Jane            51271.
    ## 3 John            47294.

``` r
    #calcualte the yearly sale average
    summary3<-ex2%>%
          group_by(Year)%>%
          summarise_at(vars(Value),list(Year_avg=mean))
    
    print(summary3)
```

    ## # A tibble: 3 x 2
    ##    Year Year_avg
    ##   <int>    <dbl>
    ## 1  2011   50588.
    ## 2  2012   47323.
    ## 3  2013   49778.

### 4.3 Merge dataset

``` r
   data<-merge(summary,summary2,by="Sales_Rep_Name")
   data2<-merge(data,summary3,by="Year")
```

### 4.4 Create new variables

``` r
   data3<-data2%>%
          mutate(higher_hor=(Avg_value>Year_avg), #if the repersentative yearly average sale is higher than the yearly average, it is coded as one
                 higher_ver=(Avg_value>Rep_avg) #if the repersentative yearly average sale is higher than the his or her own average, it is coded as one
               )

  #print the newly created dataset3
  print(data3)
```

    ##   Year Sales_Rep_Name Avg_value Tot_value  Rep_avg Year_avg higher_hor
    ## 1 2011         Ashish  46829.20   2107314 49123.40 50587.62      FALSE
    ## 2 2011           John  49643.68   2333253 47293.52 50587.62      FALSE
    ## 3 2011           Jane  56205.89   2135824 51271.27 50587.62       TRUE
    ## 4 2012         Ashish  50141.60   2356655 49123.40 47322.61       TRUE
    ## 5 2012           Jane  47333.73   2082684 51271.27 47322.61       TRUE
    ## 6 2012           John  43912.82   1712600 47293.52 47322.61      FALSE
    ## 7 2013           Jane  50974.10   2446757 51271.27 49777.96       TRUE
    ## 8 2013         Ashish  50580.87   1922073 49123.40 49777.96       TRUE
    ## 9 2013           John  47779.66   2102305 47293.52 49777.96      FALSE
    ##   higher_ver
    ## 1      FALSE
    ## 2       TRUE
    ## 3       TRUE
    ## 4       TRUE
    ## 5      FALSE
    ## 6      FALSE
    ## 7      FALSE
    ## 8       TRUE
    ## 9       TRUE

More information can be found at [linked
phrase](https://dplyr.tidyverse.org/)





        
        
        
        
        
      
