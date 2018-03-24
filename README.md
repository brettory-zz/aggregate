---
title: "Aggregating across columns: data.table, aggregate, dplyr"
# author: "Brett Ory"
thumbnailImagePosition: left
thumbnailImage: https://lh3.googleusercontent.com/AOOKqHIbVtLYDjVctWRHzycWgJTFalnGqVLp0zjiDCwXrfvW-PV_cC7j4IhLd_LRsCHXC9xiuPzJYL1BNDHYCsIKuj0kUrT3i_fDlahkMbJrYEaxy9WKg2oRfoMp8o_0l5Asg1RxzqR8h4RUbH1FvHa1X_lQ4wXObYr0ipWbW_FpOtMn-KGtfwQkg9X2D93KrCKu8bjy2WaCTqJNq-Od-8Cfz7WunVKvnu7S1oAeFRhLEoqRGFlmKhJ4UJ8jOrIxOtWPLE1h9NaYzgmCPEsGJnDn7eyFHTZ3b-ThubEKrDGXFme-n7iOSMaGA0-szUwWkpK38TtrhoXEe_yLeeAxWNDURvR-RBrAp-60H_ra1asU_4e9n_iUpF-zw7LnqYA7lSWI8MC-7ssg8sy7PWG40Ms7SzKuKUSr9r8tGPBQNCBLivmFt7rSpnTOOTuMwep58i2xSZtYkFgaQ_RD_mF8jLNYID_6JcPWYTEJot609_CZcA-2wdL7UGWr5ObangcnmI9xQu2cyLlgHm_9ORJ3HAwl_fjYX-1FCpu-lYc6MqDYy5kX6CRPNQ_1Y7j1h1lDlkRBIaVqeFHrmt8_dFFyl1KnCEWEu-tUP1G0gY86=w675-h506-no
coverImage: https://lh3.googleusercontent.com/AOOKqHIbVtLYDjVctWRHzycWgJTFalnGqVLp0zjiDCwXrfvW-PV_cC7j4IhLd_LRsCHXC9xiuPzJYL1BNDHYCsIKuj0kUrT3i_fDlahkMbJrYEaxy9WKg2oRfoMp8o_0l5Asg1RxzqR8h4RUbH1FvHa1X_lQ4wXObYr0ipWbW_FpOtMn-KGtfwQkg9X2D93KrCKu8bjy2WaCTqJNq-Od-8Cfz7WunVKvnu7S1oAeFRhLEoqRGFlmKhJ4UJ8jOrIxOtWPLE1h9NaYzgmCPEsGJnDn7eyFHTZ3b-ThubEKrDGXFme-n7iOSMaGA0-szUwWkpK38TtrhoXEe_yLeeAxWNDURvR-RBrAp-60H_ra1asU_4e9n_iUpF-zw7LnqYA7lSWI8MC-7ssg8sy7PWG40Ms7SzKuKUSr9r8tGPBQNCBLivmFt7rSpnTOOTuMwep58i2xSZtYkFgaQ_RD_mF8jLNYID_6JcPWYTEJot609_CZcA-2wdL7UGWr5ObangcnmI9xQu2cyLlgHm_9ORJ3HAwl_fjYX-1FCpu-lYc6MqDYy5kX6CRPNQ_1Y7j1h1lDlkRBIaVqeFHrmt8_dFFyl1KnCEWEu-tUP1G0gY86=w675-h506-no
metaAlignment: center
coverMeta: out
date: 2018-03-23T21:13:14-05:00
categories: ["Personal projects"]
tags: ["R", "dplyr", "data.table", "aggregate", "summary statistics"]
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

This week I'm not doing a data analysis project so much as a data cleaning project. One of the most common problems I come across in data cleaning is how to get summary statistics for various groups in the data. And it's also one of the most annoying problems, because I invariably forget how to do it, and end up having to go back to old code to copy and paste. 

<br>

## Data.table, base R, dplyr 

At various times I have used the data.table package, base R "aggregate", or the dplyr package and let me voice my favor for dplyr at the beginning. It's very intuitive and works just as well as the other methods. Like the Dutch cleaning product brand HG, dplyr "doet wat het belooft" (Does what it promises). In this post I'm going to outline and evaluate each of the methods.     



