# Data-Science---Module-6-Assignment
The data used in this assignment consists of weekly sales data from two departments of two stores, and simulated data about the number of visitors and conversions the stores had. The aim of this assignment is to use this information to gain insight on the purchasing habits of customers shopping during holidays versus non holidays, and whether sales differed between years and/or stores.

**Question 1**  
For this assignment, you will need to load the `dplyr`, `ggplot2`, `tidyr`, and `lubridate` packages, and import the data contained in the file `salesdata.csv`.  

*Instructions:*  
- Load the required packages.  
- Import the data and store it in an object called `salesdata`.  
- Use functions like `head()`, `glimpse()`, and `summary()` to familiarize yourself with the data.

*Assignment Code:*  
```{r}
# Load libraries
library(dplyr)
library(ggplot2)
library(tidyr)
library(lubridate)

# Import the data
salesdata <- read.csv(‘salesdata.csv’)

# Familiarize yourself with the data
head(salesdata)
•	 •	 	date
<fctr>	store
<int>	department
<int>	is_holiday
<lgl>	visitors
<int>	conversions
<int>	weekly_sales
<dbl>
1	2010-02-05	1	1	FALSE	180	97	24924.50
2	2010-02-12	1	1	TRUE	256	109	46039.49
3	2010-02-19	1	1	FALSE	246	74	41595.55
4	2010-02-26	1	1	FALSE	205	89	19403.54
5	2010-03-05	1	1	FALSE	217	93	21827.90
6	2010-03-12	1	1	FALSE	177	58	21043.39
6 rows

glimpse(salesdata)
Variables: 7
$ date         <fct> 2010-02-05, 2010-02-12, 2010-02-19…
$ store        <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1…
$ department   <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1…
$ is_holiday   <lgl> FALSE, TRUE, FALSE, FALSE, FALSE, …
$ visitors     <int> 180, 256, 246, 205, 217, 177, 230,…
$ conversions  <int> 97, 109, 74, 89, 93, 58, 110, 78, …
$ weekly_sales <dbl> 24924.50, 46039.49, 41595.55, 1940…

summary(salesdata)
date         store       department 
 2010-02-05:  4   Min.   :1.0   Min.   :1.0  
 2010-02-12:  4   1st Qu.:1.0   1st Qu.:1.0  
 2010-02-19:  4   Median :1.5   Median :1.5  
 2010-02-26:  4   Mean   :1.5   Mean   :1.5  
 2010-03-05:  4   3rd Qu.:2.0   3rd Qu.:2.0  
 2010-03-12:  4   Max.   :2.0   Max.   :2.0  
 (Other)   :376                             

is_holiday         visitors      conversions    
 Mode :logical   Min.   :135.0   Min.   : 31.00  
 FALSE:368       1st Qu.:201.0   1st Qu.: 86.75  
 TRUE :32        Median :225.0   Median :101.00  
                 Mean   :225.1   Mean   :101.06  
                 3rd Qu.:249.0   3rd Qu.:115.25  
                 Max.   :312.0   Max.   :172.00  

weekly_sales  
 Min.   :14537  
 1st Qu.:22795  
 Median :43679  
 Mean   :41525  
 3rd Qu.:60530  
 Max.   :93511 

*Feedback:*  
You can see that `salesdata` contains information about the weekly sales (`weekly_sales`) of two stores (`store`), by `department`, from 2010 to 2012 (`date`), and includes whether or not a holiday fell within that week (`is_holiday`). It also includes the number of `visitors` and how many of those visitors made a purchase (`conversions`).

**Question 2**  
Notice that at the moment, the `date` column is a character (factor) vector. This is a common challenge when dealing with data, and while we haven't had time to cover it in the webinars, we'll take this opportunity to introduce you to two functions from the packages `{lubridate}` that can be used to convert this column from a factor to a date class. Once complete, you'll specifically extract the year of each date so that annual data can be considered.

*Instructions:*  
- Using `mutate()`, convert the `date` column from `factor` to `date` with the `{lubridate}` function `ymd()`. This function takes a character or factor vector and converts it to a date vector by reading the first component of each element as year, the second as month, and the third as day. It is a fairly "smart" function, so the dashes you see in the `date` column will automatically be recognized as break points to differentiate between the three components. Store this data frame in an object called `salesdata_step1`.  

- Compare the structure of `salesdata` and `salesdata_step1` using `glimpse()`; although they look similar, the underlying class of `date` has changed. The values it contains now have meaning in the context of date and time rather than simply being strings of characters.  

- Using `mutate()`, create a column called `year` that contains the year information for each record. The year can be extracted from the newly-formatted `date` column using the `{lubridate}` function `year()`. Store this updated data frame in an object called `salesdata_clean`.

*Assignment Code:*  
```{r}
# Convert date from factor to date using ymd(). Takes the salesdata datset and pipes it into the mutate function that takes the date variable and converts it from class factor to class date using the lubridate function ymd AND THEN storing that altered dataset into the object salesdata_step1.
salesdata_step1 <- salesdata %>%
	mutate(date = ymd(date))

