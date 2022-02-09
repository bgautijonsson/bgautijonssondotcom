---
title: Forest Plots with Built-In Tables
author: Brynjólfur Gauti Guðrúnar Jónsson
output:
    html_document:
        keep_md: true
        toc: true
        toc_float: true
date: '2022-02-09'
slug: []
categories:
  - R
tags:
  - rstudio
  - ggplot
  - tables
---
<script src="{{< blogdown/postref >}}index_files/kePrint/kePrint.js"></script>
<link href="{{< blogdown/postref >}}index_files/lightable/lightable.css" rel="stylesheet" />





I've seen this kind of plot poking around, but I didn't really think about them until the other day I was asked about how to make them. Here I will walk through making one of these plots using the `ggplot2` and `cowplot` packages.

To start with I have some fake data, `d`.


```r
d |> 
    kable(format = "html") |> 
    kable_styling(full_width = F)
```

<table class="table" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> name </th>
   <th style="text-align:right;"> mean </th>
   <th style="text-align:right;"> lower </th>
   <th style="text-align:right;"> upper </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Covfefe </td>
   <td style="text-align:right;"> 1.1989430 </td>
   <td style="text-align:right;"> 1.0707825 </td>
   <td style="text-align:right;"> 1.358700 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Coffee </td>
   <td style="text-align:right;"> 1.8509518 </td>
   <td style="text-align:right;"> 1.5589701 </td>
   <td style="text-align:right;"> 2.227752 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Variable </td>
   <td style="text-align:right;"> 1.2181512 </td>
   <td style="text-align:right;"> 1.0834045 </td>
   <td style="text-align:right;"> 1.411683 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Covariate </td>
   <td style="text-align:right;"> 1.1970336 </td>
   <td style="text-align:right;"> 1.0699671 </td>
   <td style="text-align:right;"> 1.342737 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Predictor </td>
   <td style="text-align:right;"> 0.9085823 </td>
   <td style="text-align:right;"> 0.8049225 </td>
   <td style="text-align:right;"> 1.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Smoking </td>
   <td style="text-align:right;"> 0.8770238 </td>
   <td style="text-align:right;"> 0.7447709 </td>
   <td style="text-align:right;"> 1.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Age </td>
   <td style="text-align:right;"> 0.9344551 </td>
   <td style="text-align:right;"> 0.8305984 </td>
   <td style="text-align:right;"> 1.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Uranium </td>
   <td style="text-align:right;"> 0.9177338 </td>
   <td style="text-align:right;"> 0.8021060 </td>
   <td style="text-align:right;"> 1.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Stuff </td>
   <td style="text-align:right;"> 1.0760010 </td>
   <td style="text-align:right;"> 1.0000000 </td>
   <td style="text-align:right;"> 1.216474 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thing </td>
   <td style="text-align:right;"> 0.9143605 </td>
   <td style="text-align:right;"> 0.7753231 </td>
   <td style="text-align:right;"> 1.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Koffing </td>
   <td style="text-align:right;"> 1.0686924 </td>
   <td style="text-align:right;"> 1.0000000 </td>
   <td style="text-align:right;"> 1.178642 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Coughing </td>
   <td style="text-align:right;"> 1.0648236 </td>
   <td style="text-align:right;"> 1.0000000 </td>
   <td style="text-align:right;"> 1.194998 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Polvo </td>
   <td style="text-align:right;"> 0.9227277 </td>
   <td style="text-align:right;"> 0.8097846 </td>
   <td style="text-align:right;"> 1.000000 </td>
  </tr>
</tbody>
</table>


### The Forest Plot

The forest plot is not hard to do.


```r
p1 <- d |> 
    mutate(name = fct_reorder(name, mean)) |> 
    ggplot(aes(x = name, y = mean,
                  ymin = lower, ymax = upper)) +
    geom_vline(aes(xintercept = name), col = "grey95", size = 5) +
    geom_hline(yintercept = 1, lty = 2) +
    geom_point() +
    geom_linerange() +
    coord_flip() +
    labs(x = NULL, y = NULL) +
    theme(plot.margin = margin(t = 5, r = -4, b = 5, l = 5))

p1
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-4-1.png" width="100%" />

### The Table

To make the table look nice I convert the numbers to text and clean them up so that the text is justified nicely when plotting it


```r
p2 <- d |> 
    mutate(name = fct_reorder(name, mean),
           mean = round(mean, 2),
           mean = str_pad(mean, width = 4, side = "right", pad = "0"),
           lower = round(lower, 2),
           lower = as.character(lower),
           lower = ifelse(lower == "1", "1.00", lower),
           lower = str_pad(lower, width = 4, side = "right", pad = "0"),
           upper = round(upper, 2),
           upper = as.character(upper),
           upper = ifelse(upper == "1", "1.00", upper),
           upper = str_pad(upper, width = 4, side = "right", pad = "0"),
           ci = str_c(lower, ", ", upper)) |> 
    ggplot(aes(x = name)) +
    geom_vline(aes(xintercept = name), col = "grey95", size = 5) +
    geom_text(aes(label = mean, y = 1)) +
    geom_text(aes(label = ci, y = 1.07)) +
    coord_flip(ylim = c(0.99, 1.1)) +
    theme(axis.title = element_blank(),
          axis.text = element_blank(),
          axis.ticks = element_blank(),
          axis.line.y = element_blank(),
          panel.border = element_blank(), 
          panel.grid = element_blank(), 
          plot.background = element_blank(),
          plot.margin = margin(t = 5, r = 5, b = 19.3, l = 0))

p2
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-5-1.png" width="100%" />

#### The finished bottom row

Now we have nicely alligned the plot and table. 


```r
bottom_row <- plot_grid(p1, p2, nrow = 1, rel_widths = c(1, 0.3)) 

bottom_row
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-6-1.png" width="100%" />

### The headers

I admit that this is a pretty handwavy manual way to make the header fit, and there is probably a nice way to automatically fit this using the coordinate system. 


```r
top_row <- ggplot(data = tibble()) +
    geom_text(aes(y = 1, x = 0.02, label = "Variable"), size = 5) +
    geom_text(aes(y = 1, x = 0.84, label = "Mean"), size = 5) +
    geom_text(aes(y = 1, x = 0.98, label = "95% CI"), size = 5) +
    coord_cartesian(xlim = c(0, 1)) +
    theme_void() +
    theme(plot.margin = margin(t = 0, r = 5, b = 0, l = 5),
          axis.line.x.bottom = element_line())


top_row
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-7-1.png" width="100%" />


### Putting it all together

And so we come to the finished plot. If we wrangle the `rel_heights` setting a little bit we can get a pretty nice looking forest plot and table hybrid.


```r
plot_grid(top_row, bottom_row, ncol = 1, rel_heights = c(0.07, 1))
```

<img src="{{< blogdown/postref >}}index_files/figure-html/finished_plot-1.png" width="100%" />

This was actually less of an inconvenience than I thought. The mixture of `ggplot2` and `cowplot` made this a pretty easy task.
