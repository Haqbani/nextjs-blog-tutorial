---
title: "Descriptive Statistics (Practical)"
subtitle: "Create an AWS account and set up CLI/SDK access."
date: "2020-12-27"
---

(/Users/haqbani/Downloads/Projects_19052024/nextjs-blog-tutorial/public/images/11.png)

# Descriptive Statistics (Practical)

## **Descriptive Statistics (summary statistics)**

We use descriptive statistics to summarize sample data from a population when collecting entire population data is impractical, when dealing with data about human beings in organizations, it’s not feasible to attain data for the entire population, so instead we settle for what is hopefully a representative sample of individuals from the focal population.

These include counts (frequencies), measures of central tendency (mean, median, mode), and measures of dispersion (variance, standard deviation, interquartile range). These are not statistical significance tests; for those, we use inferential statistics like independent-samples t-test, multiple linear regression.

Descriptive statistics help summarize sample characteristics, such as employee demographic data, including race/ethnicity or average age. There are different statistics for categorical and continuous variables. For categorical variables (nominal or ordinal scale), we often use counts. For continuous variables (interval or ratio scale), we use central tendency and dispersion measures. 

| Statistics Type | Application | Example |
| --- | --- | --- |
| Counts | Nominal or ordinal scales | Counting employees by department or location |
| Measures of Central Tendency & Dispersion | Interval or ratio scales | Calculating median salary and its interquartile range |
| Conversion of ordinal scales to interval scales | Assigning numeric values to ordinal scale categories | Assigning numbers (1-5) to Likert responses |

**Sample Write-Up Example**

For an organization's employee demographics, counts were used for gender and race/ethnicity, and central tendency and dispersion measures for age. Employee ages were normally distributed, with an average of 42.13 years and standard deviation of 7.71.

## Go Practical with R

set-up the envirmont and the files (*You can dawinload the files from github*)

