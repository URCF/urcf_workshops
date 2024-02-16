# Ploting with ggplot


## Preparation

Download the following data files

- [surveys](https://github.com/URCF/urcf_workshops/blob/master/data/surveys.csv)
- [combined](https://github.com/URCF/urcf_workshops/blob/master/data/combined.csv)
- [species](https://github.com/URCF/urcf_workshops/blob/master/data/species.csv)


```{admonition} Reading data frames from files
:class: dropdown

~~~{r}
surveys <- read.csv("surveys.csv")
head(surveys)
~~~

*Header is TRUE or Header is FALSE, that is the question!*

~~~{r}
surveys <- read.csv("combined.csv", header = FALSE)
head(surveys)
~~~

~~~{r}
surveys <- read.csv("combined.csv")
head(surveys)
~~~

```

```{admonition} Data types
:class: dropdown

- Data frames are the *de facto* data structure for R's tabular data, and 
conceptionally equivalent to an Excel spreadsheet but is more powerful and versatile.
- Matrices (multi-dimensional) and vectors (one dimension) are also available 
for computational purposes. 
- Data frames represents a table whose columns are vectors with same length 
but possible different data types

*Structure of a data frame*

~~~{r}
str(surveys)
~~~

~~~{r}
summary(surveys)
~~~

*Size of a data frame*

~~~{r}
dim(surveys)
~~~

~~~{r}
nrow(surveys)
~~~

~~~{r}
ncol(surveys)
~~~

*Content of a data frame*

~~~{r}
head(surveys)
~~~

~~~{r}
head(surveys, n=10)
~~~

~~~{r}
tail(surveys)
~~~

~~~{r}
tail(surveys, n=10)
~~~

*Names*

~~~{r}
names(surveys)
~~~

~~~{r}
surveys_colnames <- names(surveys)
~~~

~~~{r}
surveys_colnames
~~~

~~~{r}
surveys_rownames <- rownames(surveys)
str(surveys_rownames)
~~~

```

```{admonition} Indexing and subsetting
:class: dropdown

Similar to an Excel spreadsheet, we can extract specific data from a dataframe via 'coordinates': row/column combinations

Accessing a single element

~~~{r}
surveys[1,1]
~~~

~~~{r}
surveys[1,2]
~~~

Accessing a block of elements

~~~{r}
surveys[1:5,2]
~~~

~~~{r}
surveys[2,3:7]
~~~

~~~{r}
surveys[1:5,3:7]
~~~

Accessing scattered groups of elements

~~~{r}
?c
~~~

~~~{r}
surveys[c(2:4,6:7),]
~~~

Excluding data with the `-` notation:

~~~{r}
surveys[1:5, -3]
~~~

Accessing columns by names:

~~~{r}
surveys[1:5,"month"]
~~~

~~~{r}
surveys[["month"]][1:5]
~~~


~~~{r}
surveys$month[1:5]
~~~

```

```{admonition} Challenge
:class: dropdown

- Create a data frame containing on observations from row 200 to the 
end of the `surveys` data set
- Create a data frame containing the row that is in the middle of the 
data frame. Store the content in a variable named `surveys_middle`.
- Combine `nrow` with the `-` notation to reproduce the 
behavior of `head(surveys)`

```

```{admonition} Factors
:class: dropdown

- Special class, representing categorical data
- Can be ordered or unordered
- Stored as integers with labels (text) associated with these unique integers
- Looked and behave like character vectors but are integers under the hood
- Once created, a `factor` object can only contain a pre-defined set of values, known as *levels*. 
- *Levels* are sorted alphabetically by default. 

~~~{r}
str(surveys)
~~~

~~~{r}
levels(surveys$sex)
~~~

~~~{r}
nlevels(surveys$sex)
~~~

Converting factors:

~~~{r}
as.character(surveys$sex)
~~~

~~~{r}
f <- factor(c(1990,1983,1977,1998,1990))
~~~

~~~{r}
f
~~~

~~~{r}
as.numeric(f) #incorrect
~~~

~~~{r}
as.numeric(as.character(f)) #works
~~~

~~~{r}
as.numeric(levels(f))[f] #recommended
~~~

Renaming factors:

~~~{r}
plot(surveys$sex)
~~~

~~~{r}
sex <- surveys$sex
~~~

~~~{r}
levels(sex)
~~~

~~~{r}
levels(sex)[1] <- "missing"
~~~

~~~{r}
plot(sex)
~~~

Using `stringsAsFactors=FALSE`

~~~{r}
surveys <- read.csv('data/combined.csv', stringsAsFactors = TRUE)
str(surveys)
~~~

~~~{r}
surveys <- read.csv('data/combined.csv', stringsAsFactors = FALSE)
str(surveys)
~~~

```

```{admonition} Data frames manipulation
:class: dropdown

~~~{r}
if (!require('dplyr', character.only = TRUE)){
  install.packages('dplyr', dep = TRUE)
  if (!require('dplyr', character.only = TRUE)){
    print ('Package not found')
  }
}
~~~

** Common `dplyr` functions: **
- `select()`
- `filter()`
- `mutate()`
- `groupby()`
- `summarize()`
- `%>%`

** Cheatsheet: **
http://www.rstudio.com/wp-content/uploads/2015/02/data-wrangling-cheatsheet.pdf

** Selecting columns and filtering rows **

~~~{r}
select(surveys, plot_id, species_id, weight)
~~~


~~~{r}
filter(surveys, year == 1995)
~~~

** Pipes: combining multile select and filter actions **

~~~{r}
surveys %>%
  filter(weight < 5) %>%
  select(species_id, sex, weight)
~~~

~~~{r}
surveys_sml <- surveys %>%
  filter(weight < 5) %>%
  select(species_id, sex, weight)

surveys_sml
~~~

** Challenge: **

Using pipes, subset the `survey` data to include individuals collected before 1995 and retain only the columns `year`, `sex`, and `weight`. 

** Mutate: create new columns based on existing columns **

~~~{r}
surveys %>%
  mutate(weight_kg = weight / 1000) %>%
  head  
~~~

~~~{r}
surveys %>%
  filter(!is.na(weight)) %>%
  mutate(weight_kg = weight / 1000) %>%
  head
~~~

- `is.na`: determines whether something is NA (not available - missing values)
- `!`: negates a logical value

** Challenge: **

Create a new data frame from the survey data that meets the following criteria: 

contains only the species_id column and a new column called hindfoot_half containing values that are half the hindfoot_length values. In this hindfoot_half column, there are no NAs and all values are less than 30.

** Split-apply-combine data analysis and the summarize() function **

- split data into groups
- apply some analysis to each group
- combine the results

** `group_by()` and `summarize()`: **

~~~{r}
surveys %>%
  group_by(sex) %>%
  summarize(mean_weight = mean(weight, na.rm = TRUE))
~~~

~~~{r}
surveys %>%
  group_by(sex, species_id) %>%
  summarize(mean_weight = mean(weight, na.rm = TRUE))
~~~

- `NaN`: not a number
- Need filtering to remove missing values

~~~{r}
surveys %>%
  filter(!is.na(weight)) %>%
  group_by(sex, species_id) %>%
  summarize(mean_weight = mean(weight))
~~~

~~~{r}
x <- surveys %>%
       filter(!is.na(weight)) %>%
       group_by(sex, species_id) %>%
       summarize(mean_weight = mean(weight))
str(x)
~~~

- If you want to display more data, you use the `print()` function at the end of your chain with the argument `n` specifying the number of rows to display:

~~~{r}
surveys %>%
  filter(!is.na(weight)) %>%
  group_by(sex, species_id) %>%
  summarize(mean_weight = mean(weight)) %>%
  print(n = 15)
~~~

Summarization on multiple variables at the same time is also possible

~~~{r}
surveys %>%
  filter(!is.na(weight)) %>%
  group_by(sex, species_id) %>%
  summarize(mean_weight = mean(weight),
            min_weight = min(weight))
~~~

** Tallying: simply counting things: **

~~~{r}
surveys %>%
  group_by(sex) %>%
  tally
~~~

** Challenge: **

- How many individuals were caught in each plot_type surveyed?
- Use `group_by()` and `summarize()` to find the mean, min, and max hindfoot length for each species (using `species_id`).

** Exporting data to file: **

- Write cleaned data to file, so that data cleaning process does not have to redone
- Output data should be stored in different location from original raw data

Conditional statement:

~~~{r}
if (condition is true){
    do something
} else {
    do something else
}
~~~

~~~{r}
current_dir <- getwd()
output_data_dir <- 'output_data'

if (!file.exists(output_data_dir)){
    dir.create(file.path(current_dir, output_data_dir))
} else {
    print ("Directory already exists")
}
~~~

~~~{r}
?dir.create
~~~


~~~{r}
?file.path
~~~

Create a clean data set with missing observations removed

~~~{r}
surveys_complete <- surveys %>%
  filter(species_id != "",         # remove missing species_id
         !is.na(weight),           # remove missing weight
         !is.na(hindfoot_length),  # remove missing hindfoot_length
         sex != "")                # remove missing sex
~~~

Additional filters: Remove rare species (less than 50 observations)

- Create index of rare species
- Filter rare species from the cleaned data set

~~~{r}
## Extract the most common species_id
species_counts <- surveys_complete %>%
  group_by(species_id) %>%
  tally %>%
  filter(n >= 50)
~~~

~~~{r}
## Only keep the most common species
surveys_complete <- surveys_complete %>%
  filter(species_id %in% species_counts$species_id)
~~~

Write data to file

~~~{r}
write.csv(surveys_complete, file = file.path(output_data_dir, "surveys_complete.csv"),
          row.names=FALSE)
~~~

```