---
title: "Reproducible Research: Peer Assessment 1"
author: "TZ"
date: "Thursday, January 18, 2015"
output: 
  html_document:
    keep_md: true
---


## Introduction
This assignment makes use of data from a personal activity monitoring device. This device collects data at 5 minute intervals through out the day. The data consists of two months of data from an anonymous individual collected during the months of October and November, 2012 and include the number of steps taken in 5 minute intervals each day.  
The data for this assignment can be downloaded from the course web
site:

Dataset: [Activity monitoring data](https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2Factivity.zip) [52K]

The variables included in this dataset are:

 **steps**: Number of steps taking in a 5-minute interval (missing
    values are coded as `NA`)

 **date**: The date on which the measurement was taken in YYYY-MM-DD
    format

 **interval**: Identifier for the 5-minute interval in which
    measurement was taken


The dataset is stored in a comma-separated-value (CSV) file and there
are a total of 17,568 observations in this
dataset.


## Loading and preprocessing the data

If csv-file "activity.csv" with needed data set does not exist in working directory following fragment of code will download (if it is necessary) the zip-file into working_directory and unzip its.


```r
if( !file.exists("activity.csv") ) {
        if ( !file.exists("repdata_data_activity.zip") ) {
                ## downloading and unzipping file into 'working_directory/data'
                library(downloader)
                url <- "http://d396qusza40orc.cloudfront.net/repdata%2Fdata%2Factivity.zip"
                download(url, "repdata_data_activity.zip")
                unlink(url)
                rm(url) ## removing spent variable
        }
        unzip("repdata_data_activity.zip", exdir = "./data")
}
```
Then is reading file into data frame named 'activity'

```r
activity <- read.csv("activity.csv",
                     sep = ",",
                     header = TRUE,
                     na.strings="NA", 
                     stringsAsFactors = FALSE)
```

Exploration of data frame:

```r
head(activity)
```

```
##   steps       date interval
## 1    NA 2012-10-01        0
## 2    NA 2012-10-01        5
## 3    NA 2012-10-01       10
## 4    NA 2012-10-01       15
## 5    NA 2012-10-01       20
## 6    NA 2012-10-01       25
```

```r
tail(activity)
```

```
##       steps       date interval
## 17563    NA 2012-11-30     2330
## 17564    NA 2012-11-30     2335
## 17565    NA 2012-11-30     2340
## 17566    NA 2012-11-30     2345
## 17567    NA 2012-11-30     2350
## 17568    NA 2012-11-30     2355
```

```r
str(activity)
```

```
## 'data.frame':	17568 obs. of  3 variables:
##  $ steps   : int  NA NA NA NA NA NA NA NA NA NA ...
##  $ date    : chr  "2012-10-01" "2012-10-01" "2012-10-01" "2012-10-01" ...
##  $ interval: int  0 5 10 15 20 25 30 35 40 45 ...
```

```r
summary(activity)
```

```
##      steps            date              interval     
##  Min.   :  0.00   Length:17568       Min.   :   0.0  
##  1st Qu.:  0.00   Class :character   1st Qu.: 588.8  
##  Median :  0.00   Mode  :character   Median :1177.5  
##  Mean   : 37.38                      Mean   :1177.5  
##  3rd Qu.: 12.00                      3rd Qu.:1766.2  
##  Max.   :806.00                      Max.   :2355.0  
##  NA's   :2304
```

```r
dates <- unique(activity$date); # dates in character format
length(dates);
```

```
## [1] 61
```

```r
intervals <- unique(activity$interval); 
length(intervals); 
```

```
## [1] 288
```

```r
print(intervals)
```