![](https://lh3.googleusercontent.com/4Hin0y1GkfoZN5doyVR0ZiiABS0tE_vUjHRGbARF1fsLb8K7IazuPagWIE42dRRG_348kh4EFKkzOt5MFq6TGnMkKwV2i2JOqvQmDVuC9RKXNX-VWXh1OgLzF9oXaovwTLjALV58cz9KYBR7440gVzY4iSStc_ZB8hTIXvwXqw20FjN4CccYMWSgluNSej7N2Tha9lKRperfYqzoBJxltZOm1V5d3V-91_4Ckj--TzdOO60d0jMZEgYkBlfCdtW8lQPE5YhxIEmUdQrInI8PrHrQUMh4KuaYObdYkUAGqRDWDlWAOZWzYbUAS6unx8crIMDe4L5POHl5ru3C-ct-8xz_DzlVE6Dt2CpiNIe4GZlPvy7mv2gF-AtGWWxFLdpVqyCjbpoOKT_0GxzZjWzTPCacSF2BROdAarqBYDyU1e_HIKisaOAN0pGJqUDrX3zB3MGddOOuLV7stbWkunnCn59642ZApJE3RO-832i9MPXmhgZ9PZ8TOQ8TdB2L05DgKrsCd1SGrN1SX1dB0SlDMP7tr75bWJ8MYRefUP0e6nEHW6sDKhwfzO50c0EutOvtLQYDAf4U8zd6yFP3UYDTzFn252KFmQFa7G--BXcZ=w275-h183-no)

<br>

## The data

For this project I'm going to use the [npk](https://stat.ethz.ch/R-manual/R-devel/library/datasets/html/npk.html) data from the R Datasets package. It's a dataframe of 24 observations and 5 variables measuring the results of an experiment of how nitrogen (N), phosphate (P), and potassium (K) affect the crop yield of peas. This dataset is useful for today's project because data is grouped into block (1-6), or we could aggregate by whether crops were exposed to nitrogen, phosphate and/or potassium. The yield of peas (outcome variable) is in units of pounds per plot, so more is better. 

Load the data
```{r}
npk <- as.data.frame(npk)
str(npk)
```

I will now proceed to take the average yield per block and add that to the dataset as a column. 

<br>

### "mean" 

First things first, if all we want to know if the mean yield for all pea crops, we can do this very easily with "mean" 

```{r}
Mean <- mean(npk$yield, na.rm=T) 
Mean
```

But usually, this is not very useful because what you actually want to know is the crop yield _per_ block.

<br>

### "data.table"

The very first way I learned to do this is somewhat cumbersome. It involved using the data.table package to create a data table of aggregated statistics, and then merging it (or not) with the original data.

```{r, warning=F, error=F, message=F}
# load the package
library(data.table)

# use data.table to transform the data frame to a data table. Careful, this command with rename your variables to V1, V2, ... Vn
npk_datatable <- data.table(npk$block, npk$yield)
# rename variables
npk_datatable$block <- npk_datatable$V1
npk_datatable$yield <- npk_datatable$V2
npk_datatable <- npk_datatable[,c("block","yield")]

# create an aggregated data set of the mean of "yield" per "block"
mean_datatable <- npk_datatable[, mean(yield, na.rm=T), by=block] 
mean_datatable
```

The variable is renamed from yield to V1. Aggravating, but there's nothing to do but change the name:

```{r}
mean_datatable$mean_yield <- mean_datatable$V1
(mean_datatable <- mean_datatable[, c("block","mean_yield")])
```

<br>

### "aggregate": Base R

You can do the exact same thing in base R using the aggregate command:

```{r}
npk_aggregate <- aggregate(npk[, 5], list(npk$block), mean, na.rm=T)
npk_aggregate
```
Arg! I can feel my blood pressure rising every time R changes the names of my variables. Block is now Group.1 and mean_yield is x. Change names again:

```{r}
npk_aggregate$mean_yield <- npk_aggregate$x
npk_aggregate$block <- npk_aggregate$Group.1
(npk_aggregate <- npk_aggregate[, c("block","mean_yield")])
```

These methods both work, but trying to remember them is about as Aggravating as trying to teach my grandma how to double click. Which is to say: very. Enter: dplyr and tidyverse to save the day!


<br>

### "dplyr"

I started out writing this post not entirely convinced that dplyr is actually better than aggregate. But in the process of writing, I'm convinced. The syntax is just so readable and seems to make intuitive sense. Reproducing it for this post was super easy.

```{r, warning=F, error=F, message=F}
library(dplyr)
npk_dplyr <- npk %>%
  group_by(block) %>%
  summarize(mean(yield, na.rm=T))
npk_dplyr

```



<br>

### "merge"

With all of these methods, if we want to append the aggregated information to the original data, we can do so using "merge". For example:
```{r}
npk_datatable <- merge(npk, mean_datatable, intersect(names(npk), names(mean_datatable)))
head(npk_datatable)
```

And that's it! A brief illustration of the differences between data.table, base R, and dplyr for summarizing aggregated data. 

This blog post can be found on [GitHub](https://github.com/brettory/aggregate).