# Compare the structure of salesdata and salesdata_step1. Verification that the data variable has been changed from class factor to class data
glimpse(salesdata)
•	Observations: 400
•	Variables: 7
•	$ date         <fct> 2010-02-05, 2010-02-12, 2010-02-19, 2010-02-26, 2010-03-05, 2010-03-12, 2…
•	$ store        <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1…
•	$ department   <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1…
•	$ is_holiday   <lgl> FALSE, TRUE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
•	$ visitors     <int> 180, 256, 246, 205, 217, 177, 230, 223, 247, 277, 168, 251, 188, 180, 212…
•	$ conversions  <int> 97, 109, 74, 89, 93, 58, 110, 78, 91, 115, 68, 112, 102, 59, 104, 88, 77,…
•	$ weekly_sales <dbl> 24924.50, 46039.49, 41595.55, 19403.54, 21827.90, 21043.39, 22136.64, 262…
glimpse(salesdata_step1)
•	Observations: 400
•	Variables: 7
•	$ date         <date> 2010-02-05, 2010-02-12, 2010-02-19, 2010-02-26, 2010-03-05, 2010-03-12, …
•	$ store        <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1…
•	$ department   <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1…
•	$ is_holiday   <lgl> FALSE, TRUE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
•	$ visitors     <int> 180, 256, 246, 205, 217, 177, 230, 223, 247, 277, 168, 251, 188, 180, 212…
•	$ conversions  <int> 97, 109, 74, 89, 93, 58, 110, 78, 91, 115, 68, 112, 102, 59, 104, 88, 77,…
•	$ weekly_sales <dbl> 24924.50, 46039.49, 41595.55, 19403.54, 21827.90, 21043.39, 22136.64, 262…

# Create year column. Takes the salesdata_step1 datset and pipes it into the mutate function that creates the year variable and uses the lubridate function year to populate the year variable with extracted year values (2010 or 2011) from corresponding observations  from the date variable AND THEN storing that altered dataset into the object salesdata_clean.
salesdata_clean <- salesdata_step1 %>%
	mutate(year = year(date))
head(salesdata_clean)
•	 •	 	date
<date>	store
<int>	department
<int>	is_holiday
<lgl>	visitors
<int>	conversions
<int>	weekly_sales
<dbl>	year
<dbl>
1	2010-02-05	1	1	FALSE	180	97	24924.50	2010
2	2010-02-12	1	1	TRUE	256	109	46039.49	2010
3	2010-02-19	1	1	FALSE	246	74	41595.55	2010
4	2010-02-26	1	1	FALSE	205	89	19403.54	2010
5	2010-03-05	1	1	FALSE	217	93	21827.90	2010
6	2010-03-12	1	1	FALSE	177	58	21043.39	2010
6 rows

