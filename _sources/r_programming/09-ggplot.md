# Visualization in R


~~~
surveys_complete <- read.csv("output_data/surveys_complete.csv")
head(surveys_complete)
~~~

## Data visualization

** Installation: **

~~~
setupLibrary <- function(libraryName){
  if (!require(libraryName, character.only = TRUE)){
    install.packages(libraryName, dep = TRUE)
    if (!require(libraryName, character.only = TRUE)){
      print('Package not found')
    }
  } else {
    print('Package is loaded')
  }
}

setupLibrary('dplyr')
setupLibrary('ggplot2')
~~~


ggplot: graphical presentations are described as a combination of elements and built by adding new elements

** Steps to build a ggplot **

Bind the plot to a specific data frame using the `data` argument

~~~
ggplot(data=surveys_complete)
~~~

define aesthetics (**`aes`**), by selecting the variables to be plotted and the variables to define presentation such as plotting size, shape, color, etc ...

~~~
ggplot(data = surveys_complete, aes(x = weight, y = hindfoot_length))
~~~

add **`geoms`** – graphical representation of the data in the plot (points, lines, bars). To add a geom to the plot use **`+`** operator:

~~~
ggplot(data = surveys_complete, aes(x = weight, y = hindfoot_length)) +
    geom_point()
~~~

The **`+`** enables using plot templates to explore various plot designs:

~~~
surveys_plot <- ggplot(data = surveys_complete, aes(x = weight, y = hindfoot_length))
~~~

~~~
surveys_plot + geom_point()
~~~

** Notes: **

- Any configurations defined inside `ggplot()` are also visible by all `geom_` layers. This includes x and y axis set up in `aes()`. 
- Individual `aes()` for a given `geom_` can be set independely of the global `aes()` in `ggplot()`.
- In the case of multi-line presentation, the `+` sign must be placed at the end of each line containing layer(s)

** Challenge: **

Scatter plots can be useful exploratory tools for small datasets. For data sets with large numbers of observations, such as the surveys_complete data set, overplotting of points can be a limitation of scatter plots. One strategy for handling such settings is to use hexagonal binning of observations. The plot space is tesselated into hexagons. Each hexagon is assigned a color based on the number of observations that fall within its boundaries. To use hexagonal binning with ggplot2, first install the R package hexbin from CRAN:

~~~
install.packages("hexbin")
~~~

Then use `geom_hex()` function from the ggplot2 package to visualize data

~~~

~~~


** Building plots iteratively: **

Start with a template:

~~~
surveys_plot <- ggplot(data = surveys_complete, aes(x = weight, y = hindfoot_length))
~~~

Choose a geom:

~~~
surveys_plot + geom_point()
~~~

Customize geom:

~~~
# add transparency
surveys_plot + geom_point(alpha = 0.1)
~~~


~~~
# add colors for all the points:
surveys_plot + geom_point(alpha = 0.1, color = "blue")
~~~

~~~
# color each species in the plot differently:
surveys_plot + geom_point(alpha = 0.1, aes(color=species_id))
~~~

Change to a different geom, `geom_boxplot()`

~~~
surveys_plot <- ggplot(data = surveys_complete, aes(x = species_id, y = hindfoot_length))
surveys_plot + geom_boxplot()
~~~

~~~
# adding points to boxplot to understand number of measurements and distribution:
surveys_plot + geom_boxplot(alpha = 0) +
    geom_jitter(alpha = 0.3, color = "tomato")
~~~

** Challenge: **

How can we show the box plot?     

~~~

~~~

** Challenge: **

Boxplots are useful summaries, but hide the shape of the distribution. For example, if there is a bimodal distribution, it would not be observed with a boxplot. An alternative to the boxplot is the violin plot (sometimes known as a beanplot), where the shape (of the density of points) is drawn.

- Replace the box plot with a violin plot; see `geom_violin()`

~~~

~~~

In many types of data, it is important to consider the scale of the observations. For example, it may be worth changing the scale of the axis to better distribute the observations in the space of the plot. Changing the scale of the axes is done similarly to adding/modifying other components (i.e., by incrementally adding commands). Try making these modifications:

- Create boxplot for the weights.
- Represent weight on the log10 scale; see `scale_y_log10()`
- Add color to the datapoints on your boxplot according to the species from which the sample was taken (species_id)

~~~

~~~

** Plotting time-series data: **

Number of counts per year for each species:

~~~
yearly_counts <- surveys_complete %>%
                 group_by(year, species_id) %>%
                 tally
head(yearly_counts)
~~~

