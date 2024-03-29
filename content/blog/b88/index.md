---
title: "Creating a New ggplot2 Theme"
date: 2018-02-08T05:04:40-06:00
draft: false
tags: ["R"]
output:
  html_document:
    keep_md: yes
---


This post introduces simple steps on how to create your own ggplot2 theme. Of course, it will not go into details of changing all the aspects. Instead, it will highlight the process using few useful elements.

Theme is basically a set of pre-defined(default) values for elements that make up a plot in ggplot2 library. Major elements are:

- Plot, the outermost area used by a plot. Think of this as let's say your A4 sheet on which you want to draw a plot.
- Panel, this is the actual plotting area. This is defined by a rectangle inside the ploting area.
- Legend, another rectangle defining the plot legend. This can be anywhere in the panel, inside or outside of the plot.
- x-axis
- y-axis

### Default Theme
Instead of rewriting properties of all elements, it is more efficient to pick a theme that is closer to what you want and make some adjustments. In my case, I like the `theme_bw` so I will use that as a starting point. A scatter plot on this theme looks like this:



```r
theme_set(theme_bw())
ggplot(mpg, aes(displ, hwy)) + geom_point()
```

![](unnamed-chunk-1-1.png)<!-- -->

### New Theme
I like white background and black border of this theme. Few changes I would like to make to this theme are:

- Remove gridlines. I personally think gridlines do not serve much purpose while doing analysis and are rather a distraction.
- Increase the base font size a bit to be more clear.
- Note that the axis marker font is grey color while the axis title is black. I will change these to black for consistency. 

To make these changes, we will simply 'add' these elements to our base theme:



```r
theme_simple <- function() {
    theme_bw(base_size = 14,
             base_family = "") %+replace%
    theme(panel.grid = element_blank(),
          axis.text = element_text(color = "black"))
}
```

Notice that I have created a new function for this theme instead of just storing it in an object. This makes it flexible since now it can be sourced in any project and used. Let's see how our plot looks now with the new theme.



```r
theme_set(theme_simple())
ggplot(mpg, aes(displ, hwy)) + geom_point()
```

![](unnamed-chunk-3-1.png)<!-- -->

It looks more clear now bringing more attention to the data instead of theme or plot elements. You can find what else can be changed by `?theme`. 

### Further Reading
In case you want to dig deeper into the topic of customizing themes, here are some additional resource:

- [Building a New Theme](https://bookdown.org/rdpeng/RProgDA/building-a-new-theme.html) - From Roger Peng's book.
