---
title: Styling Line Graphs
parent: Presentation
has_children: false
nav_order: 1
---

# Styling Line Graphs 

There are several ways of styling line graphs. The following examples demonstrate how to modify the appearances of the lines 
(type and sizes), as well chart titles and axes labels.   

## Keep in Mind
- To get started on how to plot line graphs, see [here](https://lost-stats.github.io/Presentation/line_graph_with_labels_at_the_beginning_or_end.html).
- Elements for customization include line thickness, line type (solid, dashed, etc.), shade, transparency, and color.
- Color is one of the easiest ways to distinguish a large number of line graphs. If you have many line graphs overlaid and have to use black-and-white, consider different shades of black/gray.

# Implementation 

## R
```{r}
## If necessary
## install.packages(c('ggplot2','cowplot'))
## load packages
library(ggplot2)
## Cowplot is just to join together the four graphs at the end
library(cowplot)

## load data (the Economics dataset comes with ggplot2) 
eco_df <- economics

## basic plot
p1 <- ggplot() +
  geom_line(aes(x=date, y = uempmed), data = eco_df)
p1

## Change line color and chart labels 
## Note here that color is *outside* of the aes() argument, and so this will color the line
## If color were instead *inside* aes() and set to a factor variable, ggplot would create
## a different line for each value of the factor variable, colored differently.
p2 <- ggplot() +
  ## choose a color of preference
  geom_line(aes(x=date, y = uempmed), color = "navyblue", data = eco_df) +
  ## add chart title and change axes labels
  labs(
    title = "Median Duration of Unemployment", 
    x = "Date",
    y = "") +
  ## Add a ggplot theme
  theme_light()
  ## center the chart title
  theme(plot.title = element_text(hjust = 0.5)) +

p2

## plotting multiple charts (of different line types and sizes)
p3 <-ggplot() +
  geom_line(aes(x=date, y = uempmed), color = "navyblue", 
            size = 1.5, data = eco_df) +
  geom_line(aes(x=date, y = psavert), color = "red2", 
            linetype = "dotted", size = 0.8, data = eco_df) +
  labs(
    title = "Unemployment Duration (Blue) and Savings Rate (Red)", 
    x = "Date",
    y = "") +
  theme_light() +
  theme(plot.title = element_text(hjust = 0.5))
  
p3

## Plotting a different line type for each group
## There isn't a natural factor in this data so let's just duplicate the data and make one up
eco_df$fac <- factor(1, levels = c(1,2))
eco_df2 <- eco_df
eco_df2$fac <- 2
eco_df2$uempmed <- eco_df2$uempmed - 2 + rnorm(nrow(eco_df2))
eco_df <- rbind(eco_df, eco_df2)
p4 <- ggplot() +
  ## This time, color goes inside aes
  geom_line(aes(x=date, y = uempmed, color = fac), data = eco_df) +
  ## add chart title and change axes labels
  labs(
    title = "Median Duration of Unemployment", 
    x = "Date",
    y = "") +
  ## Add a ggplot theme
  theme_light() +
  ## center the chart title
  theme(plot.title = element_text(hjust = 0.5),
        ## Move the legend onto some blank space on the diagram
        legend.position = c(.25,.8),
        ## And put a box around it
        legend.background = element_rect(color="black")) +
  ## Retitle the legend that pops up to explain the discrete (factor) difference in colors
  ## (note if we just want a name change we could do guides(color = guide_legend(title = 'Random Factor')) instead)
  scale_color_manual(name = "Random Factor",
                     # And specify the colors for the factor levels (1 and 2) by hand if we like
                     values = c("1" = "red", "2" = "blue"))
p4

# Put them all together with cowplot for LOST upload
plot_grid(p1,p2,p3,p4, nrow=2)
```
The four plots generated by the code are (in order p1, p2, then p3 and p4):

![Four Styled Line Graphs in R](https://github.com/LOST-STATS/LOST-STATS.github.io/raw/master/Presentation/Images/styling_line_graphs/styling_line_graphs_R.png)