```
##   [1]    0    5   10   15   20   25   30   35   40   45   50   55  100  105
##  [15]  110  115  120  125  130  135  140  145  150  155  200  205  210  215
##  [29]  220  225  230  235  240  245  250  255  300  305  310  315  320  325
##  [43]  330  335  340  345  350  355  400  405  410  415  420  425  430  435
##  [57]  440  445  450  455  500  505  510  515  520  525  530  535  540  545
##  [71]  550  555  600  605  610  615  620  625  630  635  640  645  650  655
##  [85]  700  705  710  715  720  725  730  735  740  745  750  755  800  805
##  [99]  810  815  820  825  830  835  840  845  850  855  900  905  910  915
## [113]  920  925  930  935  940  945  950  955 1000 1005 1010 1015 1020 1025
## [127] 1030 1035 1040 1045 1050 1055 1100 1105 1110 1115 1120 1125 1130 1135
## [141] 1140 1145 1150 1155 1200 1205 1210 1215 1220 1225 1230 1235 1240 1245
## [155] 1250 1255 1300 1305 1310 1315 1320 1325 1330 1335 1340 1345 1350 1355
## [169] 1400 1405 1410 1415 1420 1425 1430 1435 1440 1445 1450 1455 1500 1505
## [183] 1510 1515 1520 1525 1530 1535 1540 1545 1550 1555 1600 1605 1610 1615
## [197] 1620 1625 1630 1635 1640 1645 1650 1655 1700 1705 1710 1715 1720 1725
## [211] 1730 1735 1740 1745 1750 1755 1800 1805 1810 1815 1820 1825 1830 1835
## [225] 1840 1845 1850 1855 1900 1905 1910 1915 1920 1925 1930 1935 1940 1945
## [239] 1950 1955 2000 2005 2010 2015 2020 2025 2030 2035 2040 2045 2050 2055
## [253] 2100 2105 2110 2115 2120 2125 2130 2135 2140 2145 2150 2155 2200 2205
## [267] 2210 2215 2220 2225 2230 2235 2240 2245 2250 2255 2300 2305 2310 2315
## [281] 2320 2325 2330 2335 2340 2345 2350 2355
```
You can see that the **sequence number of 5-minut interval is broken**.
24 hours x (60 min/hours / (5 min/observation)) = 288. **It is ok.**  
24 hours x 60 min/hours = **1440 min**.  
But in data set 'activity' we have intervals **from 0 to 2355 minutes**. That is **not** ok!


```r
nIntervals <- 24*(60/5)
print(nIntervals)
```

```
## [1] 288
```

Now I process and transform the data into a format suitable for following analysis:
I perform column 'date' from 'character' to 'Date' format, add column 'numOfInterval' with sequence number of 5-minut interval, and  correct column 'interval'

```r
activity = transform( activity, date = as.Date(date), 
                     numOfInterval = rep( c(1:nIntervals), length(dates) ), 
                     interval = rep( ((seq(1:nIntervals)*5)-5), length(dates)))
summary(activity)
```

```
##      steps             date               interval      numOfInterval   
##  Min.   :  0.00   Min.   :2012-10-01   Min.   :   0.0   Min.   :  1.00  
##  1st Qu.:  0.00   1st Qu.:2012-10-16   1st Qu.: 358.8   1st Qu.: 72.75  
##  Median :  0.00   Median :2012-10-31   Median : 717.5   Median :144.50  
##  Mean   : 37.38   Mean   :2012-10-31   Mean   : 717.5   Mean   :144.50  
##  3rd Qu.: 12.00   3rd Qu.:2012-11-15   3rd Qu.:1076.2   3rd Qu.:216.25  
##  Max.   :806.00   Max.   :2012-11-30   Max.   :1435.0   Max.   :288.00  
##  NA's   :2304
```

```r
dates <- unique(activity$date); # dates in Date format
intervals <- unique(activity$interval); # true intervals
```


## What is mean total number of steps taken per day?


```r
total_steps_each_day <- tapply(activity$steps, 
                              activity$date, 
                              FUN = sum, 
                              simplify = TRUE)
summary(total_steps_each_day)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
##      41    8841   10760   10770   13290   21190       8
```


```r
par(mfrow = c(1,1), mar=c(6,4,1,1))
plot(dates, total_steps_each_day, 
#      type = "l",
     xlab = "Dates",
     ylab = "The total number of steps per day")
lines(dates, total_steps_each_day)
```

![plot of chunk plot1](figure/plot1-1.png) 


Number of missing values (NAs) in vector 'total_steps_each_day' is equel:

```r
sum(is.na(total_steps_each_day))
```

```
## [1] 8
```

```r
sum(is.na(total_steps_each_day))/length(dates)*100 # in percent
```

```
## [1] 13.11475
```

The total number of steps taken each day is NA in following days:

```r
dates[is.na(total_steps_each_day)]
```

```
## [1] "2012-10-01" "2012-10-08" "2012-11-01" "2012-11-04" "2012-11-09"
## [6] "2012-11-10" "2012-11-14" "2012-11-30"
```
What does it mean?  
I don't see in these sequences of dates any pattern. Maybe some days an anonymous forgot about using of the device for counting taken steps. 


Histogram of the total number of steps taken each day:

```r
par(mfrow = c(2,1), mar=c(4,4,4,1))
hist(total_steps_each_day, 
     breaks = 10,
     col = "cyan",
     main = "Histogram of the total number of steps\n taken each day",
     font.main = 12,
     xlab = "")
text(20000, 8,"breaks = 10")
hist(total_steps_each_day, 
     breaks = 20,
     col = "magenta",
     xlab = "Total number of steps taken each day",
     main = "")
text(20000, 8,"breaks = 20")
```

![plot of chunk plot2](figure/plot2-1.png) 

Mean and median of total number of steps taken per day are

```r
mean(total_steps_each_day, na.rm = TRUE)
```

```
## [1] 10766.19
```

```r
median(total_steps_each_day, na.rm = TRUE)
```

```
## [1] 10765
```


## What is the average daily activity pattern?

```r
avr_steps_each_interval <- tapply(activity$steps, 
                               activity$interval, 
                               FUN = mean, 
                               na.rm = TRUE, 
                               simplify = TRUE)
```


```r
par(mfrow = c(1,1), mar=c(4,4,1,1))
plot(intervals, avr_steps_each_interval, 
     type = "l",
     xlab = "Interval",
     ylab = "Average number of steps taken, averaged across all days")
```

![plot of chunk plot3](figure/plot3-1.png) 

Ordinal number of 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps is equel

```r
ind_avr <- which.max(avr_steps_each_interval)[[1]]
print(ind_avr)
```

```
## [1] 104
```

In is following 5-minute interval:

```r
(ind_avr-1)*5
```

```
## [1] 515
```


What is it in hours?

```r
(ind_avr-1)*5/60
```

```
## [1] 8.583333
```


Maximum of average number of steps in interval is

```r
avr_steps_each_interval[[ind_avr]]
```

```
## [1] 206.1698
```



## Imputing missing values

Only column 'steps' contains missing values (NA).  
I calculate now the total number of missing values

```r
rows_with_NA <- is.na(activity$steps)
nrows_with_NA <- sum(rows_with_NA)
print(nrows_with_NA)
```

```
## [1] 2304
```

```r
nrows_with_NA/nrow(activity)*100 # in percent
```

```
## [1] 13.11475
```
It is not very much but the presence of missing days may introduce bias into some calculations or summaries of the data.  
What is strategy for filling in all of the missing values in the dataset?
"The strategy does not need to be sophisticated".
I will try to use 2 strategies:  
1. NAs replacing by the **median** for that 5-minute interval  
2. NAs replacing by the **mean** for that 5-minute interval  


```r
activity -> activity1 -> activity2
ind <- (activity1$numOfInterval[rows_with_NA])
```

Filling in all of the missing values in the dataset 'activity1' with the **median** for that 5-minute interval

```r
med_steps_each_interval <- tapply(activity$steps, 
                                  activity$interval, 
                                  FUN = median, 
                                  na.rm = TRUE, simplify = TRUE)
```

```r
activity1$steps[rows_with_NA] <- med_steps_each_interval[ind]
```

Filling in all of the missing values in the dataset 'activity2' with the **mean** for that 5-minute interval

```r
activity2$steps[rows_with_NA] <- avr_steps_each_interval[ind]
```


```r
total1_steps_each_day <- tapply(activity1$steps, 
                               activity1$date, 
                               FUN = sum, 
                               na.rm = TRUE, simplify = TRUE)

total2_steps_each_day <- tapply(activity2$steps, 
                                activity2$date, 
                                FUN = sum, 
                                na.rm = TRUE, simplify = TRUE)
```

Histogram of the total number of steps taken each day

```r
par(mfrow = c(2,1), mar=c(4,4,4,1))
hist(total1_steps_each_day, 
     breaks = 10,
     col = "blue",
     ylim = c(0, 25),
     main = "Histogram of the total number of steps\n taken each day",
     font.main = 12,
     xlab = "")
text(17000, 20,"NAs are replaced by median")

hist(total2_steps_each_day, 
     breaks = 10,
     col = "red",
     ylim = c(0, 25),
     xlab = "Total number of steps taken each day",
     main = "")
text(17000, 20,"NAs are replaced by mean")
```

![plot of chunk plot4](figure/plot4-1.png) 

After using 2nd strategy (NAs are replaced by mean) data set better describes real life activity of subject. I think that subject in his/her life hasn't got 10 days from 2 month without moving when total number of steps per day is less than 2000).

