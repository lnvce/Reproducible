--- 
title: "Reproducible Research: Peer Assessment 1" 
output: 
  html_document: 
    keep_md: true 
---

# Version TWO
## Loading and preprocessing the data. Task 1.

```r
DF <- read.csv("activity.csv", header=T, sep=',', na.strings="NA", check.names=F, stringsAsFactors=F, comment.char="", quote='\"')
DF2 <- DF
DF2$Date <- as.Date(as.character(DF2$date,format="%Y-%m-%d"))
cat("Summary")
```

```
## Summary
```

```r
cat(summary(DF2))
```

```
## Min.   :  0.00   1st Qu.:  0.00   Median :  0.00   Mean   : 37.38   3rd Qu.: 12.00   Max.   :806.00   NA's   :2304   Length:17568       Class :character   Mode  :character   NA NA NA NA Min.   :   0.0   1st Qu.: 588.8   Median :1177.5   Mean   :1177.5   3rd Qu.:1766.2   Max.   :2355.0   NA Min.   :2012-10-01   1st Qu.:2012-10-16   Median :2012-10-31   Mean   :2012-10-31   3rd Qu.:2012-11-15   Max.   :2012-11-30   NA
```

## What is mean total number of steps taken per day? 

```r
library(dplyr)
```

```
## 
## Attaching package: 'dplyr'
```

```
## The following objects are masked from 'package:stats':
## 
##     filter, lag
```

```
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

```r
stepsDay <- aggregate (steps ~ date, DF2, FUN = sum)

cat(paste("Total number of actiivities taken ",nrow(DF2)))
```

```
## Total number of actiivities taken  17568
```

```r
cat(paste("Mean steps taken per day ",round(mean(stepsDay$steps,na.rm = TRUE),1)))
```

```
## Mean steps taken per day  10766.2
```

```r
cat(paste("Median steps taken per day ",median(stepsDay$steps, rm = TRUE)))
```

```
## Median steps taken per day  10765
```

```r
tot_steps <- DF2 %>% group_by(date) %>%
        summarise(no_steps = sum(steps)) %>%
        filter(!is.na(no_steps)) %>%
        arrange(date)
tot_steps$date <- as.Date(as.character(tot_steps$date,format="%Y-%m-%d"))
library(ggplot2)

ggplot(tot_steps, aes(x = no_steps)) + 
  geom_histogram(breaks = seq(0, 22000, by = 2000) ) + 
  labs(x = "Number of steps per day", y = "Frequency of days") + 
  labs(title = "Total number of steps taken per day") +
  theme_light()
```

![](PA1_template_files/figure-html/process_data-1.png)<!-- -->

## What is the average daily activity pattern?

The 'Not a Number"  values excluded from the calculations.


```r
mean(tot_steps$no_steps, na.rm = TRUE)
```

```
## [1] 10766.19
```

```r
median(tot_steps$no_steps, na.rm = TRUE)
```

```
## [1] 10765
```

```r
DFX <- DF %>% group_by(interval) %>% 
         arrange(interval) %>% 
         summarise(mean_value = mean(steps, na.rm = TRUE))


with (DFX, plot(mean_value, pch=20, type = "l"))
abline(h=median(DFX$mean_value), col="red")
title(main="Mean Steps per Day")
```

![](PA1_template_files/figure-html/average_daily_patten-1.png)<!-- -->

```r
DFZ <- DF %>% group_by(interval) %>% 
         arrange(interval) %>% 
         summarise(max_value = max(steps, na.rm = TRUE))

with (DFZ, plot(max_value, pch=20, type = "l"))
abline(h=median (DFZ$max_value), col="red")
title(main="Maximum Steps per Day")
```

![](PA1_template_files/figure-html/average_daily_patten-2.png)<!-- -->

## Imputing missing values
There are

```r
nrow(filter(DF, is.na(steps)))
```

```
## [1] 2304
```
Rows with missing values ....
This data set has

```r
rows_missing_steps <- is.na(DF$steps)
cat(paste("Missing rows of data is ",sum(rows_missing_steps)))
```

```
## Missing rows of data is  2304
```

The missing values will be replaced with the median number of steps.


```r
DF3 <- DF
DF3$date <- as.Date(as.character(DF3$date,format="%Y-%m-%d"))
rows_missing_steps <- is.na(DF$steps)
DF3$steps[rows_missing_steps] <- median(DF3$steps,na.rm = TRUE )


tot_steps3 <- aggregate(DF3$steps, by=list(date=DF3$date), FUN=sum)
names(tot_steps3)[2]<-paste("total_steps")
tot_steps3$date <- as.Date(as.character(tot_steps3$date,format="%Y-%m-%d"))
with (tot_steps3, hist(date, breaks = 61,  main="Total Number of Step Taken Each Day", ylab="Derived Total Steps", xlab="Day", col="red"))
```

![](PA1_template_files/figure-html/plot_data-1.png)<!-- -->
Replacing the missing values does mpacts the mean and median total number of steps taken per day. There is more acitity found in the derived data.


```r
mean_steps3 <- aggregate(DF3$steps, by=list(date=DF3$date), FUN=mean)
mean_steps3$date <- as.Date(as.character(mean_steps3$date,format="%Y-%m-%d"))
par(mfrow = c(1, 1), mar = c(4, 4, 2, 1), oma = c(0, 0, 2, 0))
with (mean_steps3, plot(date, x, xlab = "Date", ylab="Derived Average Number of Steps",main = "Derived Mean Steps per Day", type="l", col="red"))
abline(h=median(mean_steps3$x))
```

![](PA1_template_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

The median (horizaontal line) in the  plots have differnet values.

## Are there differences in activity patterns between weekdays and weekends?


```r
DF3$DOW <- as.POSIXlt(DF3$date)$wday
DOW_steps <- aggregate(DF3$steps, by=list(DOW=DF3$DOW), FUN=sum)
DOW_steps$DOW[7] <- -1

# barplot(log10(DOW_steps$x), col="blue", ylim=c(0, 6))
barplot(log10(DOW_steps$x), col="blue", ylim=c(0, 6))
axis(1, at=1:7, label=c("Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"))
mtext(side=1,text="Day of the Week",line=2.2)
mtext(side=2,text="Derived Steps per Day of the Week (log10)",line=2.2)
title(main="Total Steps per Day of the Week", col.main="red", font.main=4)
```

![](PA1_template_files/figure-html/find_DOW_pattern-1.png)<!-- -->

The log10(total steps) for weekends is

```r
log10dy <- log10(DOW_steps$x[1]) + log10(DOW_steps$x[2])+  log10(DOW_steps$x[3]) +  log(DOW_steps$x[4]) +  log10(DOW_steps$x[5])
logwkn <- log10(DOW_steps$x[7]) + log10(DOW_steps$x[1])
cat(paste("Steps (log10) taken per weekend ", logwkn ))
```

```
## Steps (log10) taken per weekend  9.87745278929884
```
The total (log10) steps for weekdays is

```r
log10dy <- log10(DOW_steps$x[1]) + log10(DOW_steps$x[2])+  log10(DOW_steps$x[3]) +  log(DOW_steps$x[4]) +  log10(DOW_steps$x[5])
cat(paste("Steps (log10) taken per WEEKDAY ", log10dy ))
```

```
## Steps (log10) taken per WEEKDAY  30.9563550013711
```
Conclusion: People are more active on weekdays vs. weekends.
 

