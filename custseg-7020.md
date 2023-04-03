---
layout: default
---
**[Home](https://yvesmango.github.io/) >> [Projects](https://yvesmango.github.io/projects) >> [7020-Customer-Segmentation](https://yvesmango.github.io/7020-customer-segmentation/) >> R Notebook**


## Table of Contents

- [1: Loading and Cleaning](#1-loading-cleaning-displaying-data)
  - [Preprocessing](#section-a-preprocessing)
  - [Feature Engineering](#section-b-feature-engineering)
- [2: Exploratory Data Analysis](#2-exploratory-data-analysis)
  - [Summary Statistics](#part-1-summary-statistics)
- [Results](#results)
- [Conclusion](#conclusion)
- [Contributing](#contributing)
- [License](#license)


**Goal**: I will attempt to perform customer segmentation based on retail data. The goal of this project is to perform  unsupervised learning and clustering to summarize customer segment and population. More attribute information and the data context can be read here: [Kaggle source](https://www.kaggle.com/datasets/imakash3011/customer-personality-analysis)

```R
library(tidyverse)
```

    ── [1mAttaching packages[22m ─────────────────────────────────────── tidyverse 1.3.1 ──
    
    [32m✔[39m [34mggplot2[39m 3.3.3     [32m✔[39m [34mpurrr  [39m 0.3.4
    [32m✔[39m [34mtibble [39m 3.1.1     [32m✔[39m [34mdplyr  [39m 1.0.6
    [32m✔[39m [34mtidyr  [39m 1.1.3     [32m✔[39m [34mstringr[39m 1.4.0
    [32m✔[39m [34mreadr  [39m 1.4.0     [32m✔[39m [34mforcats[39m 0.5.1
    
    ── [1mConflicts[22m ────────────────────────────────────────── tidyverse_conflicts() ──
    [31m✖[39m [34mdplyr[39m::[32mfilter()[39m masks [34mstats[39m::filter()
    [31m✖[39m [34mdplyr[39m::[32mlag()[39m    masks [34mstats[39m::lag()
    


## 1. Loading, Cleaning, Displaying Data 

Explain your data set here; what is it about? What are the variables? What do you want to do with it? How are you going to clean it? etc. 



```R
my_data <- read.csv("../data/marketingcampaign.csv")
names(my_data) <- tolower(names(my_data)) #convert columns to lowercase
dim(my_data)
```


<style>
.list-inline {list-style: none; margin:0; padding: 0}
.list-inline>li {display: inline-block}
.list-inline>li:not(:last-child)::after {content: "\00b7"; padding: 0 .5ex}
</style>
<ol class="list-inline"><li>2240</li><li>29</li></ol>




```R
str(my_data)
```

    'data.frame':	2240 obs. of  29 variables:
     $ id                 : int  5524 2174 4141 6182 5324 7446 965 6177 4855 5899 ...
     $ year_birth         : int  1957 1954 1965 1984 1981 1967 1971 1985 1974 1950 ...
     $ education          : chr  "Graduation" "Graduation" "Graduation" "Graduation" ...
     $ marital_status     : chr  "Single" "Single" "Together" "Together" ...
     $ income             : int  58138 46344 71613 26646 58293 62513 55635 33454 30351 5648 ...
     $ kidhome            : int  0 1 0 1 1 0 0 1 1 1 ...
     $ teenhome           : int  0 1 0 0 0 1 1 0 0 1 ...
     $ dt_customer        : chr  "4/9/12" "8/3/14" "21-08-2013" "10/2/14" ...
     $ recency            : int  58 38 26 26 94 16 34 32 19 68 ...
     $ mntwines           : int  635 11 426 11 173 520 235 76 14 28 ...
     $ mntfruits          : int  88 1 49 4 43 42 65 10 0 0 ...
     $ mntmeatproducts    : int  546 6 127 20 118 98 164 56 24 6 ...
     $ mntfishproducts    : int  172 2 111 10 46 0 50 3 3 1 ...
     $ mntsweetproducts   : int  88 1 21 3 27 42 49 1 3 1 ...
     $ mntgoldprods       : int  88 6 42 5 15 14 27 23 2 13 ...
     $ numdealspurchases  : int  3 2 1 2 5 2 4 2 1 1 ...
     $ numwebpurchases    : int  8 1 8 2 5 6 7 4 3 1 ...
     $ numcatalogpurchases: int  10 1 2 0 3 4 3 0 0 0 ...
     $ numstorepurchases  : int  4 2 10 4 6 10 7 4 2 0 ...
     $ numwebvisitsmonth  : int  7 5 4 6 5 6 6 8 9 20 ...
     $ acceptedcmp3       : int  0 0 0 0 0 0 0 0 0 1 ...
     $ acceptedcmp4       : int  0 0 0 0 0 0 0 0 0 0 ...
     $ acceptedcmp5       : int  0 0 0 0 0 0 0 0 0 0 ...
     $ acceptedcmp1       : int  0 0 0 0 0 0 0 0 0 0 ...
     $ acceptedcmp2       : int  0 0 0 0 0 0 0 0 0 0 ...
     $ complain           : int  0 0 0 0 0 0 0 0 0 0 ...
     $ z_costcontact      : int  3 3 3 3 3 3 3 3 3 3 ...
     $ z_revenue          : int  11 11 11 11 11 11 11 11 11 11 ...
     $ response           : int  1 0 0 0 0 0 0 0 1 0 ...



```R
head(my_data)
```


<table class="dataframe">
<caption>A data.frame: 6 × 29</caption>
<thead>
	<tr><th></th><th scope=col>id</th><th scope=col>year_birth</th><th scope=col>education</th><th scope=col>marital_status</th><th scope=col>income</th><th scope=col>kidhome</th><th scope=col>teenhome</th><th scope=col>dt_customer</th><th scope=col>recency</th><th scope=col>mntwines</th><th scope=col>⋯</th><th scope=col>numwebvisitsmonth</th><th scope=col>acceptedcmp3</th><th scope=col>acceptedcmp4</th><th scope=col>acceptedcmp5</th><th scope=col>acceptedcmp1</th><th scope=col>acceptedcmp2</th><th scope=col>complain</th><th scope=col>z_costcontact</th><th scope=col>z_revenue</th><th scope=col>response</th></tr>
	<tr><th></th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>⋯</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th></tr>
</thead>
<tbody>
	<tr><th scope=row>1</th><td>5524</td><td>1957</td><td>Graduation</td><td>Single  </td><td>58138</td><td>0</td><td>0</td><td>4/9/12    </td><td>58</td><td>635</td><td>⋯</td><td>7</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>3</td><td>11</td><td>1</td></tr>
	<tr><th scope=row>2</th><td>2174</td><td>1954</td><td>Graduation</td><td>Single  </td><td>46344</td><td>1</td><td>1</td><td>8/3/14    </td><td>38</td><td> 11</td><td>⋯</td><td>5</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>3</td><td>11</td><td>0</td></tr>
	<tr><th scope=row>3</th><td>4141</td><td>1965</td><td>Graduation</td><td>Together</td><td>71613</td><td>0</td><td>0</td><td>21-08-2013</td><td>26</td><td>426</td><td>⋯</td><td>4</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>3</td><td>11</td><td>0</td></tr>
	<tr><th scope=row>4</th><td>6182</td><td>1984</td><td>Graduation</td><td>Together</td><td>26646</td><td>1</td><td>0</td><td>10/2/14   </td><td>26</td><td> 11</td><td>⋯</td><td>6</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>3</td><td>11</td><td>0</td></tr>
	<tr><th scope=row>5</th><td>5324</td><td>1981</td><td>PhD       </td><td>Married </td><td>58293</td><td>1</td><td>0</td><td>19-01-2014</td><td>94</td><td>173</td><td>⋯</td><td>5</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>3</td><td>11</td><td>0</td></tr>
	<tr><th scope=row>6</th><td>7446</td><td>1967</td><td>Master    </td><td>Together</td><td>62513</td><td>0</td><td>1</td><td>9/9/13    </td><td>16</td><td>520</td><td>⋯</td><td>6</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>3</td><td>11</td><td>0</td></tr>
</tbody>
</table>




```R
num_vars <- sum(sapply(my_data, is.numeric))
non_num_vars <- sum(sapply(my_data, function(x) !is.numeric(x)))

# Print the results
cat("There are", num_vars, "numeric variables and", non_num_vars, "non-numeric variables in the dataset.")                           
```

    There are 26 numeric variables and 3 non-numeric variables in the dataset.

I would say this is a pretty good dataset to perform correlations since it is numeric-heavy. I will move on next to A) preprocess the data by removing any null values detected and B) engineer new features entirely and drop any irrelevant features.

### Section A. Preprocessing


```R
# Get count of null values in each column, arrange by count
sort(colSums(is.na(my_data)), decreasing = TRUE)
```


<style>
.dl-inline {width: auto; margin:0; padding: 0}
.dl-inline>dt, .dl-inline>dd {float: none; width: auto; display: inline-block}
.dl-inline>dt::after {content: ":\0020"; padding-right: .5ex}
.dl-inline>dt:not(:first-of-type) {padding-left: .5ex}
</style>

<dl class="dl-inline"><dt>income</dt><dd>24</dd><dt>id</dt><dd>0</dd><dt>year_birth</dt><dd>0</dd><dt>education</dt><dd>0</dd><dt>marital_status</dt><dd>0</dd><dt>kidhome</dt><dd>0</dd><dt>teenhome</dt><dd>0</dd><dt>dt_customer</dt><dd>0</dd><dt>recency</dt><dd>0</dd><dt>mntwines</dt><dd>0</dd><dt>mntfruits</dt><dd>0</dd><dt>mntmeatproducts</dt><dd>0</dd><dt>mntfishproducts</dt><dd>0</dd><dt>mntsweetproducts</dt><dd>0</dd><dt>mntgoldprods</dt><dd>0</dd><dt>numdealspurchases</dt><dd>0</dd><dt>numwebpurchases</dt><dd>0</dd><dt>numcatalogpurchases</dt><dd>0</dd><dt>numstorepurchases</dt><dd>0</dd><dt>numwebvisitsmonth</dt><dd>0</dd><dt>acceptedcmp3</dt><dd>0</dd><dt>acceptedcmp4</dt><dd>0</dd><dt>acceptedcmp5</dt><dd>0</dd><dt>acceptedcmp1</dt><dd>0</dd><dt>acceptedcmp2</dt><dd>0</dd><dt>complain</dt><dd>0</dd><dt>z_costcontact</dt><dd>0</dd><dt>z_revenue</dt><dd>0</dd><dt>response</dt><dd>0</dd></dl>



"Income" column has 24 null values. I need to decide what to do with those null values. For starters, do I keep or remove them? If keep, how do I want to fill the nulls, use average, etc?

Since only 24 rows are affected by null values in our dataset of 2240 observations. I'm simply going to drop these from our dataframe. 


```R
ante <- nrow(my_data)

my_data <- my_data %>% na.omit()

poste <- nrow(my_data)

cat("Number of rows before null removal: ", ante, "\n")
cat("Number of rows after null removal: ", poste, "\n")
```

    Number of rows before null removal:  2240 
    Number of rows after null removal:  2216 


Let's look at the categorical variables next in our data and _see if we can revise them further into simplified segments._

**Why segment categorical variables?**

In this case, segmenting education levels into three groups can help to simplify and standardize the analysis of the data. By reducing the number of categories in the variable, it may be easier to identify patterns or trends in the data, especially if there are a large number of distinct values in the original column. Additionally, segmenting the values of a given categorical column into levels make it easier to perform statistical tests or create visualizations that require discrete categories. 

### Section B. Feature Engineering


```R
my_data %>% select_if(is.character) %>% head
```


<table class="dataframe">
<caption>A data.frame: 6 × 3</caption>
<thead>
	<tr><th></th><th scope=col>education</th><th scope=col>marital_status</th><th scope=col>dt_customer</th></tr>
	<tr><th></th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th></tr>
</thead>
<tbody>
	<tr><th scope=row>1</th><td>Graduation</td><td>Single  </td><td>4/9/12    </td></tr>
	<tr><th scope=row>2</th><td>Graduation</td><td>Single  </td><td>8/3/14    </td></tr>
	<tr><th scope=row>3</th><td>Graduation</td><td>Together</td><td>21-08-2013</td></tr>
	<tr><th scope=row>4</th><td>Graduation</td><td>Together</td><td>10/2/14   </td></tr>
	<tr><th scope=row>5</th><td>PhD       </td><td>Married </td><td>19-01-2014</td></tr>
	<tr><th scope=row>6</th><td>Master    </td><td>Together</td><td>9/9/13    </td></tr>
</tbody>
</table>




```R
categ_data <- my_data %>% select_if(is.character) %>% select(-dt_customer)

for (col in names(categ_data)) {
    table <- categ_data %>% group_by(!!sym(col)) %>% summarize(count = n())
    print(table)
}
```

    [90m# A tibble: 5 x 2[39m
      education  count
      [3m[90m<chr>[39m[23m      [3m[90m<int>[39m[23m
    [90m1[39m 2n Cycle     200
    [90m2[39m Basic         54
    [90m3[39m Graduation  [4m1[24m116
    [90m4[39m Master       365
    [90m5[39m PhD          481
    [90m# A tibble: 8 x 2[39m
      marital_status count
      [3m[90m<chr>[39m[23m          [3m[90m<int>[39m[23m
    [90m1[39m Absurd             2
    [90m2[39m Alone              3
    [90m3[39m Divorced         232
    [90m4[39m Married          857
    [90m5[39m Single           471
    [90m6[39m Together         573
    [90m7[39m Widow             76
    [90m8[39m YOLO               2


The lack of discreteness in the values is unnecessarily messy, however I can further segment columns "education" and "marital_status" into simpler bins. I will use `case_when()` for mapping certain existing values to new values in the education column. For "marital_status", I wil use `ifelse()` function to categorize the valules based on a specified condition. (Data collectors: when surveying for categorical variables in your data, always make sure that data validation is required by allowing only certain values from a drop-down list).


```R
#Segmenting education levels into three bins
my_data <- my_data %>% mutate(education = case_when(
    education == "Basic" ~ "Undergraduate",
    education == "2n Cycle" ~ "Undergraduate",
    education == "Graduation" ~ "Graduate",
    education == "Master" ~ "Postgraduate",
    education == "PhD" ~ "Postgraduate",
    TRUE ~ education

))

my_data %>% count(education)

#Segmenting marital status into two bins
my_data$living_with <- ifelse(my_data$marital_status %in% c("Single", "Divorced", "Widow", "Alone", "Absurd", "YOLO"), "Alone", "Partnered")

 my_data %>% count(living_with) %>% arrange(desc(n))
```


<table class="dataframe">
<caption>A data.frame: 3 × 2</caption>
<thead>
	<tr><th scope=col>education</th><th scope=col>n</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;int&gt;</th></tr>
</thead>
<tbody>
	<tr><td>Graduate     </td><td>1116</td></tr>
	<tr><td>Postgraduate </td><td> 846</td></tr>
	<tr><td>Undergraduate</td><td> 254</td></tr>
</tbody>
</table>




<table class="dataframe">
<caption>A data.frame: 2 × 2</caption>
<thead>
	<tr><th scope=col>living_with</th><th scope=col>n</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;int&gt;</th></tr>
</thead>
<tbody>
	<tr><td>Partnered</td><td>1430</td></tr>
	<tr><td>Alone    </td><td> 786</td></tr>
</tbody>
</table>



**In the next part, I will be performing the following steps to engineer some new features, in a similar fashion to what we did above:**

* Extract the "age" of a customer by the "year_birth" indicating the birth year of each person.
* Create another feature "tspent" indicating the _total_ amount spent by the customer in various categories (mntwines, mntfruits, mntfish, etc) over the span of two years.
* Create a feature "dependents" to indicate total children in a household, simply adding "kidhome" + "teenhome".
* Creating feature indicating "household_size" to gain insight on house hold size
* Create a feature "is_parent" to indicate parenthood.
* Drop some of the redundant features.




```R
library(lubridate)

# Convert year_birth to date format
my_data$date_birth <- ymd(paste(my_data$year_birth, "-01-01", sep = ""))

# Calculate age
my_data$age <- interval(my_data$date_birth, Sys.Date()) %/% years(1)

# Create tspent feature
my_data <- my_data %>%
  mutate(tspent = mntwines + mntfruits + mntmeatproducts + mntfishproducts + mntsweetproducts + mntgoldprods)


# Create dependents feature
my_data <- my_data %>%
  mutate(dependents = kidhome + teenhome)


# Create household_size feature

my_data <- my_data %>%
  mutate(living_with_num = ifelse(living_with == "Alone", 1, 2), # recode "Alone" to 1 and "Partnered" to 2
         household_size = dependents + living_with_num) # add the two columns to create "household_size"

# Create is_parent feature
my_data <- my_data %>%
  mutate(is_parent = if_else(dependents > 0, 1, 0))

# Rename some columns for readability
names(my_data) <- sub("mntwines", "wines", names(my_data))
names(my_data) <- sub("mntfruits", "fruits", names(my_data))
names(my_data) <- sub("mntmeatproducts", "meat", names(my_data))
names(my_data) <- sub("mntfishproducts", "fish", names(my_data))
names(my_data) <- sub("mntsweetproducts", "sweets", names(my_data))
names(my_data) <- sub("mntgoldprods", "gold", names(my_data))


# Drop redundant features
exclude <- c("marital_status", "dt_customer", "z_costcontact", "z_revenue", "date_birth", "id", "living_with_num")
my_data <- my_data[, !(names(my_data) %in% exclude)]
```

    
    Attaching package: ‘lubridate’
    
    
    The following objects are masked from ‘package:base’:
    
        date, intersect, setdiff, union
    
    



```R
str(my_data)
```

    'data.frame':	2216 obs. of  30 variables:
     $ year_birth         : int  1957 1954 1965 1984 1981 1967 1971 1985 1974 1950 ...
     $ education          : chr  "Graduate" "Graduate" "Graduate" "Graduate" ...
     $ income             : int  58138 46344 71613 26646 58293 62513 55635 33454 30351 5648 ...
     $ kidhome            : int  0 1 0 1 1 0 0 1 1 1 ...
     $ teenhome           : int  0 1 0 0 0 1 1 0 0 1 ...
     $ recency            : int  58 38 26 26 94 16 34 32 19 68 ...
     $ wines              : int  635 11 426 11 173 520 235 76 14 28 ...
     $ fruits             : int  88 1 49 4 43 42 65 10 0 0 ...
     $ meat               : int  546 6 127 20 118 98 164 56 24 6 ...
     $ fish               : int  172 2 111 10 46 0 50 3 3 1 ...
     $ sweets             : int  88 1 21 3 27 42 49 1 3 1 ...
     $ gold               : int  88 6 42 5 15 14 27 23 2 13 ...
     $ numdealspurchases  : int  3 2 1 2 5 2 4 2 1 1 ...
     $ numwebpurchases    : int  8 1 8 2 5 6 7 4 3 1 ...
     $ numcatalogpurchases: int  10 1 2 0 3 4 3 0 0 0 ...
     $ numstorepurchases  : int  4 2 10 4 6 10 7 4 2 0 ...
     $ numwebvisitsmonth  : int  7 5 4 6 5 6 6 8 9 20 ...
     $ acceptedcmp3       : int  0 0 0 0 0 0 0 0 0 1 ...
     $ acceptedcmp4       : int  0 0 0 0 0 0 0 0 0 0 ...
     $ acceptedcmp5       : int  0 0 0 0 0 0 0 0 0 0 ...
     $ acceptedcmp1       : int  0 0 0 0 0 0 0 0 0 0 ...
     $ acceptedcmp2       : int  0 0 0 0 0 0 0 0 0 0 ...
     $ complain           : int  0 0 0 0 0 0 0 0 0 0 ...
     $ response           : int  1 0 0 0 0 0 0 0 1 0 ...
     $ living_with        : chr  "Alone" "Alone" "Partnered" "Partnered" ...
     $ age                : num  66 69 58 39 42 56 52 38 49 73 ...
     $ tspent             : int  1617 27 776 53 422 716 590 169 46 49 ...
     $ dependents         : int  0 2 0 1 1 1 1 1 1 2 ...
     $ household_size     : num  1 3 2 3 3 3 2 3 3 4 ...
     $ is_parent          : num  0 1 0 1 1 1 1 1 1 1 ...


Lastly, there are only 2 categorical variables ("living_with" and "education") left in our dataframe. I'm going to convert them to factor for more accurate statistical analysis visualization capabilities as opposed to character types.


```R
#convert to factor type
my_data$education <- factor(my_data$education, levels = c("Undergraduate", "Graduate", "Postgraduate"))
my_data$living_with <- factor(my_data$living_with, levels = c("Alone", "Partnered"))
```


```R
num_vars <- sum(sapply(my_data, is.numeric))
non_num_vars <- sum(sapply(my_data, function(x) !is.numeric(x)))

# Print the results
cat("There are", num_vars, "numeric variables and", non_num_vars, "non-numeric variables in the dataset.")                           
```

    There are 28 numeric variables and 2 non-numeric variables in the dataset.

As I begin the next stage in my data science project life cycle, I will prepackage my variables according to their data types for easier exploration and visualization.


```R
num_data <- my_data %>% 
  select_if(is.numeric)

cat_data <- my_data %>% 
  select_if(is.factor)
```

## 2. Exploratory Data Analysis 

### Section A. Descriptive Statistics

#### Part 1. Summary Statistics


```R
summary(my_data)
```


       year_birth           education        income          kidhome      
     Min.   :1893   Undergraduate: 254   Min.   :  1730   Min.   :0.0000  
     1st Qu.:1959   Graduate     :1116   1st Qu.: 35303   1st Qu.:0.0000  
     Median :1970   Postgraduate : 846   Median : 51382   Median :0.0000  
     Mean   :1969                        Mean   : 52247   Mean   :0.4418  
     3rd Qu.:1977                        3rd Qu.: 68522   3rd Qu.:1.0000  
     Max.   :1996                        Max.   :666666   Max.   :2.0000  
        teenhome         recency          wines            fruits      
     Min.   :0.0000   Min.   : 0.00   Min.   :   0.0   Min.   :  0.00  
     1st Qu.:0.0000   1st Qu.:24.00   1st Qu.:  24.0   1st Qu.:  2.00  
     Median :0.0000   Median :49.00   Median : 174.5   Median :  8.00  
     Mean   :0.5054   Mean   :49.01   Mean   : 305.1   Mean   : 26.36  
     3rd Qu.:1.0000   3rd Qu.:74.00   3rd Qu.: 505.0   3rd Qu.: 33.00  
     Max.   :2.0000   Max.   :99.00   Max.   :1493.0   Max.   :199.00  
          meat             fish            sweets            gold       
     Min.   :   0.0   Min.   :  0.00   Min.   :  0.00   Min.   :  0.00  
     1st Qu.:  16.0   1st Qu.:  3.00   1st Qu.:  1.00   1st Qu.:  9.00  
     Median :  68.0   Median : 12.00   Median :  8.00   Median : 24.50  
     Mean   : 167.0   Mean   : 37.64   Mean   : 27.03   Mean   : 43.97  
     3rd Qu.: 232.2   3rd Qu.: 50.00   3rd Qu.: 33.00   3rd Qu.: 56.00  
     Max.   :1725.0   Max.   :259.00   Max.   :262.00   Max.   :321.00  
     numdealspurchases numwebpurchases  numcatalogpurchases numstorepurchases
     Min.   : 0.000    Min.   : 0.000   Min.   : 0.000      Min.   : 0.000   
     1st Qu.: 1.000    1st Qu.: 2.000   1st Qu.: 0.000      1st Qu.: 3.000   
     Median : 2.000    Median : 4.000   Median : 2.000      Median : 5.000   
     Mean   : 2.324    Mean   : 4.085   Mean   : 2.671      Mean   : 5.801   
     3rd Qu.: 3.000    3rd Qu.: 6.000   3rd Qu.: 4.000      3rd Qu.: 8.000   
     Max.   :15.000    Max.   :27.000   Max.   :28.000      Max.   :13.000   
     numwebvisitsmonth  acceptedcmp3      acceptedcmp4      acceptedcmp5   
     Min.   : 0.000    Min.   :0.00000   Min.   :0.00000   Min.   :0.0000  
     1st Qu.: 3.000    1st Qu.:0.00000   1st Qu.:0.00000   1st Qu.:0.0000  
     Median : 6.000    Median :0.00000   Median :0.00000   Median :0.0000  
     Mean   : 5.319    Mean   :0.07356   Mean   :0.07401   Mean   :0.0731  
     3rd Qu.: 7.000    3rd Qu.:0.00000   3rd Qu.:0.00000   3rd Qu.:0.0000  
     Max.   :20.000    Max.   :1.00000   Max.   :1.00000   Max.   :1.0000  
      acceptedcmp1      acceptedcmp2        complain           response     
     Min.   :0.00000   Min.   :0.00000   Min.   :0.000000   Min.   :0.0000  
     1st Qu.:0.00000   1st Qu.:0.00000   1st Qu.:0.000000   1st Qu.:0.0000  
     Median :0.00000   Median :0.00000   Median :0.000000   Median :0.0000  
     Mean   :0.06408   Mean   :0.01354   Mean   :0.009477   Mean   :0.1503  
     3rd Qu.:0.00000   3rd Qu.:0.00000   3rd Qu.:0.000000   3rd Qu.:0.0000  
     Max.   :1.00000   Max.   :1.00000   Max.   :1.000000   Max.   :1.0000  
        living_with        age             tspent         dependents    
     Alone    : 786   Min.   : 27.00   Min.   :   5.0   Min.   :0.0000  
     Partnered:1430   1st Qu.: 46.00   1st Qu.:  69.0   1st Qu.:0.0000  
                      Median : 53.00   Median : 396.5   Median :1.0000  
                      Mean   : 54.18   Mean   : 607.1   Mean   :0.9472  
                      3rd Qu.: 64.00   3rd Qu.:1048.0   3rd Qu.:1.0000  
                      Max.   :130.00   Max.   :2525.0   Max.   :3.0000  
     household_size    is_parent     
     Min.   :1.000   Min.   :0.0000  
     1st Qu.:2.000   1st Qu.:0.0000  
     Median :3.000   Median :1.0000  
     Mean   :2.593   Mean   :0.7144  
     3rd Qu.:3.000   3rd Qu.:1.0000  
     Max.   :5.000   Max.   :1.0000  


#### Part 2. Histograms / Distributions plot


```R
library(purrr)
options(repr.plot.width=14, repr.plot.height=8)

# Identify the numeric variables
num_vars <- my_data %>%
  select_if(is.numeric) %>%
  names()

# Create the facet grid of density plots
my_data %>%
  select(all_of(num_vars)) %>%
  gather(variable, value) %>%
  ggplot(aes(x = value)) +
  geom_density() +
  facet_wrap(~ variable, scales = "free")
```


![Imgur](https://i.imgur.com/NSXjkPG.png)


#### Part 3. Correlations plots and boxplots

    library(GGally)
    ggpairs(num_data)

For this code block, I called the ggpairs() function to generate pairs plot (scatter plot matrix) among the variables in my dataset. What results is a computationally-intensive visualization that I will forego, for the simple reason that said pairs plot is in no way meaningful to our analysis due to its volume.

In plain terms, the (would-be) pairsplot is simply too cumbersome and large to detect any meaningful relation or insight from. I will try a correlation matrix heatmap and see if that is any better...


```R
# Get upper triangle of the correlation matrix
  get_upper_tri <- function(cormat){
    cormat[lower.tri(cormat)]<- NA
    return(cormat)
  }

upper_tri <- get_upper_tri(cor(num_data))
```


```R
options(repr.plot.width=16, repr.plot.height=16)

# Melt the correlation matrix
library(reshape2)
melted_cormat <- melt(upper_tri, na.rm = TRUE)

# Generate correlation matrix heatmap
ggplot(data = melted_cormat, aes(Var2, Var1, fill = value))+
 geom_tile(color = "white")+
 scale_fill_gradient2(low = "blue", high = "red", mid = "white", 
   midpoint = 0, limit = c(-1,1), space = "Lab", 
   name="Pearson\nCorrelation") +
  theme_minimal()+ 
 theme(axis.text.x = element_text(angle = 45, vjust = 1, 
    size = 13, hjust = 1), axis.text.y = element_text(size = 13))+
 coord_fixed() +
 geom_text(aes(label = round(value, 2)), size = 4, color = "black")
```

    
    Attaching package: ‘reshape2’
    
    
    The following object is masked from ‘package:tidyr’:
    
        smiths
    
    



![Imgur](https://i.imgur.com/nQ82lmr.png)

**This is simply too many variables to account for**

The correlation matrix heatmap is slightly better, but while it does provide insights into highly and inversely correlated variables, I'm still running into the volume problem.

I am going to employ PCA (Principal Component Analysis) to reduce the dimensions of the data. This is to help me simplify the dataset to a dimension that selects only variables with the most variance and relationships to another while keeping visualizations manageable.

Before employing PCA, I need to convert the categorical variables (education and living_with) to numeric using one-hot encoding. `dummy_cols` function from the _fastDummies_ library allows me to do just that. This process allows me to use these categorical variables as features in machine learning models and most importantly PCA.

I will store the newly-encoded features in a new dataframe "encoded_data", where I will also remove the original categorical features so that I just have numerical variables.

Another careful consideration before deploying PCA are the outliers in the data. Though it is also important to consider the reason for the presence of outliers and whether they are genuine obserations or errors. Based on the following boxplots, the **age** column appears to have values over 12, and a sole outlier in the **income** variable. I think it is safe to assume that these outliers are indeed errors and are not genuine observations (at least for the age outliers).


```R
options(repr.plot.width=16, repr.plot.height=10)


par(mfrow = c(2,4))
par(mar=c(2,2,2,2))
invisible(lapply(1:ncol(num_data), function(i) boxplot(num_data[, i], main=colnames(num_data)[i], 
                                                     ylab=colnames(num_data)[i])))

```


![Imgur](https://i.imgur.com/YBdkeDS.png)


![Imgur](https://i.imgur.com/c3SIIgg.png)



![Imgur](https://i.imgur.com/5Y34s0M.png)



![Imgur](https://i.imgur.com/AhDfd1s.png)



```R
## filtering the outliers from age and income column
my_data <- my_data %>% filter(age < 100, income < 150000, year_birth > 1920)
```


```R
install.packages("fastDummies")
library(fastDummies)

# Convert categorical variables into numerical variables using one-hot encoding
encoded_data <- dummy_cols(my_data, select_columns = c("education", "living_with"), remove_selected_columns = TRUE)

# lowercase new columns, which have been encoded
names(encoded_data) <- tolower(names(encoded_data))
```

    Updating HTML index of packages in '.Library'
    
    Making 'packages.html' ...
     done
    



```R
str(encoded_data)
```

    'data.frame':	2205 obs. of  33 variables:
     $ year_birth             : int  1957 1954 1965 1984 1981 1967 1971 1985 1974 1950 ...
     $ income                 : int  58138 46344 71613 26646 58293 62513 55635 33454 30351 5648 ...
     $ kidhome                : int  0 1 0 1 1 0 0 1 1 1 ...
     $ teenhome               : int  0 1 0 0 0 1 1 0 0 1 ...
     $ recency                : int  58 38 26 26 94 16 34 32 19 68 ...
     $ wines                  : int  635 11 426 11 173 520 235 76 14 28 ...
     $ fruits                 : int  88 1 49 4 43 42 65 10 0 0 ...
     $ meat                   : int  546 6 127 20 118 98 164 56 24 6 ...
     $ fish                   : int  172 2 111 10 46 0 50 3 3 1 ...
     $ sweets                 : int  88 1 21 3 27 42 49 1 3 1 ...
     $ gold                   : int  88 6 42 5 15 14 27 23 2 13 ...
     $ numdealspurchases      : int  3 2 1 2 5 2 4 2 1 1 ...
     $ numwebpurchases        : int  8 1 8 2 5 6 7 4 3 1 ...
     $ numcatalogpurchases    : int  10 1 2 0 3 4 3 0 0 0 ...
     $ numstorepurchases      : int  4 2 10 4 6 10 7 4 2 0 ...
     $ numwebvisitsmonth      : int  7 5 4 6 5 6 6 8 9 20 ...
     $ acceptedcmp3           : int  0 0 0 0 0 0 0 0 0 1 ...
     $ acceptedcmp4           : int  0 0 0 0 0 0 0 0 0 0 ...
     $ acceptedcmp5           : int  0 0 0 0 0 0 0 0 0 0 ...
     $ acceptedcmp1           : int  0 0 0 0 0 0 0 0 0 0 ...
     $ acceptedcmp2           : int  0 0 0 0 0 0 0 0 0 0 ...
     $ complain               : int  0 0 0 0 0 0 0 0 0 0 ...
     $ response               : int  1 0 0 0 0 0 0 0 1 0 ...
     $ age                    : num  66 69 58 39 42 56 52 38 49 73 ...
     $ tspent                 : int  1617 27 776 53 422 716 590 169 46 49 ...
     $ dependents             : int  0 2 0 1 1 1 1 1 1 2 ...
     $ household_size         : num  1 3 2 3 3 3 2 3 3 4 ...
     $ is_parent              : num  0 1 0 1 1 1 1 1 1 1 ...
     $ education_undergraduate: int  0 0 0 0 0 0 0 0 0 0 ...
     $ education_graduate     : int  1 1 1 1 0 0 1 0 0 0 ...
     $ education_postgraduate : int  0 0 0 0 1 1 0 1 1 1 ...
     $ living_with_alone      : int  1 1 0 0 0 0 1 0 0 0 ...
     $ living_with_partnered  : int  0 0 1 1 1 1 0 1 1 1 ...


Now the stage is set to employ PCA.

### Section B. Principal Component Analysis


```R
#scale data
scaled_encoded <- scale(encoded_data)

#PCA
pca_result <- prcomp(scaled_encoded)
```


```R
summary(pca_result)
```


    Importance of components:
                              PC1    PC2     PC3     PC4     PC5     PC6     PC7
    Standard deviation     2.9871 1.8235 1.51779 1.42073 1.39579 1.25566 1.12552
    Proportion of Variance 0.2704 0.1008 0.06981 0.06117 0.05904 0.04778 0.03839
    Cumulative Proportion  0.2704 0.3711 0.44096 0.50212 0.56116 0.60894 0.64733
                               PC8     PC9    PC10    PC11    PC12    PC13    PC14
    Standard deviation     1.08725 1.04687 1.00433 0.99746 0.93483 0.86835 0.79858
    Proportion of Variance 0.03582 0.03321 0.03057 0.03015 0.02648 0.02285 0.01933
    Cumulative Proportion  0.68315 0.71636 0.74692 0.77707 0.80356 0.82640 0.84573
                              PC15    PC16    PC17    PC18    PC19    PC20    PC21
    Standard deviation     0.78072 0.75936 0.74098 0.73421 0.67504 0.65138 0.64311
    Proportion of Variance 0.01847 0.01747 0.01664 0.01634 0.01381 0.01286 0.01253
    Cumulative Proportion  0.86420 0.88167 0.89831 0.91465 0.92846 0.94131 0.95385
                              PC22    PC23    PC24    PC25    PC26    PC27
    Standard deviation     0.60392 0.57685 0.52460 0.45855 0.44931 0.37180
    Proportion of Variance 0.01105 0.01008 0.00834 0.00637 0.00612 0.00419
    Cumulative Proportion  0.96490 0.97498 0.98332 0.98969 0.99581 1.00000
                                PC28      PC29      PC30      PC31      PC32
    Standard deviation     6.136e-16 5.416e-16 4.723e-16 2.965e-16 2.109e-16
    Proportion of Variance 0.000e+00 0.000e+00 0.000e+00 0.000e+00 0.000e+00
    Cumulative Proportion  1.000e+00 1.000e+00 1.000e+00 1.000e+00 1.000e+00
                                PC33
    Standard deviation     1.765e-16
    Proportion of Variance 0.000e+00
    Cumulative Proportion  1.000e+00



```R
plot(pca_result, type = "l")
```


![png](output_46_0.png)


From the results of our PCA, I can identify that the first two principal components explain 36.62% of the variance, which means they both capture a strong underlying pattern in the data (total variability). Additionally, the scree plot confirms the sufficient number of PCs to use via the elbow method.

To identify the most important features based on the PCA, we can look at the absolute values of the loadings of each variable on PC1 and PC2, and select the variables with the highest absolute loadings. These variables are the ones that contribute the most to the variability captured by PC1 and PC2, and can be used as input variables for the next stage of my analysis: clustering.
I will set a threshold level for each PC to narrow down further the list of features. 33 is too many to use for cluster analysis.


```R
#selecting only the most important features from PC1 in the form of a list
pc1_features <- pca_result$rotation[, 1] %>%
  abs() %>%
  sort(decreasing = TRUE) %>%
  head(9) %>% names()

#table view
pca_result$rotation[, 1] %>% 
    abs() %>% 
    sort(decreasing = TRUE) %>% 
    as.data.frame() %>% 
    filter(. >= 0.23) %>% 
    rownames_to_column(var = "feature")
```


<table class="dataframe">
<caption>A data.frame: 10 × 2</caption>
<thead>
	<tr><th scope=col>feature</th><th scope=col>.</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>tspent             </td><td>0.3112970</td></tr>
	<tr><td>income             </td><td>0.2794197</td></tr>
	<tr><td>meat               </td><td>0.2781833</td></tr>
	<tr><td>numcatalogpurchases</td><td>0.2739654</td></tr>
	<tr><td>wines              </td><td>0.2558732</td></tr>
	<tr><td>fish               </td><td>0.2344989</td></tr>
	<tr><td>dependents         </td><td>0.2339325</td></tr>
	<tr><td>is_parent          </td><td>0.2324959</td></tr>
	<tr><td>kidhome            </td><td>0.2321033</td></tr>
	<tr><td>numstorepurchases  </td><td>0.2309661</td></tr>
</tbody>
</table>




```R
#selecting only the most important features from PC2 in the form of a list
pc2_features <- pca_result$rotation[, 2] %>%
  abs() %>%
  sort(decreasing = TRUE) %>%
  head(8) %>% names()

#table view
pca_result$rotation[, 2] %>% 
    abs() %>% 
    sort(decreasing = TRUE) %>% 
    as.data.frame() %>% 
    filter(. >= 0.23) %>% 
    rownames_to_column(var = "feature")
```


<table class="dataframe">
<caption>A data.frame: 8 × 2</caption>
<thead>
	<tr><th scope=col>feature</th><th scope=col>.</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>teenhome         </td><td>0.4207707</td></tr>
	<tr><td>age              </td><td>0.3320624</td></tr>
	<tr><td>year_birth       </td><td>0.3320624</td></tr>
	<tr><td>household_size   </td><td>0.3256689</td></tr>
	<tr><td>dependents       </td><td>0.2837317</td></tr>
	<tr><td>numdealspurchases</td><td>0.2713271</td></tr>
	<tr><td>numwebpurchases  </td><td>0.2384993</td></tr>
	<tr><td>is_parent        </td><td>0.2332432</td></tr>
</tbody>
</table>



I arrive to the momentous stage of feature selection where I subset my original (scaled) dataframe to features only in either PC1 or PC2. The resulting subset will be used for cluster analysis.


```R
scaled_encoded_small <- scaled_encoded[, unique(c(pc1_features, pc2_features))]
```


```R
head(scaled_encoded_small)
```


<table class="dataframe">
<caption>A matrix: 6 × 15 of type dbl</caption>
<thead>
	<tr><th scope=col>tspent</th><th scope=col>income</th><th scope=col>meat</th><th scope=col>numcatalogpurchases</th><th scope=col>wines</th><th scope=col>fish</th><th scope=col>dependents</th><th scope=col>is_parent</th><th scope=col>kidhome</th><th scope=col>teenhome</th><th scope=col>age</th><th scope=col>year_birth</th><th scope=col>household_size</th><th scope=col>numdealspurchases</th><th scope=col>numwebpurchases</th></tr>
</thead>
<tbody>
	<tr><td> 1.6789425</td><td> 0.3145795</td><td> 1.7480030</td><td> 2.6279300</td><td> 0.9743448</td><td> 2.4485988</td><td>-1.26630212</td><td>-1.5843004</td><td>-0.8232184</td><td>-0.9305557</td><td> 1.0169580</td><td>-1.0169580</td><td>-1.7586132</td><td> 0.3613967</td><td> 1.4244487</td></tr>
	<tr><td>-0.9636789</td><td>-0.2548196</td><td>-0.7315122</td><td>-0.5879096</td><td>-0.8745778</td><td>-0.6521970</td><td> 1.40310149</td><td> 0.6309072</td><td> 1.0385217</td><td> 0.9063962</td><td> 1.2732412</td><td>-1.2732412</td><td> 0.4484113</td><td>-0.1687960</td><td>-1.1327002</td></tr>
	<tr><td> 0.2811786</td><td> 0.9651351</td><td>-0.1759171</td><td>-0.2305941</td><td> 0.3550743</td><td> 1.3359603</td><td>-1.26630212</td><td>-1.5843004</td><td>-0.8232184</td><td>-0.9305557</td><td> 0.3335362</td><td>-0.3335362</td><td>-0.6551009</td><td>-0.6989887</td><td> 1.4244487</td></tr>
	<tr><td>-0.9204662</td><td>-1.2058136</td><td>-0.6672284</td><td>-0.9452251</td><td>-0.8745778</td><td>-0.5062772</td><td> 0.06839968</td><td> 0.6309072</td><td> 1.0385217</td><td>-0.9305557</td><td>-1.2895907</td><td> 1.2895907</td><td> 0.4484113</td><td>-0.1687960</td><td>-0.7673932</td></tr>
	<tr><td>-0.3071786</td><td> 0.3220627</td><td>-0.2172424</td><td> 0.1267214</td><td>-0.3945691</td><td> 0.1503619</td><td> 0.06839968</td><td> 0.6309072</td><td> 1.0385217</td><td>-0.9305557</td><td>-1.0333075</td><td> 1.0333075</td><td> 0.4484113</td><td> 1.4217821</td><td> 0.3285278</td></tr>
	<tr><td> 0.1814571</td><td> 0.5257989</td><td>-0.3090762</td><td> 0.4840369</td><td> 0.6335979</td><td>-0.6886770</td><td> 0.06839968</td><td> 0.6309072</td><td>-0.8232184</td><td> 0.9063962</td><td> 0.1626807</td><td>-0.1626807</td><td> 0.4484113</td><td>-0.1687960</td><td> 0.6938348</td></tr>
</tbody>
</table>



I have finally selected my most important features using PCA. I went from 33 features to 15, a little less than half. I have named this new dataframe "scaled_encoded_small". I will now perform cluster analysis and modelling to which we will compare clusters to gain insights and use hypothesis testing to test differences among the features.

## 3. Cluster Modelling

Recall that this is an unsupervised learning approach. We do not have a tagged feature or label to evaluate or score our model. The purpose of this section is to study the patterns in the clusters formed and determine the nature of the patterns detected in the clusters.

To determine the ideal number of clusters to use, I employ the _silhouette method_ using `fviz_nbclust()` from the "factoextra" package.


```R
library(factoextra)
library(cluster)
library(fpc)
```

    Welcome! Want to learn more? See two factoextra-related books at https://goo.gl/ve3WBa
    



```R
# Determine optimal number of clusters using silhouette method
fviz_nbclust(scaled_encoded, kmeans, method = "silhouette")
```


```R
# Select the number of clusters based on findings of silhouette method
k <- 2

# Run the k-means algorithm
kclust <- kmeans(scaled_encoded_small, centers = k, nstart = 20)

# View the cluster centers
kclust$centers

# View the cluster assignments
kclust$cluster

# Plot the clusters
clusplot(scaled_encoded_small, kclust$cluster, color = TRUE, shade = TRUE, labels = 2, lines = 0)

```


```R
#save clusterplot

# png("clusplot.png", width = 30, height = 20, units = "cm", res = 150)

# # Plot the clusters
# clusplot(scaled_encoded_small, kclust$cluster, color = TRUE, shade = TRUE, labels = FALSE, lines = 0, main = "cluster plot (k-means)")

# dev.off()
```


```R
table(kclust$cluster)
#this table indicates that there are 793 observations in cluster 1 and 1419 observations in cluster 2.
```

Append the newly generated cluster designations to our original dataframe (another copy for the encoded version of df)


```R
clustered_data <- cbind(encoded_data, cluster = kclust$cluster)
clustered_ogdata <- cbind(my_data, cluster = kclust$cluster)
```


```R
clustered_data <- as.data.frame(clustered_data)
clustered_ogdata <- as.data.frame(clustered_ogdata)
```


```R
# Compute the correlation matrix for each cluster
library(corrplot)

#Important to exclude the cluster variable from the correlation matrix
cor_mat_cluster1 <- cor(clustered_data[clustered_data$cluster == 1, -which(names(clustered_data) == "cluster")])
cor_mat_cluster2 <- cor(clustered_data[clustered_data$cluster == 2, -which(names(clustered_data) == "cluster")])

# Remove upper triangle from each matrix
cor_mat_cluster1[upper.tri(cor_mat_cluster1)] <- NA
cor_mat_cluster2[upper.tri(cor_mat_cluster2)] <- NA

# Plot the correlation matrix for each cluster
corrplot(cor_mat_cluster1, method = "color")
corrplot(cor_mat_cluster2, method = "color")

```

Based on the correlation matrix for each cluster, I can see that the correlations between 1 and 2 that stick out are the spending habbits. Specifically, correlation matrix 1, which represents cluster 1, 


```R
#Examine the characteristic of each cluster
```

## 4. Hypothesis testing


```R
table(clustered_ogdata$education, clustered_ogdata$living_with)
```

### T-test


```R
# Conduct t-test to compare mean value of "totalspent" between the two clusters and test significance of difference
t.test(clustered_ogdata$tspent[clustered_ogdata$cluster == 1], 
       clustered_ogdata$spent[clustered_ogdata$cluster == 2])

```


```R
cat("mean total spending of cluster 1: ", mean(clustered_ogdata$tspent[clustered_ogdata$cluster == 1]))
cat("\nmean total spending of cluster 2: ", mean(clustered_ogdata$tspent[clustered_ogdata$cluster == 2]))
```


```R
summary(clustered_ogdata$tspent[clustered_ogdata$cluster == 1])
```


```R
summary(clustered_ogdata$tspent[clustered_ogdata$cluster == 2])
```

H$_{0}$: No difference in mean spending (tspend) between the two clusters.  
H$_{1}$: There exists a significant difference in the mean value of "tspend" variable between the two clusters

Based on the results of the t-test, the p-value is less than the alpha level (e.g., 0.05), so we can reject the null hypothesis and conclude that there is a significant difference in the mean value of "tspent" between the two clusters. In other words, the statistically significant difference (in means) suggests that two clusters are characterized by different spending behaviors.

### Chi-square test

I am going to use a chi-square test to compare the proportion of observations in the data who are non-parents and parenta (0 and 1 respectively) between the two clusters.
The null hypothesis for this test is that there is no difference in the proportion of customers who are parents between the two clusters. The alternative hypothesis is that there is a significant difference in the proportions.


```R
table(clustered_ogdata$cluster, clustered_ogdata$is_parent)
```


```R
chisq_test <- chisq.test(table(clustered_ogdata$cluster, clustered_ogdata$is_parent))
print(chisq_test)
```

Since the p-value is less than the alpha, we can reject the null hypothesis that the proportion of parents is the same in both clusters. The results of the chi-square test indicate there is a statistically signiifcant difference in the proportion of customers who are parents between the two clusters. Overall, this suggests that the presence or absence of children in a household may be an important character in *distinguishing between the two customer clusters.*

### ANOVA test

Testing for statistically significant means among groups. For three two-way ANOVA test, I am comparing the means of total spending (tspent) across different factors (living situation, parental status, education) to test for statistical significance. The null hypothesis is that there is no significant difference in the mean total spending across the different groups.


```R
anovad <- clustered_ogdata %>% mutate(is_parent = if_else(is_parent == 1, "yes", "no")) %>%
  mutate(is_parent = as.factor(is_parent)) # convert to factor

```


```R
# Conduct a two-way ANOVA

anova <- aov(tspent ~ living_with*is_parent*education, data = anovad)
```


```R
# Check the ANOVA table
summary(anova)
```


```R
# Check the assumption of normality of residuals
qqnorm(resid(anova))
qqline(resid(anova))
```


```R
# Check the assumption of homogeneity of variances
plot(anova)
```


```R
# Check the interaction plot
interaction.plot(clustered_ogdata$income, clustered_ogdata$education, clustered_ogdata$tspent)
```


```R
# Conduct post-hoc test
TukeyHSD(anova)
```


```R
summary(anova)
```

**ANOVA findings:**

The extremely small p-value for the education variable indicates that there is a significant difference in total spending (mean) across different education groups. The same can be said for parental status of the customer (i.e. parent vs. non-parent). In other words, there is a significant difference between the total spending averages between parents and non-parents as well as among education groups.

In contrast, living situation ("alone" or "partnered") has no real bearing on total spending.

*Interactions*  

Furthermore, the extremely small p-value for interaction between is_parent and education allows us to reject the null hypothesis which states there is no significant interaction effect between parent status and education on total spending. 

In simpler terms, total spending is dependent on education level and whether the customer in question is a parent.

Overall, the results suggest that income and is_parent are significant factors that affect total spending, indicating that the effect of education on spending habits depends on whether the customer is a parent (and vice versa.

**Tukey test findings**  

The results of the tukey tests goes an extra layer deeper where it detects which pairing level indicate statistical signficance within each independent variable *and* pairings within each interaction.

### Observations

* The statistical tests above confirm that there is a significant difference and therefore pattern between the two clusters. Based on these observed (and tested) significant differences, there is a natural grouping or clustering observed in the data that sufficiently segments a customer according to the characteristics of the cluster.
* One of those characteristics, as revealed in the chi-squared test, is the presence of kids or absence of kids. Simply put, being parent puts a customer in one cluster over the other - which cluster or grouping would that be? There is a high probability that a parent customer would fall in *Cluster 1* according to the contingency table earlier above.

Key concept I learned from this whole process:

    So in a way, what you (talking to myself) are saying is that once I've deployed k-means algorithm on my data, I can add the newly generated cluster designations from the k-means as a variable to my dataset and begin grouping the dataframe by said cluser designations (however many 'k' I settled on). From this grouping, I can start to slice, manipulate, and transform the data by the available features based on these groupings – revealing insights on the characteristics of each grouping. Essentially, the cluster groupings retrieved from the k-means algorithm  act as the "labels" to segment or group the dataset by whatever feature(s) I decide to explore, and then explore deeper patterns based on the groupings.

### Possible follow-ups
* One way I could expand on this project is to further segment or group the data to 3 (or more) clusters and run analysis based on the groupings.

## 5. Visualizing Insights, as gained from our Unsupervised Learning Model (K-means)

In this final section, I will visualize the groupings using features as characteristics to highlight the clear distinctions between the clusters. The end product is a separate report visualizing the findings of the analysis. (Find it in the navigation sidebar!) 


```R
options(repr.plot.width=12, repr.plot.height=10)

# Create a data frame from the table results
table_results <- as.data.frame(table(kclust$cluster))

# Rename the columns
names(table_results) <- c("Cluster", "Count")

# Create the barplot
ggplot(table_results, aes(x = Cluster, y = Count, fill = Cluster)) +
  geom_bar(stat = "identity", width=0.5) +
  geom_text(aes(label = Count), vjust = -0.5, size = 5) +
  xlab("Cluster") +
  ylab("Count") +
  theme_minimal() + 
  scale_fill_manual(values = c("#FEC44F", "#8ED5D5"))

```


```R
ggplot(clustered_ogdata, aes(x = is_parent, fill = factor(cluster))) +
  geom_bar(stat = "count", width = 0.5, position = "stack") +
  xlab("is_parent") +
  ylab("Count") +
  theme_minimal() +
  scale_fill_manual(values = c("#FEC44F", "#8ED5D5"))
```


```R
ggplot(clustered_ogdata, aes(x = tspent, y = income, fill = factor(cluster))) + 
  geom_point(size = 3, stroke = 0.5, shape = 21, colour="white") +
  labs(title = "Cluster's Profile Based on Income And Spending") + 
  theme_bw() +
  theme(panel.background = element_rect(fill = "snow1")) +
  scale_fill_manual(values = c("#FEC44F", "#8ED5D5"))

#ggsave(filename = "scatter.png", width = 14, height = 6, dpi = 400)
```


```R
ggplot(clustered_ogdata, aes(x = factor(cluster), y = tspent, fill = factor(is_parent))) + 
  geom_jitter(position = position_jitter(width = 0.2), size = 3, stroke = 0.5, shape = 21, colour="white") +
  labs(title = "Parental character of each cluster") + 
  theme_bw() +
  theme(panel.background = element_rect(fill = "snow1")) + 
  scale_fill_manual(values = c("#FEC44F", "#8ED5D5"))

#ggsave(filename = "dots.png", width = 10, height = 12, dpi = 400)
```


```R
ggplot(clustered_ogdata, aes(x = dependents, fill = factor(cluster))) +
  geom_bar(stat = "count", width = 0.5, position = "stack") +
  xlab("dependents") +
  ylab("Count") +
  theme_minimal() +
  ggtitle("Dependent Profile by Count") +
  theme_bw() +
  theme(panel.background = element_rect(fill = "snow1")) +
  scale_fill_manual(values = c("#FEC44F", "#8ED5D5"))

#ggsave(filename = "dependent.png", width = 8, height = 8, dpi = 400)
```


```R
#Detailed distribution of spending
ggplot(clustered_ogdata, aes(x = factor(cluster), y = tspent, fill = factor(cluster))) +
  geom_boxplot() +
  xlab("Cluster") +
  ylab("Total Spending") +
  ggtitle("Boxplot of Total Spending by Cluster") +
  theme_minimal() +
  theme(panel.background = element_rect(fill = "snow1")) + 
  scale_fill_manual(values = c("#FEC44F", "#8ED5D5"))

#ggsave(filename = "boxplot.png", width = 6, height = 6, dpi = 400)
```


```R
clustered_ogdata$total_promos <- clustered_ogdata$acceptedcmp1 + clustered_ogdata$acceptedcmp2 + clustered_ogdata$acceptedcmp3 + clustered_ogdata$acceptedcmp4 + clustered_ogdata$acceptedcmp5
```


```R
# Exploration of recorded marketing campaign impact
clustered_ogdata %>%
    ggplot(aes(x=total_promos)) +
    geom_density(fill = "#69b3a2", color = "#e9ecef") +
    labs(title = "Total promotions accepted, density") +
    theme_minimal() +
    theme(panel.background = element_rect(fill = "snow1"))



#ggsave(filename = "density.png", width = 7, height = 7, dpi = 400)
```


```R
#reshape data
data_melt <- clustered_ogdata %>% select(numdealspurchases, numwebpurchases, numcatalogpurchases, numstorepurchases, numwebvisitsmonth, cluster) %>% pivot_longer(cols = numdealspurchases:numwebvisitsmonth, names_to = "purchase_type", values_to = "count")
```


```R
#create a grouped barplot, but use mean for y-axis
ggplot(data_melt, aes(x = purchase_type, y = count, fill = factor(cluster))) +
  stat_summary(fun = "mean", geom = "bar", position = "dodge", width = 0.7) +
  labs(title = "Mean Counts by Type and Cluster",
       x = "Purchase Type",
       y = "Mean Purchase",
       fill = "Cluster") +
  theme_minimal() +
  theme(panel.background = element_rect(fill = "snow1"),
       axis.text.x = element_text(angle = 40, hjust = 1)) + 
  scale_fill_manual(values = c("#FEC44F", "#8ED5D5"))

#ggsave(filename = "twinbars.png", width = 7, height = 7, dpi = 400)
```


```R
clustered_ogdata %>%
  ggplot(aes(x = living_with, y = tspent)) +
  geom_bar(stat = "summary", fun = "mean", width = 0.5, fill = "pink") +
  labs(title = "Spending Habits based on Living Situation", 
       x = "Living Situation", y = "Average Total Spending") +
  theme_minimal() +
  theme(panel.background = element_rect(fill = "snow1"))

#ggsave(filename = "cohabit.png", width = 5, height = 5, dpi = 400)
```


```R
# Create the plot
clustered_ogdata %>%
  group_by(cluster) %>%
  summarize(mean_tspent = mean(tspent)) %>%
  ggplot(aes(x = factor(cluster), y = mean_tspent, fill = factor(cluster))) +
  geom_bar(stat = "identity", width = 0.5) +
  labs(title = "Average Spending by Cluster",
       x = "Cluster Type",
       y = "Average Spending",
       fill = "Cluster Type") +
  theme_minimal() +
  theme(panel.background = element_rect(fill = "snow1")) + 
  scale_fill_manual(values = c("#FEC44F", "#8ED5D5"))

#ggsave(filename = "meanspend.png", width = 5, height = 6, dpi = 400)
```


```R

```


```R

```