glimpse(salesdata_clean)
Observations: 400
Variables: 8
$ date         <date> 2010-02-05, 2010-02-12, 2010-02-19, 2010-02-26, 2010-03-05, 2010-03-12, …
$ store        <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1…
$ department   <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1…
$ is_holiday   <lgl> FALSE, TRUE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
$ visitors     <int> 180, 256, 246, 205, 217, 177, 230, 223, 247, 277, 168, 251, 188, 180, 212…
$ conversions  <int> 97, 109, 74, 89, 93, 58, 110, 78, 91, 115, 68, 112, 102, 59, 104, 88, 77,…
$ weekly_sales <dbl> 24924.50, 46039.49, 41595.55, 19403.54, 21827.90, 21043.39, 22136.64, 262…
$ year         <dbl> 2010, 2010, 2010, 2010, 2010, 2010, 2010, 2010, 2010, 2010, 2010, 2010, 2…

summary(salesdata_clean$year)
Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
2010    2010    2011    2011    2011    2011 

*Feedback:*  
Dates and times are a common challenge in data science. The `{lubridate}` package includes many handy functions for dealing with dates and times, including common tasks like calculating duration between two days or determining the day of the week, and more obscure tasks like converting between time zones and dealing with daylight savings time.

**Question 3**  
Let's imagine that you want to answer the question: "Is the proportion of visitors making purchases (i.e. conversions) significantly different between weeks with holidays and weeks with no holidays?" Here, your *null hypothesis* is that there is no significant difference in the proportions of conversions between weeks with holidays and weeks without.
•	

Start by summarising the data using `{dplyr}`. Then, use a *chi squared test* to determine whether the null hypothesis can be rejected. If the *p-value* returned by the chi squared test is less than the specified *alpha value*, the null hypothesis can be rejected (meaning there *is* a significant difference between the two values). Here you will use the alpha value 0.05, which means the probability of rejecting the null hypothesis when it is true is 5 %.

*Instructions:*   
- Use `group_by()` and `summarise()` to create a summary table from `salesdata_clean` that includes the total number of `conversions` and the total number of `visitors` for holiday and non-holiday periods. Store this data frame in an object called `sales_table1` with column names `is_holiday`, `total_conversions`, and `total_visitors`.
•	sales_table1 <- salesdata_clean %>%
	group_by(is_holiday) %>% 
	summarise(total_conversions = sum(conversions), 
	total_visitors = sum(visitors))
•	 
•	Or
•	View(sales_table1)
•	is_holiday	total_conversions	total_visitors
			
1	FALSE	36802	82921
2	TRUE	3623	7134
•	
	
- Apply a chi squared test using `prop.test()`. This function will require two arguments: a vector of values from the `total_conversion` column and a vector a values from the `total_visitors` column. Store the output of this test in an object called `test1`.
•	If calculating Chi-Squared from a summary table
o	 
o	 
•	---OR---
o	numerator <- c(4282, 5144) # 4282 = control conversions, 5144 = experiment conversions. c() function combines values into a vector list
o	denominator <- c(21971, 21840) # 21971 = control visitors, 21840 = experiment visitors. c() function combines values into a vector list
•	If calculating Chi-Squared directly from the dataset
o	# Using the c() function to create a vector that contains the sum of the Conv variable for the test group AND the sum of the Conv variable for control group AND THEN storing that in the object numerator
	numerator <- c(sum(test$Conv), sum(control$Conv))
	> numerator <- c(sum(test$Conv), sum(control$Conv))
o	# Using the c() function to create a vector that contains the length (i.e, the number of values contained within the variable, 0 and 1) of the Conv variable for the test group AND the length of the Conv variable for control group AND THEN storing that in the object denominator
	denominator <- c(length(test$Conv), length(control$Conv))
	> denominator <- c(length(test$Conv), length(control$Conv))
•	## In probability theory, a continuity correction is an adjustment (i.e, gives a more conservative estimation) that is made when a discrete distribution is approximated by a continuous distribution. The 20% conversion rate is continuous and would be a normally distributed if sampled over and over again
o	test <- prop.test(numerator, denominator, correct = FALSE) # the prop.test function runs the chi-squared test of proportions
•	Using the Chi-Squared method for a summary table:
o	numerator <- c(36802, 3623)
o	denominator <- c(82921, 7134)
o	test1 <- prop.test(numerator, denominator, conf.level = 0.95, correct = FALSE)

