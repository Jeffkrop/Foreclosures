Foreclosures\_plots
================
Jeff Kropelnicki
3/28/2017

``` r
library(dplyr)
library(tidyr)
library(ggplot2)
library(lubridate)
library(readr)
library(VIM)
library(lattice)
```

Importing the data.

``` r
foreclosures <- read_csv("//Users/jeffkropelnicki/Desktop/GIS5564_Urban_GIS/Project/foreclosures..csv") 
foreclosures$FULLADDRESS <- tolower(foreclosures$FULLADDRESS) #Makes FULLADDRESS to lowercase for a join with parcels 
foreclosures$SHERIFFSAL <- mdy_hms(foreclosures$SHERIFFSAL) #Sets date to hours minutes sec.
foreclosures_work <- foreclosures %>% separate(col = SHERIFFSAL,into = c("sheriffsal_year", "sheriffsal_month", "sheriffsal_day"), sep = "-") #separate SALE_DATE to year, month and day.
```

dplyr setting up code for plots day, month and year.

``` r
#Count the number of foreclosures for each day of the month. 
days <- foreclosures_work %>% group_by(sheriffsal_day) %>% summarise(what_day = n()) %>% arrange(sheriffsal_day)

#Count the number of foreclosures for each month of the year. 
month <- foreclosures_work %>% group_by(sheriffsal_month) %>% summarise(what_month = n()) %>% arrange(sheriffsal_month) 

#Count the number of foreclosures for each year.
year <- foreclosures_work %>% group_by(FCLS_YR) %>% summarise(what_year = n()) %>% arrange(FCLS_YR)
```

Plots for day, month and year.

``` r
#Months Plot the number of foreclosures for each month of the year. 
plot(year, type="o", xlab = "Year", ylab = "Number of Foreclosures", main = "Total Foreclosures Over 2005 to 2016")
```

![](Foreclosure_plots_files/figure-markdown_github/unnamed-chunk-3-1.png)

``` r
#Years Plot the number of foreclosures for each year. 
plot(month, type="o", xlab = "month", ylab = "Number of Foreclosures", main = "Total Foreclosures In All Months")
```

![](Foreclosure_plots_files/figure-markdown_github/unnamed-chunk-3-2.png)

``` r
#Days Plot the number of foreclosures for each day of the month. 
plot(days, type="o", xlab = "Day Of The Month", ylab = "Number of Foreclosures", main = "Total On Each Day Of The Month")
```

![](Foreclosure_plots_files/figure-markdown_github/unnamed-chunk-3-3.png)

Investigate data Number Of Foreclosures per Month By Year.

``` r
#Count of the number of foreclosures by month and year. 
number_per_month_and_year <- foreclosures_work %>% group_by(sheriffsal_year, sheriffsal_month) %>% summarize(count = n())

#Boxplot with base R. 
boxplot(count ~ sheriffsal_year, data = number_per_month_and_year, main = "Number Of Foreclosures Per Month By Year", ylab = "Number Of Foreclosures", xlab = "Year", col = c("cyan1"), cex = 1, pch = 21)
```

![](Foreclosure_plots_files/figure-markdown_github/unnamed-chunk-4-1.png)
