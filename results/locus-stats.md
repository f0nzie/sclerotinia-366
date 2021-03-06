---
title: "Locus Statistics"
output: 
  html_notebook:
    toc: true
editor_options: 
  chunk_output_type: inline
---



# Purpose

This will tabulate statistics per locus for presentation in a table.


```r
library("tidyverse")
library("poppr")
load(file.path(PROJHOME, "data", "sclerotinia_16_loci.rda"))
dir.create(file.path(PROJHOME, "results", "tables"))
```


```r
makerange <- . %>% as.integer() %>% range() %>% paste(collapse = "--")

ranges <- map_chr(alleles(dat11), makerange)
# ranges <- c(ranges, alleles(dat11) %>% unlist(use.names = FALSE) %>% makerange)
locus_table(dat11, information = FALSE) %>% 
  as.data.frame() %>%
  rownames_to_column("Locus") %>%
  slice(-n()) %>%
  mutate(Locus = gsub("\\([FH]\\)", "", Locus)) %>%
  add_column(Range = ranges) %>%
  add_column(Repeats = other(dat11)$REPLEN[locNames(dat11)]) %>%
  select(Locus, Range, Repeats, allele, Hexp, Evenness) %>%
  readr::write_csv(file.path(PROJHOME, "results/tables/locus-table.csv"), col_names = TRUE) %>%
  mutate(allele = round(allele, 2)) %>%
  mutate(Repeats = case_when(
    Repeats <= 2 ~ "di-",
    Repeats <= 3 ~ "tri-",
    Repeats <= 4 ~ "tetra-",
    Repeats <= 5 ~ "penta-",
    Repeats <= 6 ~ "hexa-"
  )) %>%
  rename(`*h*` = Hexp) %>%
  rename(`No. alleles` = allele) %>%
  rename(`Repeat Motif` = Repeats) %>%
  huxtable::as_huxtable(add_colnames = TRUE) %>%
  huxtable::set_number_format(huxtable::everywhere, 3:4, 0) %>%
  huxtable::set_number_format(huxtable::everywhere, 5:6, 2) %>%
  huxtable::set_align(huxtable::everywhere, 2:6, "right") %>%
  huxtable::set_col_width(c(0.06, 0.07, 0.11, 0.1, 0.05, 0.075)) %>%
  huxtable::print_md(max_width = 61)
```

```
------------------------------------------------------------
Locus      Range  Repeat Motif  No. alleles    *h*  Evenness 
------- -------- ------------- ------------ ------ ---------
5-2     318--324           di-            4   0.45      0.62 

6-2     483--495         hexa-            3   0.64      0.95 

7-2     158--174           di-            7   0.73      0.76 

8-3     244--270           di-            7   0.74      0.79 

9-2     360--382           di-            9   0.35      0.41 

12-2    214--222           di-            5   0.58      0.78 

17-3    342--363          tri-            7   0.55      0.53 

20-3    280--282           di-            2   0.05      0.42 

55-4    153--216        tetra-           10   0.72      0.66 

110-4   370--386        tetra-            5   0.76      0.91 

114-4   339--416        tetra-           10   0.83      0.80 

------------------------------------------------------------
```

<details>
<summary>Session Information</summary>


```
## Session info --------------------------------------------------------------------------------------
```

```
##  setting  value                       
##  version  R version 3.4.1 (2017-06-30)
##  system   x86_64, linux-gnu           
##  ui       X11                         
##  language (EN)                        
##  collate  en_US.UTF-8                 
##  tz       UTC                         
##  date     2017-09-19
```

```
## Packages ------------------------------------------------------------------------------------------
```

