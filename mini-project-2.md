Mini Data Analysis Milestone 2
================

*To complete this milestone, you can either edit [this `.rmd`
file](https://raw.githubusercontent.com/UBC-STAT/stat545.stat.ubc.ca/master/content/mini-project/mini-project-2.Rmd)
directly. Fill in the sections that are commented out with
`<!--- start your work here--->`. When you are done, make sure to knit
to an `.md` file by changing the output in the YAML header to
`github_document`, before submitting a tagged release on canvas.*

# Welcome to the rest of your mini data analysis project!

In Milestone 1, you explored your data. and came up with research
questions. This time, we will finish up our mini data analysis and
obtain results for your data by:

- Making summary tables and graphs
- Manipulating special data types in R: factors and/or dates and times.
- Fitting a model object to your data, and extract a result.
- Reading and writing data as separate files.

We will also explore more in depth the concept of *tidy data.*

**NOTE**: The main purpose of the mini data analysis is to integrate
what you learn in class in an analysis. Although each milestone provides
a framework for you to conduct your analysis, it’s possible that you
might find the instructions too rigid for your data set. If this is the
case, you may deviate from the instructions – just make sure you’re
demonstrating a wide range of tools and techniques taught in this class.

# Instructions

**To complete this milestone**, edit [this very `.Rmd`
file](https://raw.githubusercontent.com/UBC-STAT/stat545.stat.ubc.ca/master/content/mini-project/mini-project-2.Rmd)
directly. Fill in the sections that are tagged with
`<!--- start your work here--->`.

**To submit this milestone**, make sure to knit this `.Rmd` file to an
`.md` file by changing the YAML output settings from
`output: html_document` to `output: github_document`. Commit and push
all of your work to your mini-analysis GitHub repository, and tag a
release on GitHub. Then, submit a link to your tagged release on canvas.

**Points**: This milestone is worth 50 points: 45 for your analysis, and
5 for overall reproducibility, cleanliness, and coherence of the Github
submission.

**Research Questions**: In Milestone 1, you chose two research questions
to focus on. Wherever realistic, your work in this milestone should
relate to these research questions whenever we ask for justification
behind your work. In the case that some tasks in this milestone don’t
align well with one of your research questions, feel free to discuss
your results in the context of a different research question.

# Learning Objectives

By the end of this milestone, you should:

- Understand what *tidy* data is, and how to create it using `tidyr`.
- Generate a reproducible and clear report using R Markdown.
- Manipulating special data types in R: factors and/or dates and times.
- Fitting a model object to your data, and extract a result.
- Reading and writing data as separate files.

# Setup

Begin by loading your data and the tidyverse package below:

``` r
library(datateachr) # <- might contain the data you picked!
library(tidyverse)
library(ggplot2)
```

# Task 1: Process and summarize your data

From milestone 1, you should have an idea of the basic structure of your
dataset (e.g. number of rows and columns, class types, etc.). Here, we
will start investigating your data more in-depth using various data
manipulation functions.

### 1.1 (1 point)

First, write out the 4 research questions you defined in milestone 1
were. This will guide your work through milestone 2:

<!-------------------------- Start your work below ---------------------------->

1.  How does the tree species distribution vary across different
    neighborhoods in Vancouver, and are there specific neighborhoods
    with a higher prevalence of certain tree species?

2.  Is there a correlation between the age of trees and their diameter,
    and can we identify any trends in how tree growth varies with time
    since planting?

3.  What is the relationship between the presence of root barriers and
    the health and growth of trees in Vancouver? Are trees with root
    barriers more likely to thrive?

4.  How does the distribution of tree heights differ across various
    neighbourhoods in Vancouver?
    <!----------------------------------------------------------------------------->

Here, we will investigate your data using various data manipulation and
graphing functions.

### 1.2 (8 points)

Now, for each of your four research questions, choose one task from
options 1-4 (summarizing), and one other task from 4-8 (graphing). You
should have 2 tasks done for each research question (8 total). Make sure
it makes sense to do them! (e.g. don’t use a numerical variables for a
task that needs a categorical variable.). Comment on why each task helps
(or doesn’t!) answer the corresponding research question.

Ensure that the output of each operation is printed!

Also make sure that you’re using dplyr and ggplot2 rather than base R.
Outside of this project, you may find that you prefer using base R
functions for certain tasks, and that’s just fine! But part of this
project is for you to practice the tools we learned in class, which is
dplyr and ggplot2.

**Summarizing:**

1.  Compute the *range*, *mean*, and *two other summary statistics* of
    **one numerical variable** across the groups of **one categorical
    variable** from your data.
2.  Compute the number of observations for at least one of your
    categorical variables. Do not use the function `table()`!
3.  Create a categorical variable with 3 or more groups from an existing
    numerical variable. You can use this new variable in the other
    tasks! *An example: age in years into “child, teen, adult, senior”.*
4.  Compute the proportion and counts in each category of one
    categorical variable across the groups of another categorical
    variable from your data. Do not use the function `table()`!

**Graphing:**

6.  Create a graph of your choosing, make one of the axes logarithmic,
    and format the axes labels so that they are “pretty” or easier to
    read.
7.  Make a graph where it makes sense to customize the alpha
    transparency.

Using variables and/or tables you made in one of the “Summarizing”
tasks:

8.  Create a graph that has at least two geom layers.
9.  Create 3 histograms, with each histogram having different sized
    bins. Pick the “best” one and explain why it is the best.

Make sure it’s clear what research question you are doing each operation
for!

<!------------------------- Start your work below ----------------------------->

``` r
# Review the dataset
glimpse(vancouver_trees)
```

    ## Rows: 146,611
    ## Columns: 20
    ## $ tree_id            <dbl> 149556, 149563, 149579, 149590, 149604, 149616, 149…
    ## $ civic_number       <dbl> 494, 450, 4994, 858, 5032, 585, 4909, 4925, 4969, 7…
    ## $ std_street         <chr> "W 58TH AV", "W 58TH AV", "WINDSOR ST", "E 39TH AV"…
    ## $ genus_name         <chr> "ULMUS", "ZELKOVA", "STYRAX", "FRAXINUS", "ACER", "…
    ## $ species_name       <chr> "AMERICANA", "SERRATA", "JAPONICA", "AMERICANA", "C…
    ## $ cultivar_name      <chr> "BRANDON", NA, NA, "AUTUMN APPLAUSE", NA, "CHANTICL…
    ## $ common_name        <chr> "BRANDON ELM", "JAPANESE ZELKOVA", "JAPANESE SNOWBE…
    ## $ assigned           <chr> "N", "N", "N", "Y", "N", "N", "N", "N", "N", "N", "…
    ## $ root_barrier       <chr> "N", "N", "N", "N", "N", "N", "N", "N", "N", "N", "…
    ## $ plant_area         <chr> "N", "N", "4", "4", "4", "B", "6", "6", "3", "3", "…
    ## $ on_street_block    <dbl> 400, 400, 4900, 800, 5000, 500, 4900, 4900, 4900, 7…
    ## $ on_street          <chr> "W 58TH AV", "W 58TH AV", "WINDSOR ST", "E 39TH AV"…
    ## $ neighbourhood_name <chr> "MARPOLE", "MARPOLE", "KENSINGTON-CEDAR COTTAGE", "…
    ## $ street_side_name   <chr> "EVEN", "EVEN", "EVEN", "EVEN", "EVEN", "ODD", "ODD…
    ## $ height_range_id    <dbl> 2, 4, 3, 4, 2, 2, 3, 3, 2, 2, 2, 5, 3, 2, 2, 2, 2, …
    ## $ diameter           <dbl> 10.00, 10.00, 4.00, 18.00, 9.00, 5.00, 15.00, 14.00…
    ## $ curb               <chr> "N", "N", "Y", "Y", "Y", "Y", "Y", "Y", "Y", "Y", "…
    ## $ date_planted       <date> 1999-01-13, 1996-05-31, 1993-11-22, 1996-04-29, 19…
    ## $ longitude          <dbl> -123.1161, -123.1147, -123.0846, -123.0870, -123.08…
    ## $ latitude           <dbl> 49.21776, 49.21776, 49.23938, 49.23469, 49.23894, 4…

``` r
summary(vancouver_trees)
```

    ##     tree_id        civic_number    std_street         genus_name       
    ##  Min.   :    12   Min.   :    0   Length:146611      Length:146611     
    ##  1st Qu.: 65464   1st Qu.: 1306   Class :character   Class :character  
    ##  Median :134903   Median : 2604   Mode  :character   Mode  :character  
    ##  Mean   :131892   Mean   : 2937                                        
    ##  3rd Qu.:194450   3rd Qu.: 4005                                        
    ##  Max.   :266203   Max.   :17888                                        
    ##                                                                        
    ##  species_name       cultivar_name      common_name          assigned        
    ##  Length:146611      Length:146611      Length:146611      Length:146611     
    ##  Class :character   Class :character   Class :character   Class :character  
    ##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
    ##                                                                             
    ##                                                                             
    ##                                                                             
    ##                                                                             
    ##  root_barrier        plant_area        on_street_block  on_street        
    ##  Length:146611      Length:146611      Min.   :   0    Length:146611     
    ##  Class :character   Class :character   1st Qu.:1300    Class :character  
    ##  Mode  :character   Mode  :character   Median :2600    Mode  :character  
    ##                                        Mean   :2909                      
    ##                                        3rd Qu.:4000                      
    ##                                        Max.   :9900                      
    ##                                                                          
    ##  neighbourhood_name street_side_name   height_range_id     diameter     
    ##  Length:146611      Length:146611      Min.   : 0.000   Min.   :  0.00  
    ##  Class :character   Class :character   1st Qu.: 1.000   1st Qu.:  3.50  
    ##  Mode  :character   Mode  :character   Median : 2.000   Median :  9.00  
    ##                                        Mean   : 2.627   Mean   : 11.49  
    ##                                        3rd Qu.: 4.000   3rd Qu.: 16.50  
    ##                                        Max.   :10.000   Max.   :435.00  
    ##                                                                         
    ##      curb            date_planted          longitude         latitude    
    ##  Length:146611      Min.   :1989-10-27   Min.   :-123.2   Min.   :49.20  
    ##  Class :character   1st Qu.:1998-02-23   1st Qu.:-123.1   1st Qu.:49.23  
    ##  Mode  :character   Median :2004-01-28   Median :-123.1   Median :49.25  
    ##                     Mean   :2004-04-07   Mean   :-123.1   Mean   :49.25  
    ##                     3rd Qu.:2010-03-02   3rd Qu.:-123.1   3rd Qu.:49.26  
    ##                     Max.   :2019-07-03   Max.   :-123.0   Max.   :49.29  
    ##                     NA's   :76548        NA's   :22771    NA's   :22771

Research Question One: How does the tree species distribution vary
across different neighborhoods in Vancouver, and are there specific
neighborhoods with a higher prevalence of certain tree species? Tasks: 1
and 6

``` r
# Create a graph of your choosing, make one of the axes logarithmic, and format the axes labels so that they are "pretty" or easier to read.

# Create a new variable 'tree_count' representing the number of trees in each species
vancouver_trees <- vancouver_trees %>%
  group_by(neighbourhood_name, species_name) %>%
  mutate(tree_count = n()) %>%
  ungroup()

# Group the data by 'neighbourhood_name' and 'species_name' and compute summary statistics
summary_stats <- vancouver_trees %>%
  group_by(neighbourhood_name, species_name) %>%
  summarize(
    range_trees = max(tree_id) - min(tree_id),
    mean_trees = mean(tree_id),
    total_trees = sum(tree_count),
    unique_cultivars = n_distinct(cultivar_name)
  ) %>%
  ungroup()
```

    ## `summarise()` has grouped output by 'neighbourhood_name'. You can override
    ## using the `.groups` argument.

``` r
# Print the summary statistics
print(summary_stats)
```

    ## # A tibble: 3,056 × 6
    ##    neighbourhood_name species_name   range_trees mean_trees total_trees
    ##    <chr>              <chr>                <dbl>      <dbl>       <int>
    ##  1 ARBUTUS-RIDGE      ABIES                    1    265596.           4
    ##  2 ARBUTUS-RIDGE      ACERIFOLIA   X      179879     81105.       10609
    ##  3 ARBUTUS-RIDGE      ACUTISSIMA           41489    133680.          25
    ##  4 ARBUTUS-RIDGE      ALNIFOLIA           229993    174484.         256
    ##  5 ARBUTUS-RIDGE      AMERICANA           254815    115012.       50625
    ##  6 ARBUTUS-RIDGE      AQUIFOLIUM           95635     69185.           9
    ##  7 ARBUTUS-RIDGE      ARIA                108901    224187.         361
    ##  8 ARBUTUS-RIDGE      ARNOLDIANA X         61233    213387           16
    ##  9 ARBUTUS-RIDGE      AUCUPARIA           160907    121185.         256
    ## 10 ARBUTUS-RIDGE      AVIUM                94891     44944.         225
    ## # ℹ 3,046 more rows
    ## # ℹ 1 more variable: unique_cultivars <int>

``` r
# Create a box plot of the number of trees in different species in the "DOWNTOWN" neighborhood 
# Filter the data for the "DOWNTOWN" neighborhood
downtown_trees <- vancouver_trees %>%
  filter(neighbourhood_name == "DOWNTOWN")

# Create a box plot with a logarithmic y-axis
ggplot(downtown_trees, aes(x = species_name, y = tree_count)) +
  geom_boxplot() +
  scale_y_log10() +  # Use a logarithmic scale on the y-axis
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 45, hjust = 1)) +
  labs(x = "Species Name", y = "Number of Trees (log scale)", title = "Box Plot of Number of Trees in Different Species in DOWNTOWN") +
  scale_x_discrete(labels = function(labels) {
    str_wrap(labels, width = 15)  # Wrap long labels to multiple lines for readability
  })
```

![](mini-project-2_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

Research Question Two: Is there a correlation between the age of trees
and their diameter, and can we identify any trends in how tree growth
varies with time since planting? Tasks: 2 and 6

``` r
# Calculate tree age (in years) by subtracting the year of planting from the current year
current_year <- as.numeric(format(Sys.Date(), "%Y"))
vancouver_trees <- vancouver_trees %>%
  mutate(tree_age = current_year - year(date_planted))

# Create a scatter plot of tree age vs. log-transformed tree diameter
ggplot(vancouver_trees, aes(x = tree_age, y = log10(diameter))) +
  geom_point() +
  labs(
    title = "Scatter Plot of Tree Age vs. Log(Diameter)",
    x = "Tree Age (Years)",
    y = "Log(Diameter)"
  ) +
  theme_minimal()
```

    ## Warning: Removed 76548 rows containing missing values (`geom_point()`).

![](mini-project-2_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

Research Question Three: hat is the relationship between the presence of
root barriers and the health and growth of trees in Vancouver? Tasks: 2
and 8

``` r
# Filter the dataset to include only relevant columns and remove missing values
tree_data <- vancouver_trees %>%
  select(root_barrier, height_range_id) %>%
  drop_na()

# Create a box plot
ggplot(tree_data, aes(x = root_barrier, y = height_range_id, fill = root_barrier)) +
  geom_boxplot() +
  labs(x = "Root Barrier", y = "Height Range ID", title = "Box Plot of Tree Height by Root Barrier") +
  theme_minimal()
```

![](mini-project-2_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

Research Question Four: How does the distribution of tree heights differ
across various neighbourhoods in Vancouver? Tasks: 3 and 8

``` r
# Filter the dataset to include only relevant columns and remove missing values
tree_data <- vancouver_trees %>%
  select(neighbourhood_name, height_range_id) %>%
  drop_na()

# Calculate the mean height for each neighborhood
neighborhood_height_means <- tree_data %>%
  group_by(neighbourhood_name) %>%
  summarize(mean_height = mean(height_range_id))

# Arrange the neighborhoods by mean height in descending order
neighborhood_height_means <- neighborhood_height_means %>%
  arrange(desc(mean_height))

# View the result
neighborhood_height_means
```

    ## # A tibble: 22 × 2
    ##    neighbourhood_name mean_height
    ##    <chr>                    <dbl>
    ##  1 KITSILANO                 3.26
    ##  2 SHAUGHNESSY               3.24
    ##  3 WEST POINT GREY           3.03
    ##  4 DUNBAR-SOUTHLANDS         3.03
    ##  5 WEST END                  2.88
    ##  6 KERRISDALE                2.81
    ##  7 FAIRVIEW                  2.80
    ##  8 ARBUTUS-RIDGE             2.72
    ##  9 MOUNT PLEASANT            2.65
    ## 10 GRANDVIEW-WOODLAND        2.60
    ## # ℹ 12 more rows

``` r
# Create a bar plot
ggplot(neighborhood_height_means, aes(x = reorder(neighbourhood_name, -mean_height), y = mean_height)) +
  geom_bar(stat = "identity", fill = "skyblue") +
  labs(x = "Neighborhood", y = "Mean Tree Height", title = "Mean Tree Height by Neighborhood") +
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 45, hjust = 1))
```

![](mini-project-2_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

<!----------------------------------------------------------------------------->

### 1.3 (2 points)

Based on the operations that you’ve completed, how much closer are you
to answering your research questions? Think about what aspects of your
research questions remain unclear. Can your research questions be
refined, now that you’ve investigated your data a bit more? Which
research questions are yielding interesting results?

<!------------------------- Write your answer here ---------------------------->

Visualization serves as an invaluable tool for data exploration and
comprehension. However, it alone may not suffice to fully address my
research questions. To deliver more comprehensive responses to these
queries, additional statistical analysis such as regression models or
hypothesis testing are necessary.

Research question four has emerged as particularly revealing. As evident
from the bar plot, specific neighborhoods stand out for having notably
taller trees, including Kitsilano (with an average height of 3.26
meters) and Shaughnessy (3.24 meters). Conversely, neighborhoods like
Oakridge (2.14 meters) and Renfrew-Collingwood (2.23 meters) exhibit an
estimated one-meter lower average tree height.
<!----------------------------------------------------------------------------->

# Task 2: Tidy your data

In this task, we will do several exercises to reshape our data. The goal
here is to understand how to do this reshaping with the `tidyr` package.

A reminder of the definition of *tidy* data:

- Each row is an **observation**
- Each column is a **variable**
- Each cell is a **value**

### 2.1 (2 points)

Based on the definition above, can you identify if your data is tidy or
untidy? Go through all your columns, or if you have \>8 variables, just
pick 8, and explain whether the data is untidy or tidy.

<!--------------------------- Start your work below --------------------------->

``` r
# Use the head() function to display the first few rows with the first eight variables
head(vancouver_trees[, 1:8])
```

    ## # A tibble: 6 × 8
    ##   tree_id civic_number std_street genus_name species_name cultivar_name  
    ##     <dbl>        <dbl> <chr>      <chr>      <chr>        <chr>          
    ## 1  149556          494 W 58TH AV  ULMUS      AMERICANA    BRANDON        
    ## 2  149563          450 W 58TH AV  ZELKOVA    SERRATA      <NA>           
    ## 3  149579         4994 WINDSOR ST STYRAX     JAPONICA     <NA>           
    ## 4  149590          858 E 39TH AV  FRAXINUS   AMERICANA    AUTUMN APPLAUSE
    ## 5  149604         5032 WINDSOR ST ACER       CAMPESTRE    <NA>           
    ## 6  149616          585 W 61ST AV  PYRUS      CALLERYANA   CHANTICLEER    
    ## # ℹ 2 more variables: common_name <chr>, assigned <chr>

The dataset appears to adhere to the principles of tidy data. Each
column represents a variable, each row represents a unique observation,
and each cell contains a value. There are no issues with multiple
variables being stored in a single column or observations being spread
across multiple tables.
<!----------------------------------------------------------------------------->

### 2.2 (4 points)

Now, if your data is tidy, untidy it! Then, tidy it back to it’s
original state.

If your data is untidy, then tidy it! Then, untidy it back to it’s
original state.

Be sure to explain your reasoning for this task. Show us the “before”
and “after”.

<!--------------------------- Start your work below --------------------------->

``` r
# Create an untidy structure
```

``` r
# Tidy the data back to its original state
```

<!----------------------------------------------------------------------------->

### 2.3 (4 points)

Now, you should be more familiar with your data, and also have made
progress in answering your research questions. Based on your interest,
and your analyses, pick 2 of the 4 research questions to continue your
analysis in the remaining tasks:

<!-------------------------- Start your work below ---------------------------->

1.  What is the relationship between the presence of root barriers and
    the health and growth of trees in Vancouver? Are trees with root
    barriers more likely to thrive?
2.  Is there a correlation between the age of trees and their diameter,
    and can we identify any trends in how tree growth varies with time
    since planting?

<!----------------------------------------------------------------------------->

Explain your decision for choosing the above two research questions.

<!--------------------------- Start your work below --------------------------->

These two research questions need further statistical analysis such as
regression models or hypothesis testing to answer.
<!----------------------------------------------------------------------------->

Now, try to choose a version of your data that you think will be
appropriate to answer these 2 questions. Use between 4 and 8 functions
that we’ve covered so far (i.e. by filtering, cleaning, tidy’ing,
dropping irrelevant columns, etc.).

(If it makes more sense, then you can make/pick two versions of your
data, one for each research question.)

<!--------------------------- Start your work below --------------------------->

# Task 3: Modelling

## 3.0 (no points)

Pick a research question from 1.2, and pick a variable of interest
(we’ll call it “Y”) that’s relevant to the research question. Indicate
these.

<!-------------------------- Start your work below ---------------------------->

**Research Question**: What is the relationship between the presence of
root barriers and the health and growth of trees in Vancouver? Are trees
with root barriers more likely to thrive?

**Variable of interest**: root_barrier

<!----------------------------------------------------------------------------->

## 3.1 (3 points)

Fit a model or run a hypothesis test that provides insight on this
variable with respect to the research question. Store the model object
as a variable, and print its output to screen. We’ll omit having to
justify your choice, because we don’t expect you to know about model
specifics in STAT 545.

- **Note**: It’s OK if you don’t know how these models/tests work. Here
  are some examples of things you can do here, but the sky’s the limit.

  - You could fit a model that makes predictions on Y using another
    variable, by using the `lm()` function.
  - You could test whether the mean of Y equals 0 using `t.test()`, or
    maybe the mean across two groups are different using `t.test()`, or
    maybe the mean across multiple groups are different using `anova()`
    (you may have to pivot your data for the latter two).
  - You could use `lm()` to test for significance of regression
    coefficients.

<!-------------------------- Start your work below ---------------------------->

``` r
# Data preparation
tree_data <- vancouver_trees %>%
  select(root_barrier, height_range_id) %>%
  drop_na()

# Data analysis
with_barrier <- tree_data %>% filter(root_barrier == "Y")
without_barrier <- tree_data %>% filter(root_barrier == "N")

# Compare the means of tree height with and without root barriers
mean_height_with_barrier <- mean(with_barrier$height_range_id)
mean_height_without_barrier <- mean(without_barrier$height_range_id)

# Perform a t-test to assess the significance
t_test_result <- t.test(with_barrier$height_range_id, without_barrier$height_range_id)

# Interpretation
cat("Mean height with root barrier:", mean_height_with_barrier, "\n")
```

    ## Mean height with root barrier: 1.302534

``` r
cat("Mean height without root barrier:", mean_height_without_barrier, "\n")
```

    ## Mean height without root barrier: 2.715187

``` r
cat("T-test p-value:", t_test_result$p.value, "\n")
```

    ## T-test p-value: 0

<!----------------------------------------------------------------------------->

## 3.2 (3 points)

Produce something relevant from your fitted model: either predictions on
Y, or a single value like a regression coefficient or a p-value.

- Be sure to indicate in writing what you chose to produce.
- Your code should either output a tibble (in which case you should
  indicate the column that contains the thing you’re looking for), or
  the thing you’re looking for itself.
- Obtain your results using the `broom` package if possible. If your
  model is not compatible with the broom function you’re needing, then
  you can obtain your results by some other means, but first indicate
  which broom function is not compatible.

<!-------------------------- Start your work below ---------------------------->

I chose to produce a p-value to assess the significance between root
barrier and tree height

``` r
# Load the broom package
library(broom)

# Create a data frame with results
results_df <- tibble(
  Group = c("With Root Barrier", "Without Root Barrier"),
  Mean_Height = c(mean_height_with_barrier, mean_height_without_barrier),
  P_Value = t_test_result$p.value
)

# Display the results
results_df
```

    ## # A tibble: 2 × 3
    ##   Group                Mean_Height P_Value
    ##   <chr>                      <dbl>   <dbl>
    ## 1 With Root Barrier           1.30       0
    ## 2 Without Root Barrier        2.72       0

<!----------------------------------------------------------------------------->

# Task 4: Reading and writing data

Get set up for this exercise by making a folder called `output` in the
top level of your project folder / repository. You’ll be saving things
there.

## 4.1 (3 points)

Take a summary table that you made from Task 1, and write it as a csv
file in your `output` folder. Use the `here::here()` function.

- **Robustness criteria**: You should be able to move your Mini Project
  repository / project folder to some other location on your computer,
  or move this very Rmd file to another location within your project
  repository / folder, and your code should still work.
- **Reproducibility criteria**: You should be able to delete the csv
  file, and remake it simply by knitting this Rmd file.

<!-------------------------- Start your work below ---------------------------->

``` r
# Load the here package
library(here)
```

    ## here() starts at /Users/yiifan/Desktop/STAT 545/mda-yiifan-L/mda-yiifan-L

``` r
# Define the output file path using here::here()
output_file <- here("output", "summary_stats.csv")

# Write the summary table to a CSV file
write.csv(summary_stats, file = output_file, row.names = FALSE)

# Print the file path
cat("CSV file saved at:", output_file, "\n")
```

    ## CSV file saved at: /Users/yiifan/Desktop/STAT 545/mda-yiifan-L/mda-yiifan-L/output/summary_stats.csv

<!----------------------------------------------------------------------------->

## 4.2 (3 points)

Write your model object from Task 3 to an R binary file (an RDS), and
load it again. Be sure to save the binary file in your `output` folder.
Use the functions `saveRDS()` and `readRDS()`.

- The same robustness and reproducibility criteria as in 4.1 apply here.

<!-------------------------- Start your work below ---------------------------->

``` r
model <- lm(height_range_id ~ root_barrier, data = tree_data)

# Define the output file path using here::here()
output_file <- here("output", "model.rds")

# Save the model object to an RDS file
saveRDS(model, file = output_file)

# Print the file path
cat("Model saved as RDS file at:", output_file, "\n")
```

    ## Model saved as RDS file at: /Users/yiifan/Desktop/STAT 545/mda-yiifan-L/mda-yiifan-L/output/model.rds

``` r
# Load the model from the RDS file
loaded_model <- readRDS(output_file)

# Confirm that the loaded model is the same as the original model
identical(model, loaded_model)
```

    ## [1] TRUE

<!----------------------------------------------------------------------------->

# Overall Reproducibility/Cleanliness/Coherence Checklist

Here are the criteria we’re looking for.

## Coherence (0.5 points)

The document should read sensibly from top to bottom, with no major
continuity errors.

The README file should still satisfy the criteria from the last
milestone, i.e. it has been updated to match the changes to the
repository made in this milestone.

## File and folder structure (1 points)

You should have at least three folders in the top level of your
repository: one for each milestone, and one output folder. If there are
any other folders, these are explained in the main README.

Each milestone document is contained in its respective folder, and
nowhere else.

Every level-1 folder (that is, the ones stored in the top level, like
“Milestone1” and “output”) has a `README` file, explaining in a sentence
or two what is in the folder, in plain language (it’s enough to say
something like “This folder contains the source for Milestone 1”).

## Output (1 point)

All output is recent and relevant:

- All Rmd files have been `knit`ted to their output md files.
- All knitted md files are viewable without errors on Github. Examples
  of errors: Missing plots, “Sorry about that, but we can’t show files
  that are this big right now” messages, error messages from broken R
  code
- All of these output files are up-to-date – that is, they haven’t
  fallen behind after the source (Rmd) files have been updated.
- There should be no relic output files. For example, if you were
  knitting an Rmd to html, but then changed the output to be only a
  markdown file, then the html file is a relic and should be deleted.

Our recommendation: delete all output files, and re-knit each
milestone’s Rmd file, so that everything is up to date and relevant.

## Tagged release (0.5 point)

You’ve tagged a release for Milestone 2.

### Attribution

Thanks to Victor Yuan for mostly putting this together.
