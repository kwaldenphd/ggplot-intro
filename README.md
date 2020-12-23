# Getting started with `ggplot2`

<a href="http://creativecommons.org/licenses/by-nc/4.0/" rel="license"><img style="border-width: 0;" src="https://i.creativecommons.org/l/by-nc/4.0/88x31.png" alt="Creative Commons License" /></a>
This tutorial is licensed under a <a href="http://creativecommons.org/licenses/by-nc/4.0/" rel="license">Creative Commons Attribution-NonCommercial 4.0 International License</a>.

## Lab Goals

It is often necessary to create graphs to effectively communicate key patterns within a dataset. While many software packages allow the user to make basic plots, it can be challenging to create plots that are customized to address a specific idea. While there are numerous ways to create graphs, this tutorial will focus on the R package ggplot2, created by Hadley Wickham.


## Acknowledgements

This lab procedure is adapted from and based on Ryan Miller's ["Creating Graphs with ggplot2"](https://remiller1450.github.io/s230f19/ggplot.html) (Fall 2019, Intro to Data Science STA 230 course, Grinnell College).

# Table of Contents

# ggplot2 and grammar of graphics

## What is `ggplot2`?

“R has several systems for making graphs, but ggplot2 is one of the most elegant and most versatile. ggplot2 implements the grammar of graphics, a coherent system for describing and building graphs. With ggplot2, you can do more faster by learning one system and applying it in many places.” [[Chapter 3 “Data Visualization”](https://r4ds.had.co.nz/data-visualisation.html) in Garrett Grolemund and Hadley Wickham, *R for Data Science*]

“ggplot2 is a system for declaratively creating graphics, based on The Grammar of Graphics. You provide the data, tell ggplot2 how to map variables to aesthetics, what graphical primitives to use, and it takes care of the details...It’s hard to succinctly describe how ggplot2 works because it embodies a deep philosophy of visualisation. However, in most cases you start with ggplot(), supply a dataset and aesthetic mapping (with aes()). You then add on layers (like geom_point() or geom_histogram()), scales (like scale_colour_brewer()), faceting specifications (like facet_wrap()) and coordinate systems (like coord_flip()).” [[ggplot2.tidyverse.org](https://ggplot2.tidyverse.org/)]

## What is the "grammar of graphics"?

“A grammar of graphics is a tool that enables us to concisely describe the components of a graphic. Such a grammar allows us to move beyond named graphics (e.g., the ‘scatterplot’) and gain insight into the deep structure that underlies statistical graphics.”
Developing a grammar allows us to efficiently answer questions like: “What is a graphic? How can we succinctly describe a graphic? And how can we create the graphic that we have described?”

From: Hadley Wickham, “[A Layered Grammar of Graphics](https://vita.had.co.nz/papers/layered-grammar.html)” *Journal of Computational and Graphical Statistics* 19:1 (2010), 3-28.

Which builds on...Leland Wilkinson and Graham Wills, The Grammar of Graphics (Springer, 2005). [Link to electronic access through Hesburgh Libraries](https://onesearch.library.nd.edu/permalink/f/1phik6l/ndu_aleph003079467).

## Basic `ggplot2` syntax

There are two key functions that are used in ggplot2:

qplot()or quick plot is similar to base plotting functions in R and is primarily used to produce quick and easy graphics. We’ve seen qplot a few times and will not cover it in this lab.

ggplot()the grammar of graphics plot is different from other graphics functions because it uses a particular grammar inspired by Leland Wilkinson’s landmark book, The Grammar of Graphics, that focused on thinking about, reasoning with and communicating with graphics. It enables layering of independent components to create custom graphics.

# Data and Environment Setup

Necessary packages: dplyr and ggplot2
```R
# install.packages("dplyr")
# install.packages("ggplot2")
library(dplyr)
library(ggplot2)
```

In this tutorial, we will use the `AmesHousing` data, which provides information on the sales of individual residential properties in Ames, Iowa from 2006 to 2010. 

The data set contains 2930 observations, and a large number of explanatory variables involved in assessing home values. 

[Click here](http://jse.amstat.org/v19n3/decock/DataDocumentation.txt) for a full description of this dataset.

```R
# The csv file should be imported into rstudio:
AmesHousing <- read.csv("https://raw.githubusercontent.com/kwaldenphd/ggplot-intro/main/AmesHousing.csv")
# str(AmesHousing)
```

# Basic Structure of `ggplot` functions

All ggplot functions must have at least three components:
- data frame: In this activity we will be using the AmesHousing data.
- geom: to determine the type of geometric shape used to display the data, such as line, bar, point, or area.
- aes: to determine how variables in the data are mapped to visual properties (aesthetics) of geoms. This can include x position, y position, color, shape, fill, and size.

Thus the simplest code for a graphic made with ggplot() would have one of the the following forms:
- `ggplot(data, aes(x, y)) + geom_line()`
or
- `ggplot(data) + geom_line(aes(x, y))`

These two lines of code provide identical results. 

In the first case, the aes is set for all geoms, meaning the same x and y variables are mapped to any geoms that are added. 

Because more complex graphics can include multiple geoms, it can be advantageous to locally define the aes for each geom as shown in the second line of code.

```R
# Create a histogram of housing prices
ggplot(data=AmesHousing) + geom_histogram(mapping = aes(x = SalePrice))
```

FIGURE 1

In the above code, the terms data=, mapping=, and x= are not required, but are used for clarification. 

For example, the following code will produce identical results: `ggplot(AmesHousing) + geom_histogram(aes(SalePrice))`

```R
# Create a scatterplot of above ground living area by sales price
ggplot(data=AmesHousing) + geom_point(mapping= aes(x=GrLivArea, y=SalePrice))
```

FIGURE 2

Question #1: Open your R Script and add a comment beneath your earlier answers indicating all subsequent work pertains to the ggplot lab.

Beneath this comment, write code that does the following:
 -Creates the same histogram as the example above, but modifies the code so that the aes is not within the geom.
- Creates a scatterplot using ggplot with Fireplaces as the x-axis and SalePrice as the y-axis.

# Customizing Graphics Using `ggplot`

In this section, we layer additional components onto the two graphs shown above.

```R
ggplot(data=AmesHousing) +                         
      geom_histogram(mapping = aes(SalePrice/100000), 
          breaks=seq(0, 7, by = 1), col="red", fill="lightblue") + 
      geom_density(mapping = aes(x=SalePrice/100000, y = (..count..)))  +   
      labs(title="Figure 9: Housing Prices in Ames, Iowa (in $100,000)", 
          x="Sale Price of Individual Homes")   
```

FIGURE 3

The histogram geom transforms the SalePrice, modifies the bin size and changes the color.

geom_density overlays a density curve on top of the histogram.

Typically density curves and histrograms have very different scales, here we use y = (..count..) to modify the density. Alternatively, we could specify aes(x = SalePrice/100000, y = (..density..)) in the histogram geom.

The labs() command adds a title and an x-axis label. A y-axis label can also be added by using `y = " `.

In the code below we create three scatterplots of the log of the above ground living area by the log of sales price.

```R
ggplot(data=AmesHousing, aes(x=log(GrLivArea), y=log(SalePrice)) ) +      
  geom_point(shape = 3, color = "darkgreen") +                                     
  geom_smooth(method=lm,  color="green") +                  
  labs(title="Figure 10: Housing Prices in Ames, Iowa")
```

FIGURE 4

```R
ggplot(data=AmesHousing) + 
  geom_point(aes(x=log(GrLivArea), y=log(SalePrice), color=KitchenQual),shape=2, size=2) + 
  geom_smooth(aes(x=log(GrLivArea), y=log(SalePrice), color=KitchenQual), 
          method=loess, size=1) +                        
  labs(title="Figure 11: Housing Prices in Ames, Iowa") 
```

FIGURE 5

```R
ggplot(data=AmesHousing) +
  geom_point(mapping = aes(x=log(GrLivArea), y=log(SalePrice), color=KitchenQual)) +
  geom_smooth(mapping = aes(x=log(GrLivArea), y=log(SalePrice), color=KitchenQual), 
      method=lm, se=FALSE, fullrange=TRUE) +                             
  facet_grid( ~ Fireplaces) +                      
  labs(title="Figure 12: Housing Prices in Ames, Iowa")
```

FIGURE 6

geom_point is used to create a scatterplot. As shown in Figure 10, multiple shapes can be used as points. The Data Visualization Cheat Sheet lists several shape options

geom_smooth adds a fitted line through the data.
- method=lm specifies a linear regression line. method=loess creates a smooth fit curve
- se=FALSE removes the shaded confidence regions around each line
- fullrange=TRUE extends all regression lines to the same length

facet_grid and facet_wrap commands are used to create multiple plots. In Figure 12, we have created separate scatterplots based upon the number of fireplaces.

When assigning fixed characteristics, (such as color, shape or size), the commands occur outside the aes, as in Figure 10, color="green". 

When characteristics are dependent on the data, the command should occur within the aes, such as in Figure 11 color=Kitchen.Qual.

In the above examples, only a few geoms are listed. The ggplot2 website lists each geom and gives detailed examples of how they are used.

Question #2: Add a comment indicating you’re answering question 2, beneath the comment write code that does the following:
- Creates a scatterplot using YearBuilt as the explanatory variable and SalePrice as the response variable. Include a regression line, a title, and labels for the x and y axes.
- Modifies the scatterplot in Part A by replacing the regression line with a loess smoother, and coloring the points by the overall condition of the home, OverallCond.

# Additional Considerations

## Data Types

It's important to think about how the type of data you are working with shapes choices you make around visualization.

If you use the `str` command after reading data into R, you will notice that each variable is assigned one of the following types: Character, Numeric (real numbers), Integer, Complex, or Logical (TRUE/FALSE). 

In particular, the variable Fireplaces in considered an integer. 

In the code below we try to color and fill a density graph by an integer value. 

Notice that the color and fill commands appear to be ignored in the graph.

```R
# str(AmesHousing)
ggplot(data=AmesHousing) +                   
  geom_density(aes(SalePrice, color = Fireplaces,  fill = Fireplaces))
```

FIGURE 7

In the following code, we use the dplyr package to modify the AmesHousing data.

We first restrict the dataset to only houses with less than three fireplaces and then create a new variable, called Fireplace2. 

The as.factor command creates a factor, which is a variable that contains a set of numeric codes that map to character-valued levels. 

Notice that the color and fill command now work properly.

```R
# Create a new data frame with only houses with less than 3 fireplaces
AmesHousing2 <- filter(AmesHousing, Fireplaces < 3)
# Create a new variable called Fireplace2
AmesHousing2 <-mutate(AmesHousing2,Fireplace2=as.factor(Fireplaces))
#str(AmesHousing2)

ggplot(data=AmesHousing2) +                 
  geom_density(aes(SalePrice, color = Fireplace2,  fill = Fireplace2), alpha = 0.2)
```

FIGURE 8

## Customizing Graphs

In addition to using a data frame, geoms, and aes, several additional components can be added to customize each graph:
- stats
- scales
- themes
- positions
- coordinate systems
- labels
- legends

We will not discuss all of these components here, but the materials in the [Additional Resources](#additional-resources) section provide detailed explanations. 

In the code below we provide a few examples on how to customize graphs.

```R
ggplot(AmesHousing2, aes(x = Fireplace2, y = SalePrice, color = PavedDrive)) +
  geom_boxplot(position = position_dodge(width = 1)) +
  coord_flip()+ 
  labs(title="Housing Prices in Ames, Iowa") +
  theme(plot.title = element_text(family = "Trebuchet MS", color = "blue", face="bold", size=12, hjust=0))
```

FIGURE 9

position is used to address geoms that would take the same space on a graph. 

In the above boxplot, position_dodge(width = 1) adds a space between each box. For scatterplots, position = position_jitter() puts spaces between overlapping points.

theme is used to change the style of a graph, but does not change the data or geoms. 

The above code is used to modify only the title in a boxplot. 

A better approach for beginners is to choose among themes that were created to customize the overall graph. 

Common examples are theme_bw(), theme_classic(), theme_grey(), and theme_minimal(). 

You can also install the ggthemes package for many more options.

Question #3: Add a comment indicating you’re answering question 3, beneath the comment answer the following:
- In the density plot above, explain what the color, fill, and alpha commands are used for. Hint: try running the code with and without these commands or use the Data Visualization Cheat Sheet.
- In the boxplot, what is done by the code coord_flip()?
- Create a new boxplot, similar to the one above, but use theme_bw() instead of the given theme command. Explain how this changes the graph.

# Practice Problems

Question #4: Add a comment to your lab write-up indicating you’re working on question 4. This question requires you to use the dplyr package to manipulate the dataset before making any graphics.
- Restrict the AmesHousing data to only sales under normal conditions. In other words, SaleCondition == "Norm"
- Create a new variable called TotalSqFt = GRLivArea  +  TotalBsmtSF and remove any homes with more than 3000 total square feet.
- With this modified dataset, create a graphic involving no more than three explanatory variables that best illustrates how to predict home’s sale price. For example, Figure 12 uses a linear model of kitchen quality, above ground square footage, and number of fireplaces to predict sale price.

# Additional Resources

Roger Peng, "Plotting with ggplot2" *YouTube* 
- [Part 1](https://youtu.be/HeqHMM4ziXA) (14 October 2013)
- [Part 2](https://youtu.be/n8kYa9vu1l8) (14 October 2013)

[RStudio "Data Visualization With `ggplot2`" cheat sheet](https://www.rstudio.com/wp-content/uploads/2015/03/ggplot2-cheatsheet.pdf)

[`ggplot2` overview](https://ggplot2.tidyverse.org/), Tidyverse documentation

[`ggplot2` package documentation](http://cran.r-project.org/web/packages/ggplot2/ggplot2.pdf), R project

Basel Institute for Clinical Epidemiology and Biostatistics, [`ggplot2` tutorial](http://bbs.ceb-institute.org/wp-content/uploads/2011/09/handout_ggplot2.pdf)

[Questions tagged `ggplot2` on Stackoverflow](https://stackoverflow.com/tags/ggplot2)

Winston Chang, [*R Graphics Cookbook: Practical Recipes for Visualizing Data*](http://www.cookbook-r.com/Graphs/), O'Reilly (2013).

Hadley Wickham, Danielle Navarro, and Thomas Lin Pedersen, [*ggplot2: elegant graphics for data analysis*](https://ggplot2-book.org/), Springer (in-progress 3rd edition).

# Lab Notebook Questions
