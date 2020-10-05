Week 2 Programming Assignment
=============================



This markdown corresponds to the programming assignment 1 of the JHU's Reproducible Search course in Coursera. The intention of this project is to develop abilities in using markdown and knitr package.

In this assignment I will analyze kinestesic data recollected by monitoring devices as nike fuelband or fitbit. This assignment makes use of data from a personal activity monitoring device. This device collects data at 5 minute intervals through out the day. The data consists of two months of data from an anonymous individual collected during the months of October and November, 2012 and include the number of steps taken in 5 minute intervals each day. To see the complete instructions and the data used in this assignment click in this [link](https://www.coursera.org/learn/reproducible-research/peer/gYyPt/course-project-1)

Load the needed libraries


  
## 1.-Loading and preprocessing the data
Show any code that is needed to

1. Load the data (i.e. \color{red}{\verb|read.csv()|}read.csv())
2. Process/transform the data (if necessary) into a format suitable for your analysis

```r
# read the activity dataset
act <- read.csv("activity.csv")
```

```
## Warning in file(file, "rt"): no fue posible abrir el archivo 'activity.csv': No
## such file or directory
```

```
## Error in file(file, "rt"): no se puede abrir la conexión
```
  
  
## 2.-What is mean total number of steps taken per day?
For this part of the assignment, you can ignore the missing values in the dataset.

1. Calculate the total number of steps taken per day
2. If you do not understand the difference between a histogram and a barplot, research the difference between them. Make a histogram of the total number of steps taken each day
3. Calculate and report the mean and median of the total number of steps taken per day


```r
# sum of the steps taken per day
steps_perday <- act %>%
        group_by(date) %>%
        summarise(total_steps = sum(steps, na.rm = TRUE))
```

```
## Error in eval(lhs, parent, parent): objeto 'act' no encontrado
```

```r
# mean and median of total steps per day
steps_perday %>% summarise(mean = mean(total_steps, na.rm = TRUE), median = median(total_steps, na.rm = TRUE))
```

```
## Error in eval(lhs, parent, parent): objeto 'steps_perday' no encontrado
```

```r
# plot the distribution of total steps per day
ggplot(steps_perday, aes(total_steps)) + 
        geom_histogram(bins = 15)
```

```
## Error in ggplot(steps_perday, aes(total_steps)): objeto 'steps_perday' no encontrado
```
  
## 3.-What is the average daily activity pattern?
1. Make a time series plot (i.e. \color{red}{\verb|type = "l"|}type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)
2. Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?


```r
# Get the mean of steps and group it by the 5 minute intervals
averageStepsTime <- aggregate(x=list(meanSteps=act$steps), 
                              by=list(interval=act$interval), mean, na.rm=TRUE)
```

```
## Error in aggregate(x = list(meanSteps = act$steps), by = list(interval = act$interval), : objeto 'act' no encontrado
```

```r
# Which 5 minute interval contains the max number of steps?
maxsteps <- which.max(averageStepsTime$meanSteps)
```

```
## Error in which.max(averageStepsTime$meanSteps): objeto 'averageStepsTime' no encontrado
```

```r
slice(averageStepsTime, maxsteps)
```

```
## Error in slice(averageStepsTime, maxsteps): objeto 'averageStepsTime' no encontrado
```

```r
# plot the data
ggplot(data=averageStepsTime, aes(x=interval, y=meanSteps)) +
    geom_line() +
    xlab("5-minute time interval") +
    ylab("average steps taken")
```

```
## Error in ggplot(data = averageStepsTime, aes(x = interval, y = meanSteps)): objeto 'averageStepsTime' no encontrado
```

## 4-.Imputing missing values
Note that there are a number of days/intervals where there are missing values (coded as \color{red}{\verb|NA|}NA). The presence of missing days may introduce bias into some calculations or summaries of the data.

1. Calculate and report the total number of missing values in the dataset (i.e. the total number of rows with \color{red}{\verb|NA|}NAs)
2. Devise a strategy for filling in all of the missing values in the dataset. The strategy does not need to be sophisticated. For example, you could use the mean/median for that day, or the mean for that 5-minute interval, etc.
3. Create a new dataset that is equal to the original dataset but with the missing data filled in.
4. Make a histogram of the total number of steps taken each day and Calculate and report the mean and median total number of steps taken per day. Do these values differ from the estimates from the first part of the assignment? What is the impact of imputing missing data on the estimates of the total daily number of steps?


```r
# get the number of NA's in the dataset
isna <- is.na(act)
```

```
## Error in eval(expr, envir, enclos): objeto 'act' no encontrado
```

```r
length(which(isna))
```

```
## Error in which(isna): objeto 'isna' no encontrado
```

```r
# fill the missings values with the median
actimputed <- act
```

```
## Error in eval(expr, envir, enclos): objeto 'act' no encontrado
```

```r
  actimputed <- impute(act,object = NULL, method = "median/mode", flag = FALSE)
```

```
## Error in lapply(X = X, FUN = FUN, ...): objeto 'act' no encontrado
```

```r
# calculate the total number of steps taken per day
steps_perdayImputed <- tapply(actimputed$steps, actimputed$date, sum)
```

```
## Error in tapply(actimputed$steps, actimputed$date, sum): objeto 'actimputed' no encontrado
```

```r
# make histogram of total number of steps taken each day
hist(steps_perdayImputed, xlab = "Total Steps", color = "red", border = "dark blue", main = "Total steps per day with missing values replaced")
```

```
## Error in hist(steps_perdayImputed, xlab = "Total Steps", color = "red", : objeto 'steps_perdayImputed' no encontrado
```

```r
# calculate and report the mean and the median
median(steps_perday$total_steps)
```

```
## Error in median(steps_perday$total_steps): objeto 'steps_perday' no encontrado
```

```r
mean(steps_perday$total_steps)
```

```
## Error in mean(steps_perday$total_steps): objeto 'steps_perday' no encontrado
```

## 5.- Are there differences in activity patterns between weekdays and weekends?
For this part the \color{red}{\verb|weekdays()|}weekdays() function may be of some help here. Use the dataset with the filled-in missing values for this part.

1. Create a new factor variable in the dataset with two levels – “weekday” and “weekend” indicating whether a given date is a weekday or weekend day.
2. Make a panel plot containing a time series plot (i.e. \color{red}{\verb|type = "l"|}type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all weekday days or weekend days (y-axis). See the README file in the GitHub repository to see an example of what this plot should look like using simulated data.


```r
  actimputed$dateType <-  ifelse(as.POSIXlt(actimputed$date)$wday %in% c(0,6), 'weekend', 'weekday')
```

```
## Error in as.POSIXlt(actimputed$date): objeto 'actimputed' no encontrado
```

```r
  averagedActivityImputed <- aggregate(steps ~ interval + dateType, data=actimputed, mean)
```

```
## Error in eval(m$data, parent.frame()): objeto 'actimputed' no encontrado
```

```r
  ggplot(averagedActivityImputed, aes(interval, steps)) + 
    geom_line() + 
    facet_grid(dateType ~ .) +
    xlab("5-minute interval") + 
    ylab("avarage number of steps")
```

```
## Error in ggplot(averagedActivityImputed, aes(interval, steps)): objeto 'averagedActivityImputed' no encontrado
```
