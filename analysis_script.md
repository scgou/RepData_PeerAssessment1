# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data


```r
library(tidyverse)
```

```
## Loading tidyverse: ggplot2
## Loading tidyverse: tibble
## Loading tidyverse: tidyr
## Loading tidyverse: readr
## Loading tidyverse: purrr
## Loading tidyverse: dplyr
```

```
## Conflicts with tidy packages ----------------------------------------------
```

```
## filter(): dplyr, stats
## lag():    dplyr, stats
```

```r
data <- read.csv("activity.csv")
```



## Mean total number of steps taken per day

```r
options(scipen=999)
ds <- data %>% group_by(date) %>% summarise(daily = sum(steps))
ms <- round(mean(ds$daily, na.rm=TRUE), 0)
md <- round(median(ds$daily, na.rm=TRUE), 0)
hist(ds$daily, main="Histogram of daily steps totals", xlab="Daily steps")
```

![](analysis_script_files/figure-html/unnamed-chunk-2-1.png)<!-- -->

The mean of the daily steps is 10766 and the median is 10765.


## What is the average daily activity pattern?

```r
dint <- data %>% group_by(interval) %>% summarise(avg=mean(steps, na.rm=TRUE))
intmax <- dint[which.max(dint$avg),]$interval
plot(dint$interval, dint$avg, type="l", main="Time series plot of average steps per interval", xlab="Interval (5-minute)", ylab="Average steps")
```

![](analysis_script_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

The 5-minute interval which contains the maximum number of steps averaged across all the days is 835.

## Imputing missing values
Missing values will be the interval averaged ones calculated above.

```r
mv <- sum(is.na(data$steps))
cdata <- data.frame(data)
for (i in 1:length(cdata$steps)) {
  if (is.na(cdata$steps[i])) {
    cdata$steps[i] <- dint$avg[which(dint$interval == cdata$interval[i])]
  }
}
cds <- cdata %>% group_by(date) %>% summarise(daily = sum(steps))
cms <- round(mean(cds$daily, na.rm=TRUE), 0)
cmd <- round(median(cds$daily, na.rm=TRUE), 0)
hist(cds$daily, main="Histogram of NA corrected daily steps totals", xlab="Daily steps")
```

![](analysis_script_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

There are 2304 missing values in the step data. I replaced the missing values with the average number of steps for the specific intervals calculated across all the days.

For this corrected data, the new mean of the daily steps is 10766 and the new median is 10766.


## Are there differences in activity patterns between weekdays and weekends?
