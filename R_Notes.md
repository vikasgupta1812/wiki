# Packages in R

## awesome-R
https://awesome-r.com/
https://github.com/qinwf/awesome-R

## magrittr

Instead of nested statements, it is using pipe operator %>%. So the code is easier to read. Impressive!

- [Slides - Pipelines in R](http://rpubs.com/tjmahr/pipelines_2015) [Drive](https://drive.google.com/file/d/0B0J1O2jMMERWWEx0STJVTVh2U28)
- [Slides - dplyr: verbs for manipulating data-frames](http://rpubs.com/tjmahr/dplyr_2015) [Drive](https://drive.google.com/file/d/0B0J1O2jMMERWSlh0cE9pNVRHYlE/view?usp=drivesdk)
- [Slides - Making pretty regression tables with pipes](http://rpubs.com/tjmahr/prettytables_2015) [Drive](https://drive.google.com/file/d/0B0J1O2jMMERWV3hsQWdQSW4wM1E/view?usp=drivesdk)
- http://rud.is/b/2015/02/04/a-step-to-the-right-in-r-assignments/ [cache](https://drive.google.com/file/d/0B0J1O2jMMERWX1p3THM4b0tqZ28/view?usp=drivesdk)


## stringr and plyr packages

http://martinsbioblogg.wordpress.com/2013/03/24/using-r-reading-tables-that-need-a-little-cleaning/

A data.frame is pretty much a list of vectors, so we use plyr to apply over the list and stringr to search and replace in the vectors.


## ggplot2
Some examples: http://blog.diegovalle.net/2015/01/the-74-most-violent-cities-in-mexico.html

Introduction - https://www.youtube.com/watch?v=SaJCKpYX5Lo&t=2742

### ggthemr: 
Themes for ggplot2


## Parallel Computing

Example code for the book Parallel R by McCallum and Weston.  - http://shop.oreilly.com/product/0636920021421.do

An introduction to distributed memory parallelism in R and C - http://www.stat.berkeley.edu/scf/paciorek-distribComp.pdf

Processing: When does it worth? - http://danielmarcelino.com/parallel-processing/Parallel

### Windows Security Warning
It seems it is safe to choose 'Cancel' when Windows Firewall tried to block R program when we use makeCluster() to create a socket cluster.

```r
library(parallel)
cl <- makeCluster(2)
clusterApply(cl, 1:2, get("+"), 3)
stopCluster(cl)
```

If we like to see current firewall settings, just click Windows Start button, search 'Firewall' and choose 'Windows Firewall with Advanced Security'. In the 'Inbound Rules', we can see what programs (like, R for Windows GUI front-end, or Rserve) are among the rules. These rules are called 'private' in the 'Profile' column. Note that each of them may appear twice because one is 'TCP' protocol and the other one has a 'UDP' protocol.

### Parallel package

Parallel package was included in `R 2.14.0`. It is derived from the `snow` and `multicore` packages and provides many of the same functions as those packages.

The parallel package provides several `*apply` functions for `R` users to quickly modify their code using parallel computing.


* `makeCluster`(makePSOCKcluster, makeForkCluster), stopCluster. Other cluster * types are passed to package snow.
* `clusterCall`, clusterEvalQ, clusterSplit
* `clusterApply`, clusterApplyLB
* `clusterExport`
* `clusterMap`
* `parLapply`, `parSapply`, `parApply`, `parRapply`, `parCapply`
* `parLapplyLB`, parSapplyLB (load balance version)
* `clusterSetRNGStream`, `nextRNGStream`, nextRNGSubStream
Examples (See ?clusterApply)

```r
library(parallel)
cl <- makeCluster(2, type = "SOCK")
clusterApply(cl, 1:2, function(x) x*3)    # OR clusterApply(cl, 1:2, get("*"), 3)
# [[1]]
# [1] 3
#
# [[2]]
# [1] 6
parSapply(cl, 1:20, get("+"), 3)
#  [1]  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23
stopCluster(cl)
```
### `snow` package
Supported cluster types are "SOCK", "PVM", "MPI", and "NWS".

### `multicore` package
This package is removed from CRAN. Consider using package `parallel` instead.

### `foreach` package

This package depends on one of the following

* doParallel - Foreach parallel adaptor for the parallel package
* doSNOW - Foreach parallel adaptor for the snow package
* doMC - Foreach parallel adaptor for the multicore package
* doMPI - Foreach parallel adaptor for the Rmpi package
* doRedis - Foreach parallel adapter for the `rredis` package as a backend.

```r
library(foreach)
library(doParallel)

m <- matrix(rnorm(9), 3, 3)

cl <- makeCluster(2, type = "SOCK")
registerDoParallel(cl)
foreach(i=1:nrow(m), .combine=rbind) %dopar%
  (m[i,] / mean(m[i,]))

stopCluster(cl)
```

### `snowfall` package

http://www.imbi.uni-freiburg.de/parallel/docs/Reisensburg2009_TutParallelComputing_Knaus_Porzelius.pdf


### Rmpi package

MPI defines an environment where programs can run in parallel and communicate with each other by passing messages to each other. 

Some examples/tutorials

- ~~http://trac.nchc.org.tw/grid/wiki/R-MPI_Install~~
- http://trac.3du.me/grid/wiki/R-MPI_Install [cache](https://drive.google.com/file/d/0B0J1O2jMMERWVUdNMGhnd0NKbU0/view?usp=drivesdk)
- http://www.arc.vt.edu/resources/software/r/index.php
- ~~https://www.sharcnet.ca/help/index.php/Using_R_and_MPI~~
- ~~http://math.acadiau.ca/ACMMaC/Rmpi/examples.html~~
- Ryan Rosario - http://www.slideshare.net/bytemining/taking-r-to-the-limit-high-performance-computing-in-r-part-1-parallelization-la-r-users-group-727
- http://pj.freefaculty.org/guides/Rcourse/parallel-1/parallel-1.pdf
- http://biowulf.nih.gov/apps/R.html



## Cloud Computing

### Install R on Amazon EC2
http://randyzwitch.com/r-amazon-ec2/

### Bioconductor on Amazon EC2
http://www.bioconductor.org/help/bioconductor-cloud-ami/



### RInside (embed R inside C++ code)

http://dirk.eddelbuettel.com/code/rinside.html
http://dirk.eddelbuettel.com/papers/rfinance2010_rcpp_rinside_tutorial_handout.pdf

## Plot ip addresses on google map

- [Mapping blog visits on google maps with googleVis](http://thebiobucket.blogspot.com/2011/12/some-fun-with-googlevis-plotting-blog.html#more) [cache](https://drive.google.com/file/d/0B0J1O2jMMERWR0xDdkM3WHlfc2M/view?usp=drivesdk) (RCurl, RJONIO, plyr, googleVis) 
- http://devblog.icans-gmbh.com/using-the-maxmind-geoip-api-with-r/ (RCurl, RJONIO, maps)
- http://cran.r-project.org/web/packages/geoPlot/index.html (geoPlot package (deprecated as 8/12/2013))
- http://batchgeo.com/features/geolocation-ip-lookup/ (Not R) (Enter a spreadsheet of adress, city, zip or a column of IPs and it will show the location on google map)
- http://code.google.com/p/apachegeomap/

The following example is modified from the first of above list.

```r
require(RJSONIO) # fromJSON
require(RCurl)   # getURL

temp = getURL("https://gist.github.com/arraytools/6743826/raw/23c8b0bc4b8f0d1bfe1c2fad985ca2e091aeb916/ip.txt", ssl.verifypeer = FALSE)
ip <- read.table(textConnection(temp), as.is=TRUE)
names(ip) <- "IP"
nr = nrow(ip)
 
Lon <- as.numeric(rep(NA, nr))
Lat <- Lon
Coords <- data.frame(Lon, Lat)
 
ip2coordinates <- function(ip) {
  api <- "http://freegeoip.net/json/"
  get.ips <- getURL(paste(api, URLencode(ip), sep=""))
  # result <- ldply(fromJSON(get.ips), data.frame)
  result <- data.frame(fromJSON(get.ips))
  names(result)[1] <- "ip.address"
  return(result)
}

for (i in 1:nr){
  cat(i, "\n")
  try(
  Coords[i, 1:2] <- ip2coordinates(ip$IP[i])[c("longitude", "latitude")]
  )
}
 
# append to log-file:
logfile <- data.frame(ip, Lat = Coords$Lat, Long = Coords$Lon, LatLong = paste(round(Coords$Lat, 1), round(Coords$Lon, 1), sep = ":")) 
log_gmap <- logfile[!is.na(logfile$Lat), ]

require(googleVis) # gvisMap
gmap <- gvisMap(log_gmap, "LatLong",
                options = list(showTip = TRUE, enableScrollWheel = TRUE,
                               mapType = 'hybrid', useMapTypeControl = TRUE,
                               width = 1024, height = 800))
plot(gmap)
```

The plot.gvis() method in `googleVis` packages also teaches the `startDynamicHelp`() function in the tools package, which was used to launch a http server. See Jeffrey Horner's note about deploying Rook App.

http://jeffreyhorner.tumblr.com/page/3

##Maps

### leaflet 
rstudio.github.io/leaflet/#installation-and-use

### choroplethr

http://blog.revolutionanalytics.com/2014/01/easy-data-maps-with-r-the-choroplethr-package-.html
http://www.arilamstein.com/blog/2015/06/25/learn-to-map-census-data-in-r/


## Use Rcpp in RStudio

RStudio makes it easy to use `Rcpp` package.

Open RStudio, click New File -> C++ File. It will create a C++ template on the RStudio editor

```
#include <Rcpp.h>
using namespace Rcpp;

// Below is a simple example of exporting a C++ function to R. You can
// source this function into an R session using the Rcpp::sourceCpp 
// function (or via the Source button on the editor toolbar)

// For more on using Rcpp click the Help button on the editor toolbar

// [[Rcpp::export]]
int timesTwo(int x) {
   return x * 2;
}
```

Now in R console, type

```r
library(Rcpp)
sourceCpp("~/Downloads/timesTwo.cpp")
timesTwo(9)
```

```
# [1] 18
```

See more examples on http://adv-r.had.co.nz/Rcpp.html.

If we wan to test `Boost` library, we can try it in RStudio. Consider the following example in stackoverflow.com.

```
// [[Rcpp::depends(BH)]]
#include <Rcpp.h>
#include <boost/foreach.hpp>
#include <boost/math/special_functions/gamma.hpp>

#define foreach BOOST_FOREACH

using namespace boost::math;

//[[Rcpp::export]]
Rcpp::NumericVector boost_gamma( Rcpp::NumericVector x ) {
  foreach( double& elem, x ) {
    elem = boost::math::tgamma(elem);
  };

  return x;
}
```

Then the R console

```r
boost_gamma(0:10 + 1)
```

```
#  [1]       1       1       2       6      24     120     720    5040   40320
# [10]  362880 3628800
```

```r
identical( boost_gamma(0:10 + 1), factorial(0:10) )
```

```
# [1] TRUE
```
### Example 1. Convolution example

First, Rcpp package should be installed (I am working on Linux system). Next we try one example shipped in Rcpp package.

PS. If `R` was not available in global environment (such as built by ourselves), we need to modify 'Makefile' file by replacing 'R' command with its complete path (4 places).

```
cd ~/R/x86_64-pc-linux-gnu-library/3.0/Rcpp/examples/ConvolveBenchmarks/
make
R
```

Then type the following in an R session to see how it works. Note that we don't need to issue library(Rcpp) in R.

```r
dyn.load("convolve3_cpp.so")
x <- .Call("convolve3cpp", 1:3, 4:6)
x # 4 13 28 27 18
```
If we have our own cpp file, we need to use the following way to create dynamic loaded library file. Note that the character (grave accent) ` is not (single quote)'. If you mistakenly use ', it won't work.

```
export PKG_CXXFLAGS=`Rscript -e "Rcpp:::CxxFlags()"`
export PKG_LIBS=`Rscript -e "Rcpp:::LdFlags()"`
R CMD SHILB xxxx.cpp
```
### Example 2. Use together with inline package

http://adv-r.had.co.nz/C-interface.html#calling-c-functions-from-r

```r
library(inline)
src <-'
 Rcpp::NumericVector xa(a);
 Rcpp::NumericVector xb(b);
 int n_xa = xa.size(), n_xb = xb.size();

 Rcpp::NumericVector xab(n_xa + n_xb - 1);
 for (int i = 0; i < n_xa; i++)
 for (int j = 0; j < n_xb; j++)
 xab[i + j] += xa[i] * xb[j];
 return xab;
'
fun <- cxxfunction(signature(a = "numeric", b = "numeric"),
 src, plugin = "Rcpp")
fun(1:3, 1:4) 
# [1]  1  4 10 16 17 12
```
### Example 3. Calling an R function

RcppParallel - http://cran.r-project.org/web/packages/RcppParallel/index.html
caret - http://cran.r-project.org/web/packages/caret/index.html


