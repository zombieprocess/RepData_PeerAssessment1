---
title: "Reproducible Research: Peer Assessment 1"
output: 
  html_document:
    keep_md: true
---


## Loading and preprocessing the data


```r
library(knitr)
#fig.path("figures")
#base.dir("~/sandbox/repdata-008/RepData_PeerAssessment1")

data <- read.csv("activity.csv")
```
## What is mean total number of steps taken per day?


```r
# Libraries
library(plyr)
library(ggplot2)
library(scales)   # For pretty_breaks()

# Print mean_steps to file.
png(filename = "./mean_steps.png")

sumdata <- ddply(data, .(date), summarise, sum_steps = sum(steps, na.rm=TRUE))
meansum <- mean(sumdata$sum_steps)
mediansum <- median(sumdata$sum_steps)

# Setup the plot
p1 <- ggplot(data=sumdata, aes(x=sum_steps)) + geom_histogram(binwidth=2000,colour="black",fill="white") + geom_vline(aes(xintercept=mean(sum_steps, na.rm=TRUE)), color="red", linetype="dashed", size=1) + geom_vline(aes(xintercept=median(sum_steps, na.rm=TRUE)), color="blue", linetype="dashed", size=1)

# Make it pretty
p1 + ylab("Count") + xlab("Total Steps") + geom_text(aes(meansum - 4000,12,label=paste("Mean ~ ",as.integer(meansum)),color="red")) + geom_text(aes(mediansum + 6000, 12, label=paste("Median = ",mediansum)),color="blue") + theme(legend.position = "none")

# Close device
dev.off()
```

![plot of chunk unnamed-chunk-2](figure/unnamed-chunk-2.png) 

```
## RStudioGD 
##         2
```

The mean total number of steps taken per day are 9354.2295.



## What is the average daily activity pattern?

A time series plot of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)



```r
# Calculate the 5 minute interval average number of steps
fiveInterval <- ddply(data, .(interval), summarise, avgSteps = mean(steps, na.rm=TRUE))

# Print avg_interval to file.
png(filename = "./avg_interval.png")

p2 <- ggplot(data=fiveInterval, aes(x=interval)) + geom_line(aes(y=avgSteps))
p2 +  ylab("Average Steps for all days") + xlab("5-minute Interval")

# Close device
dev.off()
```

![plot of chunk unnamed-chunk-3](figure/unnamed-chunk-3.png) 

```
## RStudioGD 
##         2
```



```r
# Determine the max value interval and save it
maxInt_idx <- which.max(fiveInterval$avgSteps)
```

The 5-minute interval 835, on average across all the days in the dataset, contains the maximum number of 206.1698 steps.


## Imputing missing values


```r
# Using the previous calculated fiveInterval averages, fill in missing data

data_more <- as.data.frame(data)
#data_more[is.na(steps)] <- 
  
#  data_more[with(data_more,is.na(steps)),]$steps <-

#  with(data_more, ifelse(is.na(steps)), match(fiveInterval$interval, 
                                              
  # Lookup table for missing value
#  fiveInterval[which(fiveInterval$interval == data_more$interval),2]                                            
```

I decided to use the 5 minute intervals average values since I have them on hand. I was able to use it as a lookup table to replace the NA's.

## Are there differences in activity patterns between weekdays and weekends?


```r
warning("Ack! Ran out of time, on the road for a conference!")
```

```
## Warning: Ack! Ran out of time, on the road for a conference!
```
