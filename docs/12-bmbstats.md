---
output: html_document
editor_options: 
  chunk_output_type: console
---
# (PART) Part Two {-}

# `bmbstats`: Bootstrap Magnitude-based Statistics package

<img src="figures/bmbstats-logo.png" align="right" width="200" />



In the first part of this book we have covered descriptive, predictive, and causal inference tasks, followed by the basics of statistical inference using frequentist, Bayesian, and bootstrap methods and concluded with the measurement error explanation. In this part of the book, we will turn to `bmbstats`, which is short of *Bootstrap Magnitude-based Statistics*, package to perform analysis of the most common sports science problems and utilize bootstrap as the inference method of choice [@R-bmbstats].

Since I strongly believe that the best way to understand statistical analysis is through simulations and smart visualizations, in this part of the book I will show the R code that you can use to reproduce the analysis, but more importantly, to understand the underlying DGP we are trying to explain (or estimate) with the analysis. 

## `bmbstats` Installation

You can install the development version from [GitHub](https://github.com/mladenjovanovic/bmbstats) with:

``` r
# install.packages("devtools")
devtools::install_github("mladenjovanovic/bmbstats")

require(bmbstats)
```