[https://github.com/Haqbani/R-Tutorial-Data-Files-HR](https://github.com/Haqbani/R-Tutorial-Data-Files-HR)

### Seting up

```r
# Set workign dierctory
setwd("/Users/haqbani/Library/CloudStorage/Dropbox/5 _courses/54_R_for_HR/R-Tutorial-Data-Files-master")

# load the data
demo <- read.csv("employee_demo.csv")

# View the data
view(demo)
```

### Data strucutre

As we can see from the data “demo” containes 5 veribles as follows: (if you want a refresh or new to the data types in R → [go to this page.](https://www.notion.so/R-data-types-0e4c7b40b3cb4e278b73697ab0552f03?pvs=21)

| Verible | Measument Scale | Data Type |
| --- | --- | --- |
| EmpID | Nominal | Charecter |
| Facility | Categorical | Charecter |
| Education | Orderinal | Charecter |
| Performance | Ratio | Numircal |
| Age | Interval | Integer |

### Counting number of employees

Here we are going to count the number of employees in each Facility:

```r
table(demo$Facility)
```

Results:

| Beaverton | Hillsboro | Portland |
| --- | --- | --- |
| 15 | 5 | 10 |

Let's count how many employees are at each level. The term "Level" is an order-based category, like "College Degree", "Some College", "High School Diploma". R doesn't know their order, so we need to tell it. The order goes from the highest, "College Degree", to the lowest, "High School Diploma". We can use an R function called "factor" to set this order. Check out the code below:

```r
demo$Education <- factor(demo$Education,  
order = TRUE,
levels = c("College Degree", "Some College", "High School Diploma")
```

Results:

```r
[1] "College Degree"      "Some College"      "High School Diploma”
```

Let's visualize the data. Remember, we're counting to make sense of our data descriptively, so picking the right visualization is crucial. Your choice of visualization depends on the type of data you have: ordinal, nominal, or ratio. Choose wisely.

<aside>
📢 sometime we call data Visualiztion “Viz” for short

</aside>

### Center Tendancy and Verance

- **Central Tendency**: Provides a single value that represents the center of the data.
    - **Mean**: Average of all values.
    - **Median**: Middle value when data is ordered.
    - **Mode**: Most frequently occurring value.
- **Variance and Standard Deviation**: Measures of how spread out the data is.
    - **Variance**: Average of the squared differences from the mean.
    - **Standard Deviation**: Square root of the variance, giving a measure in the same units as the data.
    
- **Age Distribution**

```r
# ploting hisegram
hist(demo$Age,
     main = "Age Distribution",
     xlab = "Age",
     ylab = "Count", 
     col = "slateblue",
     border = "black",
     breaks = 5,
     ylim = c(0, 20))
     
# Calculating the mean and median of age     
mean(demo$Age, na.rm = TRUE) = 28
median(demo$Age, na.rm = TRUE) = 28
sd(demo$Age, na.rm = TRUE)= 2.66
```

**Standard Deviation**: The standard deviation (2.67) tells us how much the individual ages typically differ from the average age (28). Most ages are between 25.33 and 30.67 years. This means the ages are quite close to the average age of 28, not spread out very far.

![Untitled](Descriptive%20Statistics%20(Practical)%2072ca6633e1c94c6f867957491aa1e18d/Untitled.png)

**interpreation**: 

The above histogram shows that there is almot normal distrpution where the median and mean are both 28 (right in the middle). However, this is not allwayes the case, we can validate more by calculating [Skewness and Kurtosis](https://datasharkie.com/descriptive-statistics-in-r/). 

```r

skewness(demo$Age, na.rm = TRUE)
kurtosis(demo$Age, na.rm = TRUE)
```

Results:

```r
#The skewness value is very close to zero (-0.0106), indicating that 
the distribution of the Age variable is almost perfectly symmetrical.
> skewness(demo$Age, na.rm = TRUE)[1] -0.01056395>
 
 #A kurtosis value close to zero indicates that the tails of the Age distribution 
 are similar to those of a normal distribution. This means that the Age data does
 not have an unusual number of extreme values or outliers; it has a typical spread
>kurtosis(demo$Age, na.rm = TRUE)[1] 0.02952367
```

- **Performance Distribution**

![2024-05-19_01-25-02.png](Descriptive%20Statistics%20(Practical)%2072ca6633e1c94c6f867957491aa1e18d/2024-05-19_01-25-02.png)

The thick horizontal line in the middle of the box is the median score, the lower edge of the box represents the lower quartile (i.e., 25th percentile, median of lower half of the distribution), and the upper edge of the box represents the upper quartile (i.e., 75th percentile, median of the upper half of the distribution).

```r
quantile(demo$Performance, 0.25) # 25th precintile
quantile(demo$Performance, 0.75) # 75th precntile
quantile(demo$Performance, 0.50) # 50th prcintil or the median
IQR(demo$Performance) # Interqurtile tange IQR
```

Results

```r
quantile(demo$Performance, 0.25)25% 6.225
quantile(demo$Performance, 0.75)75% 8.3
quantile(demo$Performance, 0.50)50% 7.5
IQR(demo$Performance)2.075
```

 The height of the box is the interquartile range. By default, the `boxplot` function sets the upper “whisker” (i.e., the horizontal line at the top of the upper dashed line) as the smaller of two values: the maximum value or 1.5 times the interquartile range. 

Further, the function sets the lower “whisker” (i.e., the horizontal line at the bottom of the lower dashed line) as the larger of two values: the minimum value or 1.5 times the interquartile range.

In the box plot for `Performance`, we can see that the distribution of scores appears to be slightly negatively skewed, as the upper quartile is smaller than the lower quartile (i.e., the median is closer to the top of the box) and the upper whisker is shorter than the lower whisker. If there had been any outlier scores, these would appear beyond the upper and lower limits of the whiskers.

**Thank you 🙂**