```
##  package     * version date       source                                  
##  ade4        * 1.7-8   2017-08-09 CRAN (R 3.4.1)                          
##  adegenet    * 2.1.0   2017-09-19 Github (thibautjombart/adegenet@8bc0ae0)
##  ape           4.1     2017-02-14 CRAN (R 3.4.1)                          
##  assertr       2.0.2.2 2017-06-06 CRAN (R 3.4.1)                          
##  assertthat    0.2.0   2017-04-11 CRAN (R 3.4.1)                          
##  base        * 3.4.1   2017-09-19 local                                   
##  bindr         0.1     2016-11-13 CRAN (R 3.4.1)                          
##  bindrcpp    * 0.2     2017-06-17 CRAN (R 3.4.1)                          
##  boot          1.3-20  2017-07-30 CRAN (R 3.4.1)                          
##  broom         0.4.2   2017-02-13 CRAN (R 3.4.1)                          
##  cellranger    1.1.0   2016-07-27 CRAN (R 3.4.1)                          
##  cluster       2.0.6   2017-03-16 CRAN (R 3.4.1)                          
##  coda          0.19-1  2016-12-08 CRAN (R 3.4.1)                          
##  colorspace    1.3-2   2016-12-14 CRAN (R 3.4.1)                          
##  compiler      3.4.1   2017-09-19 local                                   
##  datasets    * 3.4.1   2017-09-19 local                                   
##  deldir        0.1-14  2017-04-22 CRAN (R 3.4.1)                          
##  devtools      1.13.3  2017-08-02 CRAN (R 3.4.1)                          
##  digest        0.6.12  2017-01-27 CRAN (R 3.4.1)                          
##  dplyr       * 0.7.3   2017-09-09 CRAN (R 3.4.1)                          
##  evaluate      0.10.1  2017-06-24 CRAN (R 3.4.1)                          
##  expm          0.999-2 2017-03-29 CRAN (R 3.4.1)                          
##  ezknitr       0.6     2016-09-16 CRAN (R 3.4.1)                          
##  fastmatch     1.1-0   2017-01-28 CRAN (R 3.4.1)                          
##  forcats       0.2.0   2017-01-23 CRAN (R 3.4.1)                          
##  foreign       0.8-69  2017-06-21 CRAN (R 3.4.1)                          
##  gdata         2.18.0  2017-06-06 CRAN (R 3.4.1)                          
##  ggplot2     * 2.2.1   2016-12-30 CRAN (R 3.4.1)                          
##  glue          1.1.1   2017-06-21 CRAN (R 3.4.1)                          
##  gmodels       2.16.2  2015-07-22 CRAN (R 3.4.1)                          
##  graphics    * 3.4.1   2017-09-19 local                                   
##  grDevices   * 3.4.1   2017-09-19 local                                   
##  grid          3.4.1   2017-09-19 local                                   
##  gtable        0.2.0   2016-02-26 CRAN (R 3.4.1)                          
##  gtools        3.5.0   2015-05-29 CRAN (R 3.4.1)                          
##  haven         1.1.0   2017-07-09 CRAN (R 3.4.1)                          
##  hms           0.3     2016-11-22 CRAN (R 3.4.1)                          
##  htmltools     0.3.6   2017-04-28 CRAN (R 3.4.1)                          
##  httpuv        1.3.5   2017-07-04 CRAN (R 3.4.1)                          
##  httr          1.3.1   2017-08-20 CRAN (R 3.4.1)                          
##  huxtable      0.3.1   2017-09-12 CRAN (R 3.4.1)                          
##  igraph        1.1.2   2017-07-21 CRAN (R 3.4.1)                          
##  jsonlite      1.5     2017-06-01 CRAN (R 3.4.1)                          
##  knitr       * 1.17    2017-08-10 CRAN (R 3.4.1)                          
##  lattice       0.20-35 2017-03-25 CRAN (R 3.4.1)                          
##  lazyeval      0.2.0   2016-06-12 CRAN (R 3.4.1)                          
##  LearnBayes    2.15    2014-05-29 CRAN (R 3.4.1)                          
##  lubridate     1.6.0   2016-09-13 CRAN (R 3.4.1)                          
##  magrittr      1.5     2014-11-22 CRAN (R 3.4.1)                          
##  MASS          7.3-47  2017-04-21 CRAN (R 3.4.1)                          
##  Matrix        1.2-11  2017-08-16 CRAN (R 3.4.1)                          
##  memoise       1.1.0   2017-04-21 CRAN (R 3.4.1)                          
##  methods     * 3.4.1   2017-09-19 local                                   
##  mgcv          1.8-21  2017-09-17 CRAN (R 3.4.1)                          
##  mime          0.5     2016-07-07 CRAN (R 3.4.1)                          
##  mnormt        1.5-5   2016-10-15 CRAN (R 3.4.1)                          
##  modelr        0.1.1   2017-07-24 CRAN (R 3.4.1)                          
##  munsell       0.4.3   2016-02-13 CRAN (R 3.4.1)                          
##  nlme          3.1-131 2017-02-06 CRAN (R 3.4.1)                          
##  parallel      3.4.1   2017-09-19 local                                   
##  pegas         0.10    2017-05-03 CRAN (R 3.4.1)                          
##  permute       0.9-4   2016-09-09 CRAN (R 3.4.1)                          
##  phangorn      2.2.0   2017-04-03 CRAN (R 3.4.1)                          
##  pkgconfig     2.0.1   2017-03-21 CRAN (R 3.4.1)                          
##  plyr          1.8.4   2016-06-08 CRAN (R 3.4.1)                          
##  poppr       * 2.5.0   2017-09-11 CRAN (R 3.4.1)                          
##  psych         1.7.8   2017-09-09 CRAN (R 3.4.1)                          
##  purrr       * 0.2.3   2017-08-02 CRAN (R 3.4.1)                          
##  quadprog      1.5-5   2013-04-17 CRAN (R 3.4.1)                          
##  R.methodsS3   1.7.1   2016-02-16 CRAN (R 3.4.1)                          
##  R.oo          1.21.0  2016-11-01 CRAN (R 3.4.1)                          
##  R.utils       2.5.0   2016-11-07 CRAN (R 3.4.1)                          
##  R6            2.2.2   2017-06-17 CRAN (R 3.4.1)                          
##  Rcpp          0.12.12 2017-07-15 CRAN (R 3.4.1)                          
##  readr       * 1.1.1   2017-05-16 CRAN (R 3.4.1)                          
##  readxl        1.0.0   2017-04-18 CRAN (R 3.4.1)                          
##  reshape2      1.4.2   2016-10-22 CRAN (R 3.4.1)                          
##  rlang         0.1.2   2017-08-09 CRAN (R 3.4.1)                          
##  rvest         0.3.2   2016-06-17 CRAN (R 3.4.1)                          
##  scales        0.5.0   2017-08-24 CRAN (R 3.4.1)                          
##  seqinr        3.4-5   2017-08-01 CRAN (R 3.4.1)                          
##  shiny         1.0.5   2017-08-23 CRAN (R 3.4.1)                          
##  sp            1.2-5   2017-06-29 CRAN (R 3.4.1)                          
##  spdep         0.6-15  2017-09-01 CRAN (R 3.4.1)                          
##  splines       3.4.1   2017-09-19 local                                   
##  stats       * 3.4.1   2017-09-19 local                                   
##  stringi       1.1.5   2017-04-07 CRAN (R 3.4.1)                          
##  stringr       1.2.0   2017-02-18 CRAN (R 3.4.1)                          
##  tibble      * 1.3.4   2017-08-22 CRAN (R 3.4.1)                          
##  tidyr       * 0.7.1   2017-09-01 CRAN (R 3.4.1)                          
##  tidyverse   * 1.1.1   2017-01-27 CRAN (R 3.4.1)                          
##  tools         3.4.1   2017-09-19 local                                   
##  utils       * 3.4.1   2017-09-19 local                                   
##  vegan         2.4-4   2017-08-24 CRAN (R 3.4.1)                          
##  withr         2.0.0   2017-07-28 CRAN (R 3.4.1)                          
##  xml2          1.1.1   2017-01-24 CRAN (R 3.4.1)                          
##  xtable        1.8-2   2016-02-05 CRAN (R 3.4.1)
```

</details>
