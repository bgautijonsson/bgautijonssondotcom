---
title: Hello World!
author: Brynjófur
date: '2020-03-01'
slug: hello-world
categories:
  - R
tags:
  - R Markdown
subtitle: ''
summary: ''
authors: []
lastmod: '2020-03-01T00:22:15Z'
featured: no
image:
  caption: ''
  focal_point: ''
  preview_only: no
projects: []
---



# This is a header


```r
library(tidyverse); library(kableExtra);  library(cowplot); library(scales); library(DT)
theme_set(theme_cowplot() +  
              background_grid(major = "xy", 
                              minor = "xy",
                              size.major = 0.25,
                              size.minor = 0.1) +
              theme(legend.position = "top",
                    plot.margin = margin(10, 10, 10, 10)))
```



```r
ggplot(mtcars, aes(wt, mpg)) +
  geom_smooth() +
  geom_point() +
  labs(title = "This is a plot")
```

<img src="/en/post/2020-03-01-hello-world_files/figure-html/unnamed-chunk-2-1.png" width="100%" />



```r
mtcars %>% 
  group_by(am, cyl) %>% 
  summarise(mpg = mean(mpg)) %>% 
  spread(cyl, mpg) %>% 
  kable("html", caption = "This is a table") %>% 
  kable_styling(bootstrap_options = "striped",position = "center", full_width = F) %>% 
  add_header_above(c("", "Cylinders" = 3))
```

<table class="table table-striped" style="width: auto !important; margin-left: auto; margin-right: auto;">
<caption>Table 1: This is a table</caption>
 <thead>
<tr>
<th style="border-bottom:hidden" colspan="1"></th>
<th style="border-bottom:hidden; padding-bottom:0; padding-left:3px;padding-right:3px;text-align: center; " colspan="3"><div style="border-bottom: 1px solid #ddd; padding-bottom: 5px; ">Cylinders</div></th>
</tr>
  <tr>
   <th style="text-align:right;"> am </th>
   <th style="text-align:right;"> 4 </th>
   <th style="text-align:right;"> 6 </th>
   <th style="text-align:right;"> 8 </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 22.900 </td>
   <td style="text-align:right;"> 19.12500 </td>
   <td style="text-align:right;"> 15.05 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 28.075 </td>
   <td style="text-align:right;"> 20.56667 </td>
   <td style="text-align:right;"> 15.40 </td>
  </tr>
</tbody>
</table>

*This is math:*

$$
e^{i\pi} - 1 = 0
$$