Plot everything!

~~~
ggplot(data = yearly_counts, aes(x = year, y = n)) +
     geom_line()
~~~

We can improve the clarity of the graphs by separating them into individual lines for individual species

~~~
ggplot(data = yearly_counts, aes(x = year, y = n, group = species_id)) +
    geom_line()
~~~

We can make this even better!

~~~
ggplot(data = yearly_counts, aes(x = year, y = n, group = species_id, colour = species_id)) +
    geom_line()
~~~

** Faceting: ** 

It is possible to create multiple plots within a larger plot frame based on a factor variable within the data set. 

~~~
str(yearly_counts)
~~~

~~~
ggplot(data = yearly_counts, aes(x = year, y = n, group = species_id, colour = species_id)) +
    geom_line() +
    facet_wrap(~ species_id)
~~~

Further customization on faceted plots can be done. For example, we can split data within each individual plots into lines presenting male and female:

~~~
yearly_sex_counts <- surveys_complete %>%
                      group_by(year, species_id, sex) %>%
                      tally

head(yearly_sex_counts)
~~~

~~~
 ggplot(data = yearly_sex_counts, aes(x = year, y = n, color = species_id, group = sex)) +
     geom_line() +
     facet_wrap(~ species_id)
~~~

To improve this presentation, we can customize the theme layers

~~~
 ggplot(data = yearly_sex_counts, aes(x = year, y = n, color = species_id, group = sex)) +
     geom_line() +
     facet_wrap(~ species_id) +
     theme_bw() +
     theme(panel.grid.major.x = element_blank(),
       panel.grid.minor.x = element_blank(),
       panel.grid.major.y = element_blank(),
       panel.grid.minor.y = element_blank())
~~~

Since we already separate species into individual plots, we do not need to color the plot, but we will need to differentiate between the male and femal lines within each plot

~~~
ggplot(data = yearly_sex_counts, aes(x = year, y = n, color = sex, group = sex)) +
    geom_line() +
    facet_wrap(~ species_id) +
    theme_bw()
~~~

** Challenge: **

Create a plot that shows how the average weight of each species changes over the years. 

~~~

~~~

** Customization: **

Cheatsheet: https://www.rstudio.com/wp-content/uploads/2016/11/ggplot2-cheatsheet-2.1.pdf

Customize title and axis' titles

~~~
ggplot(data = yearly_sex_counts, aes(x = year, y = n, color = sex, group = sex)) +
    geom_line() +
    facet_wrap(~ species_id) +
    labs(title = 'Observed species in time',
         x = 'Year of observation',
         y = 'Number of species') +
    theme_bw()
~~~

Change font size

~~~
ggplot(data = yearly_sex_counts, aes(x = year, y = n, color = sex, group = sex)) +
    geom_line() +
    facet_wrap(~ species_id) +
    labs(title = 'Observed species in time',
        x = 'Year of observation',
        y = 'Number of species') +
    theme_bw() +
    theme(text=element_text(size=16, family="Arial"))
~~~

Change label orientation:

~~~
ggplot(data = yearly_sex_counts, aes(x = year, y = n, color = sex, group = sex)) +
    geom_line() +
    facet_wrap(~ species_id) +
    labs(title = 'Observed species in time',
        x = 'Year of observation',
        y = 'Number of species') +
    theme_bw() +
    theme(axis.text.x = element_text(colour="grey20", size=12, angle=90, hjust=.5, vjust=.5),
                        axis.text.y = element_text(colour="grey20", size=12),
          text=element_text(size=16, family="Arial"))
~~~

** Save plots to files: **

~~~
my_plot <- ggplot(data = yearly_sex_counts, aes(x = year, y = n, color = sex, group = sex)) +
    geom_line() +
    facet_wrap(~ species_id) +
    labs(title = 'Observed species in time',
        x = 'Year of observation',
        y = 'Number of species') +
    theme_bw() +
    theme(axis.text.x = element_text(colour="grey20", size=12, angle=90, hjust=.5, vjust=.5),
                        axis.text.y = element_text(colour="grey20", size=12),
          text=element_text(size=16, family="Arial"))
~~~

~~~
current_dir <- getwd()
output_graph_dir <- 'output_graph'

if (!file.exists(output_graph_dir)){
    dir.create(file.path(current_dir, output_graph_dir))
} else {
    print ("Directory already exists")
}
~~~

~~~
ggsave(file.path(current_dir, output_graph_dir,"yearly_sex_counts.png"), 
       my_plot, width=15, height=10)
~~~