- If you reject the null hypothesis using an alpha of 0.05, store the value `"reject"` in an object called `result1`. If you do not reject it, store the value `"do not reject"` in an object called `result1`.
•	result1 <- c(‘reject’)

*Assignment Code:*  
```{r}
# Create a summary table
sales_table1 <- salesdata_clean %>%
	group_by(is_holiday) %>% 
	summarise(total_conversions = sum(conversions), total_visitors = sum(visitors))

# Look at sales_table1
sales_table1

# Apply a chi squared test of proportions
numerator <- c(36802, 3623)
denominator <- c(82921, 7134)
test1 <- prop.test(numerator, denominator, conf.level = 0.95, correct = FALSE)

# Look at test results
test1

# Create result1
result1 <- “reject”
```
*Feedback:*  
The results of the chi squared test tell you that there is a significant difference in the proportion of conversions between weeks with holidays and weeks without holidays. During weeks with holidays, the overall conversion rate was 50.8 % , while weeks without holidays experienced an overall conversion rate of 44.4 %. How might you use this information to provide recommendations to this business?

**Question 4**  
"Were the `weekly_sales` in 2010 significantly different than the `weekly_sales` in 2011?" Here, your *null hypothesis* is that there is no significant difference between the 2010 and 2011 sales.

Start by visualizing and summarising the data, then use a *t-test* to determine whether the null hypothesis can be rejected. If the *p-value* returned by the t-test is less than the specified *alpha value*, the null hypothesis can be rejected (meaning there *is* a significant difference between the two sets of values).  

*Instructions:*  
- Visualize the `salesdata_clean` data using `ggplot()` to create a boxplot with `year` on the x axis and `weekly_sales` on the y axis. Remember that you will need to treat `year` as a factor in `aes()` to ensure that a boxplot is drawn for each year. Store this plot in an object called `figure1`.  
- Use `group_by()` and `summarise()` to generate a summary table of the data containing the mean `weekly_sales` and standard deviation of `weekly_sales` for each `year`. The columns should be named `year`, `mean_sales`, and `sd_sales`. Store this data frame in an object called `sales_table2`.  
- Create a numeric vector called `year2010` containing all of the `weekly_sales` values from 2010. Hint: `pull()`  
- Create a numeric vector called `year2011` containing all of the `weekly_sales` values from 2011.  
- Apply a t-test using `t.test()`; it will require two arguments - the two vectors you have just created. Store the output of this test in an object called `test2`.  
- If you reject the null hypothesis using an alpha of 0.05, store the value `"reject"` in an object called `result2`. If you do not reject it, store the value `"do not reject"` in an object called `result2`.  
- Store the p-value of the t-test in an object called `p2`.  

*Assignment Code:*  
```{r}
# Visualize the data
figure1 <- ggplot(salesdata_clean) + geom_boxplot(aes(x = as.factor(year), y = weekly_sales)

# Display the figure
figure1
 ![image](https://user-images.githubusercontent.com/59670247/208313318-59c4bd35-d7c2-4f1b-b0d3-280edc38ffe7.png)


# Create a summary table
sales_table2 <- salesdata_clean %>%
	group_by(year) %>% 
	summarise(mean_sales = mean(weekly_sales),
      sd_sales = sd(weekly_sales))

# Look at the summary table
sales_table2
 ![image](https://user-images.githubusercontent.com/59670247/208313337-80dc9967-3e66-4bc2-b669-b08b143c8f16.png)


# Pull all sales from 2010
year2010 <- salesdata_clean %>% 
  filter(year == 2010) %>%
  pull(weekly_sales)

# Pull all sales from 2011
year2011 <- salesdata_clean %>% 
  filter(year == 2011) %>%
  pull(weekly_sales)

# Apply a t-test
•	 ![image](https://user-images.githubusercontent.com/59670247/208313350-9609f563-cf46-4edd-99ae-494b576c6680.png)

•	---OR---
•	test2 <- t.test(year2010, year2011, var.equal = TRUE)

# Look at test results
test2
 ![image](https://user-images.githubusercontent.com/59670247/208313365-e6abdc49-a3a3-4208-b841-ad9bb465382b.png)


# Create result2
result2 <- “do not reject”

# Create p2
p2 <- 0.4076
```

