# Reproducible Research: Peer Assessment 1


# Loading and preprocessing the data


```r
data <- read.csv("activity.csv")
```



# Mean total number of steps taken per day

```r
ds <- aggregate(data$steps, by=list(date=data$date), FUN=sum)
hist(ds$x)
```

![](analysis_script_files/figure-html/unnamed-chunk-2-1.png)<!-- -->


## What is the average daily activity pattern?



## Imputing missing values



## Are there differences in activity patterns between weekdays and weekends?