Mean and median of total number of steps taken per day after realizing *1st* strategy:

```r
mean(total1_steps_each_day)
```

```
## [1] 9503.869
```

```r
median(total1_steps_each_day)
```

```
## [1] 10395
```

These values differ from mean (particular) and median of total number of steps taken per day in data set *with NAs*.
 
 
Mean and median of total number of steps taken per day after realizing *2nd* strategy:

```r
mean(total2_steps_each_day)
```

```
## [1] 10766.19
```

```r
median(total2_steps_each_day)
```

```
## [1] 10766.19
```

These values are practically equal mean and median of total number of steps taken per day in data set *with NAs*. That is very well.



```r
avr1_steps_each_interval <- tapply(activity1$steps, 
                                   activity1$interval, 
                                   FUN = mean, 
                                  na.rm = TRUE, simplify = TRUE)
avr2_steps_each_interval <- tapply(activity2$steps, 
                                   activity2$interval, 
                                   FUN = mean, 
                                   na.rm = TRUE, simplify = TRUE)
```


```r
par(mfrow = c(1,1), mar=c(4,4,1,1))
plot(intervals, avr2_steps_each_interval, 
     type = "l", col = "red",
     xlab = "Interval", 
     ylab = "Average number of steps taken, averaged across all days")
lines(intervals, avr1_steps_each_interval, 
     col = "blue")
legend(x=600, y=200, lty = "solid", 
       col = c("blue", "red"), 
       legend = c("NAs are replaced by median", "NAs are replaced by mean"),
       bty = "n")
```

![plot of chunk plot5](figure/plot5-1.png) 

My conclusion, 2nd strategy (NAs are replaced by mean) in this case is better than 1st strategy (NAs are replaced by median). I will data set 'activity2' for next part of this assignment.

## Are there differences in activity patterns between weekdays and weekends?

Creatinga new data frame 'activity3' on basis of 'activity2' and new variables in this dataset:  
-character vector with names of weekdays ('week.days')  
-and factor 'wd' with two levels - "weekday" and "weekend" indicating whether a given date is a weekday or weekend day.

```r
activity3 = transform(activity2, 
                      week.days = weekdays(date, abbreviate = TRUE),
                      wd = as.character("weekday"),
                      stringsAsFactors = FALSE) # !!!!!!
wkendIndex <- (activity3$week.days == "Sun" | activity3$week.days == "Sat")
activity3$wd[wkendIndex] <- "weekend"
activity3 <- transform(activity3, wd = factor(wd))
```

Creating two separated data sets for weekends and weekdays using logical vector 'wkendIndex':

```r
activity3wkday <- activity3[!wkendIndex,]
activity3wkend <- activity3[wkendIndex,]
```

Calculation the average number of steps taken per day, averaged across all weekday days or weekend days:

```r
avr3_steps_each_interval_wkday <- tapply(activity3wkday$steps, 
                                         activity3wkday$interval,
                                         FUN = mean, 
                                         na.rm = TRUE, simplify = TRUE)
avr3_steps_each_interval_wkend <- tapply(activity3wkend$steps, 
                                         activity3wkend$interval,
                                         FUN = mean, 
                                         na.rm = TRUE, simplify = TRUE)
```

Making a panel plot containing a time series plot of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all weekday days or weekend days (y-axis).

```r
ymax_on_plot6 <- round(max(avr3_steps_each_interval_wkend, avr3_steps_each_interval_wkday))+10
par(mfrow = c(2,1), mar=c(4,4,1.5,1))
plot(intervals, avr3_steps_each_interval_wkend, 
     type = "l", 
     xlim = c(0, nIntervals*5), 
     ylim = c(0, ymax_on_plot6), 
     xlab = "", ylab = "Number of steps",
     main = "weekend", 
     font.main = 12, 
     font.lab = 10,  
     font.axis = 8,
     font = 1)
plot(intervals, avr3_steps_each_interval_wkday, 
     type = "l", 
     xlim = c(0, nIntervals*5), 
     ylim = c(0, ymax_on_plot6), 
     xlab = "interval", ylab = "Number of steps",
     main = "weekday",
     font.main = 12, 
     font.lab = 10,  
     font.axis = 8,
     font = 1)
```

![plot of chunk plot6](figure/plot6-1.png) 


###Answer:  
Yes, there are differences in activity patterns between weekdays and weekends.
On weekends subject have more of going/running than on weekdays. Perhups subject have sedentary job.