*Feedback:*  
The t-test returned a p-value greater than 0.05, which indicates that the null hypothesis can not be rejected. This is interpreted as there being no significant difference between the `weekly_sales` in 2010 and 2011. The output of the t-test also includes the mean values of each group, which indicate that sales were 42341.38 in 2010 and 40772.22 in 2011.

**Question 5**  
In *2010*, *excluding holidays*, did *department 1* in *store 1* have significantly higher sales than *department 1* in *store 2*?

Consider what your null hypothesis will be and use a t-test to determine if the null hypothesis can be rejected. How should your results be interpreted?

*Instructions:*  
- Filter `salesdata_clean` to create a subset containing only the necessary records to address the question above. Store this data frame in an object called `subset1`.  
- Visualize the data using `ggplot()` to create a boxplot with `store` on the x axis and `weekly_sales` on the y axis. Store this plot in an object called `figure2`.  
- Generate a summary table of `subset1` containing the mean `weekly_sales` and standard deviation of `weekly_sales` for each `store`. The columns should be named `store`, `mean_sales`, and `sd_sales`. Store this data frame in an object called `sales_table3`.  
- Create a vector called `store1` containing all of the `weekly_sales` of store 1. Hint: `pull()`  
- Create a vector called `store2` containing all of the `weekly_sales` of store 2.  
- Apply a t-test to compare these vectors. Store the output of this test in an object called `test3`.  
- Using an alpha of 0.05, if you reject the null hypothesis, store the value `"reject"` in an object called `result3`. If you do not reject it, store the value `"do not reject"` in an object called `result3`.  
- Store the p-value of the t-test in an object called `p3`.  

*Assignment Code:*  
```{r}
 ![image](https://user-images.githubusercontent.com/59670247/208313385-89057028-3600-4fdb-9e42-48ad2a2f2c28.png)

---OR---
# Filter the data (This is not correct because department 2 is not filtered out)
subset1 <- salesdata_clean %>% 
filter(year == 2010) %>% 
select(-is_holiday, -visitors, -conversions)

# Look at the subset
subset1

# Visualize the data
figure2 <- ggplot(salesdata_clean) + geom_boxplot(aes(x = store, y = weekly_sales)

# Display the figure
figure2
 ![image](https://user-images.githubusercontent.com/59670247/208313399-f10bff50-6842-4793-9d31-5333408fee8a.png)


# Create a summary table
sales_table3 <- subset1 %>% 
	group_by(store) %>% 
	summarise(mean_sales = mean(weekly_sales),
      sd_sales = sd(weekly_sales))

# Look at the summary table
sales_table3
 ![image](https://user-images.githubusercontent.com/59670247/208313417-6bec70a7-cb4b-4b15-8605-0a6aebc95d23.png)


# Pull sales from store 1
store1 <- subset1 %>% 
  filter(store == 1) %>%
  pull(weekly_sales)

# Pull sales from store 2
store2 <- subset1 %>% 
  filter(store == 2) %>%
  pull(weekly_sales)

# Apply a t-test
 ![image](https://user-images.githubusercontent.com/59670247/208313445-6af76c79-7749-42c5-8d81-883c6a3e4d58.png)

---OR---
test3 <- t.test(store1, store2, var.equal = TRUE)

# Look at the test results
test3
 ![image](https://user-images.githubusercontent.com/59670247/208313458-6af4b95b-cc4e-47bb-8a12-b5d3251d6f44.png)


# Create result3
result3 <- “reject”

# Create p3
p3 <- 0.001242
```

*Feedback:*  
Here, the null hypothesis is that there is no significant difference between the `weekly_sales` of department 1 in stores 1 and 2 during the non-holiday weeks of 2010. The t-test returned a p-value less than 0.05, which indicates that the null hypothesis can be rejected. The output of the t-test also includes the mean values of each group, which indicate that sales were higher in store 2 (23276.58) than store 1 (32561.54).
![image](https://user-images.githubusercontent.com/59670247/208313154-9af1b7f0-6ade-46d0-ab15-6ddf349231b6.png)
