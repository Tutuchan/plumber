# plumber

[![Build Status](https://travis-ci.org/trestletech/plumber.svg?branch=master)](https://travis-ci.org/trestletech/plumber)
[![](http://www.r-pkg.org/badges/version/plumber)](http://www.r-pkg.org/pkg/plumber)
[![CRAN RStudio mirror downloads](http://cranlogs.r-pkg.org/badges/plumber?color=brightgreen)](http://www.r-pkg.org/pkg/plumber)
[![codecov.io](https://codecov.io/github/trestletech/plumber/coverage.svg?precision=0)](https://codecov.io/github/trestletech/plumber?branch=master)

<img align="right" src="http://plumber.trestletech.com/components/images/plumber.png" />

plumber allows you to create a REST API by merely decorating your existing R source code with special comments. Take a look at an example.

```r
# myfile.R

#* @get /mean
normalMean <- function(samples=10){
  data <- rnorm(samples)
  mean(data)
}

#* @post /sum
addTwo <- function(a, b){
  as.numeric(a) + as.numeric(b)
}
```

These comments allow plumber to make your R functions available as API endpoints. You can use either `#*` as the prefix or `#'`, but we recommend the former since `#'` will collide with Roxygen. 

```r
> library(plumber)
> r <- plumb("myfile.R")  # Where 'myfile.R' is the location of the file shown above
> r$run(port=8000)
```

You can visit this URL using a browser or a terminal to run your R function and get the results. Here we're using `curl` via a Mac/Linux terminal.

```
$ curl "http://localhost:8000/mean"
 [-0.254]
$ curl "http://localhost:8000/mean?samples=10000"
 [-0.0038]
```  

As you might have guessed, the request's query string parameters are forwarded to the R function as arguments (as character strings).

```
$ curl --data "a=4&b=3" "http://localhost:8000/sum"
 [7]
```

## Installation

You can install the latest stable version from CRAN using the following command:

```r
install.packages("plumber")
```

If you want to try out the latest development version, you can install it from GitHub. The easiest way to do that is by using `devtools`.

```r
library(devtools)
install_github("trestletech/plumber")
library(plumber)
```

## Provenance

plumber was originally released as the `rapier` package and has since been renamed (7/13/2015).

