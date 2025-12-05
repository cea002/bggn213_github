---
author:
- "Courtney Anderson (PID:A69038035)"
authors:
- "Courtney Anderson (PID:A69038035)"
title: "Class 05: Data Visualization with GGPLOT"
toc-title: Table of contents
---

Today we are playing with plotting and graphics in R.

There are lots of ways to make cool figures in R. There is "base" E
graphics (`plot()`,``` hist()``boxplot() ```)

There is also add-on packages, like **ggplot**

:::: cell
``` {.r .cell-code}
head(cars)
```

::: {.cell-output .cell-output-stdout}
      speed dist
    1     4    2
    2     4   10
    3     7    4
    4     7   22
    5     8   16
    6     9   10
:::
::::

:::: cell
``` {.r .cell-code}
plot(cars)
```

::: cell-output-display
![](Class05_files/figure-markdown/unnamed-chunk-2-1.png)
:::
::::

:::: cell
``` {.r .cell-code}
hist(mtcars$mpg)
```

::: cell-output-display
![](Class05_files/figure-markdown/unnamed-chunk-3-1.png)
:::
::::

:::: cell
``` {.r .cell-code}
head(mtcars)
```

::: {.cell-output .cell-output-stdout}
                       mpg cyl disp  hp drat    wt  qsec vs am gear carb
    Mazda RX4         21.0   6  160 110 3.90 2.620 16.46  0  1    4    4
    Mazda RX4 Wag     21.0   6  160 110 3.90 2.875 17.02  0  1    4    4
    Datsun 710        22.8   4  108  93 3.85 2.320 18.61  1  1    4    1
    Hornet 4 Drive    21.4   6  258 110 3.08 3.215 19.44  1  0    3    1
    Hornet Sportabout 18.7   8  360 175 3.15 3.440 17.02  0  0    3    2
    Valiant           18.1   6  225 105 2.76 3.460 20.22  1  0    3    1
:::
::::

Lets plot `mpg` vs `disp`

:::: cell
``` {.r .cell-code}
plot(mtcars$mpg, mtcars$disp)
```

::: cell-output-display
![](Class05_files/figure-markdown/unnamed-chunk-5-1.png)
:::
::::

##GGPLOT

The main function in the GGplot package is `ggplot()`. First I need to
install the **ggplot2** package. I can install any package with the
function `install packages()`

> **N.B** I never want to run `install.packages()` in my quarto source
> document!!!

::::: cell
``` {.r .cell-code}
library(ggplot2)
```

::: {.cell-output .cell-output-stderr}
    Warning: package 'ggplot2' was built under R version 4.5.2
:::

``` {.r .cell-code}
ggplot(cars) + 
  aes(x=speed, y=dist) +
  geom_point()
```

::: cell-output-display
![](Class05_files/figure-markdown/unnamed-chunk-6-1.png)
:::
:::::

::::: cell
``` {.r .cell-code}
library(ggplot2)
ggplot(cars) + 
  aes(x=speed) +
  geom_histogram()
```

::: {.cell-output .cell-output-stderr}
    `stat_bin()` using `bins = 30`. Pick better value `binwidth`.
:::

::: cell-output-display
![](Class05_files/figure-markdown/unnamed-chunk-7-1.png)
:::
:::::

`ggplot()` selects the data `aes()` selects the x and y variabales
(aesthetic) `geom_()` gives you the type of graph (you can mutiple geoms
to add components to your graphs

###Adding more layers

Lets add a line and a title, subtitle and caption as well as axis labels

:::: cell
``` {.r .cell-code}
ggplot(cars) + 
  aes(x=speed, y=dist) +
  geom_line()
```

::: cell-output-display
![](Class05_files/figure-markdown/unnamed-chunk-8-1.png)
:::
::::

::::: cell
``` {.r .cell-code}
ggplot(cars) + 
  aes(x=speed, y=dist) +
  geom_point()+
  geom_smooth(method='lm', se=FALSE) +
  labs(title="Silly Plot", 
       x="Speed (MPH)", 
       y="Distance (ft)",
  caption= "When is tea time anyway??") +
theme_bw()
```

::: {.cell-output .cell-output-stderr}
    `geom_smooth()` using formula = 'y ~ x'
:::

::: cell-output-display
![](Class05_files/figure-markdown/unnamed-chunk-9-1.png)
:::
:::::

##Plot some expression data

Read data file from online URL

:::: cell
``` {.r .cell-code}
url <- "https://bioboot.github.io/bimm143_S20/class-material/up_down_expression.txt"
genes <- read.delim(url)
head(genes)
```

::: {.cell-output .cell-output-stdout}
            Gene Condition1 Condition2      State
    1      A4GNT -3.6808610 -3.4401355 unchanging
    2       AAAS  4.5479580  4.3864126 unchanging
    3      AASDH  3.7190695  3.4787276 unchanging
    4       AATF  5.0784720  5.0151916 unchanging
    5       AATK  0.4711421  0.5598642 unchanging
    6 AB015752.4 -3.6808610 -3.5921390 unchanging
:::
::::

> Q.1 How many genes are in this wee dataset?

There are 5196 in this dataset

> Q.2 How many genes are upregualted?

:::: cell
``` {.r .cell-code}
sum(genes$State=="up")
```

::: {.cell-output .cell-output-stdout}
    [1] 127
:::
::::

There are in this dataset

:::: cell
``` {.r .cell-code}
table(genes$State)
```

::: {.cell-output .cell-output-stdout}

          down unchanging         up 
            72       4997        127 
:::
::::

::: cell
``` {.r .cell-code}
library(ggrepel)

p <- ggplot(genes)+
  aes(x=Condition1, 
      y=Condition2, 
      col=State, 
      label=Gene)+
  geom_point() +
  scale_color_manual(values=c("blue","grey", "red"))+
  geom_text_repel(max.overlaps=10)+
  theme_bw()
```
:::

::::: cell
``` {.r .cell-code}
p+ labs(title= "look at me")
```

::: {.cell-output .cell-output-stderr}
    Warning: ggrepel: 5195 unlabeled data points (too many overlaps). Consider
    increasing max.overlaps
:::

::: cell-output-display
![](Class05_files/figure-markdown/unnamed-chunk-14-1.png)
:::
:::::

::: cell
``` {.r .cell-code}
# File location online
url <- "https://raw.githubusercontent.com/jennybc/gapminder/master/inst/extdata/gapminder.tsv"

gapminder <- read.delim(url)
```
:::

:::: cell
``` {.r .cell-code}
head(gapminder)
```

::: {.cell-output .cell-output-stdout}
          country continent year lifeExp      pop gdpPercap
    1 Afghanistan      Asia 1952  28.801  8425333  779.4453
    2 Afghanistan      Asia 1957  30.332  9240934  820.8530
    3 Afghanistan      Asia 1962  31.997 10267083  853.1007
    4 Afghanistan      Asia 1967  34.020 11537966  836.1971
    5 Afghanistan      Asia 1972  36.088 13079460  739.9811
    6 Afghanistan      Asia 1977  38.438 14880372  786.1134
:::
::::

:::: cell
``` {.r .cell-code}
tail(gapminder)
```

::: {.cell-output .cell-output-stdout}
          country continent year lifeExp      pop gdpPercap
    1699 Zimbabwe    Africa 1982  60.363  7636524  788.8550
    1700 Zimbabwe    Africa 1987  62.351  9216418  706.1573
    1701 Zimbabwe    Africa 1992  60.377 10704340  693.4208
    1702 Zimbabwe    Africa 1997  46.809 11404948  792.4500
    1703 Zimbabwe    Africa 2002  39.989 11926563  672.0386
    1704 Zimbabwe    Africa 2007  43.487 12311143  469.7093
:::
::::

A first plot

:::: cell
``` {.r .cell-code}
ggplot(gapminder) +
  aes(y=lifeExp,x=gdpPercap, col=continent, size=pop)+
  geom_point(alpha=0.5)+
  facet_wrap(~continent)+
  theme_bw()
```

::: cell-output-display
![](Class05_files/figure-markdown/unnamed-chunk-18-1.png)
:::
::::
