
- Use `dput` to archive an R object to a file; load into another R session with `dget`

```
dput(theData, "temporary_file")
theDataReconstitutedAgain <- dget("temporary_file")
```
- Get started with foreach and parallel programming: http://cran.r-project.org/web/packages/doParallel/vignettes/gettingstartedParallel.pdf
- Time short code snippets through replication

```
system.time(glm(HomeWin ~ Home + Away, data = results.df))

   user  system elapsed 
   1.62    0.08    1.72
```

- Use \n to embed a newline within a string, e.g. cat("Line 1\nLine 2")

```
cat("Line 1\nLine 2")

Line 1
Line 2
```

* `which(x > 0)` returns the locations of positive elements in numeric vector x http://bit.ly/qCdKKj 

```
which(LETTERS == "R")
[1] 18
which(ll <- c(TRUE, FALSE, TRUE, NA, FALSE, FALSE, TRUE))
[1] 1 3 7

which((1:12)%%2 == 0) # which are even?
[1]  2  4  6  8 10 12
```

* `ifelse(b,u,v)` where b is a boolean expression, is a `vectorized` `if-then-else` construct: http://bit.ly/t0Bi1Y

```
x <- c(2:-2)
sqrt(ifelse(x >= 0, x, NA))  # no warning
[1] 1.414214 1.000000 0.000000       NA       NA
 
ifelse(x >= 0, sqrt(x), NA)
Warning message: In sqrt(x): NaNs produced
[1] 1.414214 1.000000 0.000000       NA       NA
``` 

The ifelse() function evaluates a condition, and then assigns a value if it's true and a value if it's false.  The great part about it is that it can read in a vector and check each element of the vector one by one so you don't need indices or a loop. You don't even need to initialize some new variable before you run the statement.

[article](http://rforpublichealth.blogspot.ca/2013/01/for-loops-and-how-to-avoid-them.html) [Drive](https://drive.google.com/file/d/0B0J1O2jMMERWN0pEMUNFUW1FWXc/view?usp=drivesdk)

* Use the `subset()` function to select rows and columns from a data frame, based on the data it contains: http://bit.ly/qPEhsk 

```
In [7]: subset(airquality, Temp > 80, select = c(Ozone, Temp))
Out[7]: 
    Ozone Temp
29     45   81
35     NA   84
36     NA   85
38     29   82
...

subset(airquality, Day == 1, select = -Temp)

subset(airquality, select = Ozone:Wind)

with(airquality, subset(Ozone, Temp > 80))

# Selecting rownames starting with M using logical argument
In [10]: nm <- rownames(state.x77)
In [11]: subset(state.x77, grepl("^M", nm), Illiteracy:Murder)
Out[11]: 
              Illiteracy Life Exp Murder
Maine                0.7    70.39    2.7
Maryland             0.9    70.22    8.5
Massachusetts        1.1    71.83    3.3
Michigan             0.9    70.63   11.1
Minnesota            0.6    72.96    2.3
Mississippi          2.4    68.09   12.5
Missouri             0.8    70.69    9.3
Montana              0.6    70.56    5.0
```

- Use `formals(foo)` to get the formal arguments of a function http://bit.ly/1rHzfie 

```
In [13]: require(stats); require(graphics)

In [15]: length(formals(lm))  
Out[15]: [1] 14

In [16]: names(formals(boxplot))
Out[16]: [1] "x"   "..."
```

```
In [20]: f <- function(x) x^2
In [21]: f
Out[21]: function(x) x^2

In [22]: formals(f)
Out[22]: 
$x

In [24]: body(f)  # code inside the function
Out[24]: x^2 


In [23]: environment(f) 
Out[23]: <environment: R_GlobalEnv>
```
[_Adavnced R ref._](http://adv-r.had.co.nz/Functions.html)


- Strip non-ASCII characters from a string with `iconv(bad.text, to="ASCII", sub="")` http://bit.ly/RPbC3U

```
## Both x below are in latin1 and will only display correctly in a
## locale that can represent and display latin1.

In [25]: x <- "fa\xE7ile"
In [26]: Encoding(x) <- "latin1"
In [27]: x
Out[27]: [1] "fa<e7>ile"

In [28]: xx <- iconv(x, "latin1", "UTF-8")
In [29]: xx
Out[29]: [1] "fa<U+00E7>ile"

In [30]: iconv(x, "latin1", "ASCII")
Out[30]: [1] NA

In [31]: iconv(x, "latin1", "ASCII", "?")     # "fa?ile"
Out[31]: [1] "fa?ile"

In [32]: iconv(x, "latin1", "ASCII", "")      # "faile"
Out[32]: [1] "faile"

In [33]: iconv(x, "latin1", "ASCII", "byte")  # "fa<e7>ile"
Out[33]: [1] "fa<e7>ile"

```

 
- `na.omit(df)` will omit all rows of a data frame containing NAs http://bit.ly/1mH7iES  

```
In [34]: DF <- data.frame(x = c(1, 2, 3), y = c(0, 10, NA))

In [35]: na.omit(DF)
Out[35]: 
  x  y
1 1  0
2 2 10
```
- Type `data()` to see a list of all available data sets http://bit.ly/n5mCJZ 

```
In [38]: data()

R data sets

Data sets in package 'datasets':

AirPassengers           Monthly Airline Passenger Numbers 1949-1960
BJsales                 Sales Data with Leading Indicator
```
```
In [40]: data(package = "caret")
R data sets

Data sets in package 'caret':

GermanCredit            German Credit Data
absorp (tecator)        Fat, Water and Protein Content of Meat Samples
bbbDescr (BloodBrain)   Blood Brain Barrier Data
```
```
data(USArrests, "VADeaths")    # load the data sets 'USArrests' and 'VADeaths'
```


- `.Library` will give you the path to your default R library http://bit.ly/1oiwAr3

```
In [42]: .libPaths() 
Out[42]: [1] "/Library/Frameworks/R.framework/Versions/3.2/Resources/library"
```


- Use `unlist()` to flatten out a list of lists into a single vector: [_R Function of the Day_](http://rfunction.com/archives/2238)

```
In [43]: test1 <- list(5, "b", 12)

In [44]: test1
Out[44]: 
[[1]]
[1] 5

[[2]]
[1] "b"

[[3]]
[1] 12


In [45]: unlist(test1)
Out[45]: [1] "5"  "b"  "12"

```
```
In [48]: test2 <- list(v1=5, v2=list(2983, 1890), v3=c(3, 119)); test2
Out[48]: 
$v1
[1] 5

$v2
$v2[[1]]
[1] 2983

$v2[[2]]
[1] 1890


$v3
[1]   3 119
```
 
- To speed up your R code, use `Rprof()` to turn on profiling, and `summaryRprof()` to find the slow parts: http://bit.ly/OrT9bX

```
In [49]: Rprof(tmp <- tempfile())
In [50]: example(glm)
In [51]: Rprof()

In [52]: summaryRprof(tmp)
Out[52]: 
$by.self
                  self.time self.pct total.time total.pct
".Call"                0.02    16.67       0.02     16.67
"lazyLoadDBfetch"      0.02    16.67       0.02     16.67
...
$by.total
                      total.time total.pct self.time self.pct
"<Anonymous>"               0.12    100.00      0.00     0.00
"handle_shell"              0.10     83.33      0.00     0.00
...
$sample.interval
[1] 0.02

$sampling.time
[1] 0.12

In [53]: unlink(tmp)
```
A detailed example of profiling R code can be found at http://www.stat.berkeley.edu/~nolan/stat133/Fall05/lectures/profilingEx.html
[_Drive_](https://drive.google.com/file/d/0B0J1O2jMMERWNm9PS3FHTEJYTk0/view?usp=drivesdk)
 
- `gregexpr()` {base} finds all instances of a pattern http://bit.ly/1mmkytP 


- **R Environments** A very nice discussion of R Environments from Hadley Wickham http://adv-r.had.co.nz/Environments.html 
- Use `object.size()` to estimate the amount of memory an R object is consuming http://bit.ly/1dYFk2m

```
object.size(letters)
Out[1]: 1496 bytes

object.size(ls)
Out[2]: 70112 bytes

print(object.size(library), units = "auto")
1.1 Mb
```
```
z <- sapply(ls("package:base"), function(x)
            object.size(get(x, envir = baseenv())))
as.matrix(rev(sort(z))[1:10])
Out[5]: 
                      [,1]
loadNamespace      1334120
library            1175776
[<-.data.frame      954160
rbind.data.frame    602048
parseNamespaceFile  560584
source              525088
merge.data.frame    481320
cut.POSIXt          473424
cut.Date            461016
data.frame          445968
```
 
- Type `objects()` at the console to see what variables, funcions and data sets you have defined http://bit.ly/1rd8cJF 

```
objects()
Out[12]: 
[1] "z"

ls()
Out[13]: 
[1] "z"
```

- Look here (http://www.revolutionanalytics.com/r-language-resources) for a fairly comprehensive list of R #rstats resources.


- `x <- R.version$version.string` assign a character string containing the version of R you are running http://bit.ly/1lDnbNz 

```
R.version$version.string
Out[16]: 
[1] "R version 3.2.4 (2016-03-16)"
```

- Use the `xtable` package to convert a table or matrix to HTML: `print(xtable(X), type="html")` http://bit.ly/zrvgHJ 

```
data(tli)
tli.table <- xtable(tli[1:3,1:2 ])

print(tli.table, type="html")
<!-- html table generated in R 3.2.4 by xtable 1.8-2 package -->
<!-- Thu Jun  2 20:41:44 2016 -->
<table border=1>
<tr> <th>  </th> <th> grade </th> <th> sex </th>  </tr>
  <tr> <td align="right"> 1 </td> <td align="right">   6 </td> <td> M </td> </tr>
  <tr> <td align="right"> 2 </td> <td align="right">   7 </td> <td> M </td> </tr>
  <tr> <td align="right"> 3 </td> <td align="right">   5 </td> <td> F </td> </tr>
   </table>
```

- The `lubridate` package simplifies operations on dates and times: http://www.r-statistics.com/2012/03/do-more-with-dates-and-times-in-r-with-lubridate-1-1-0/ [_drive_](https://drive.google.com/file/d/0B0J1O2jMMERWVGxIUlNNM3pJQ1E/view?usp=drivesdk)

- Generate a sequence of dates: `seq(as.Date("2000/1/1"), as.Date("2014/1/1"), "years")` http://bit.ly/1p2yZa6 

```
seq(as.Date("2000/1/1"), as.Date("2004/1/1"), "years")
Out[32]: [1] "2000-01-01" "2001-01-01" "2002-01-01" "2003-01-01" "2004-01-01"
```
```
seq(as.Date("2000/1/1"), by = "month", length.out = 4)
Out[34]: [1] "2000-01-01" "2000-02-01" "2000-03-01" "2000-04-01"
```
```
seq(as.Date("2000/1/1"), as.Date("2001/1/1"), by = "3 months")
Out[35]: [1] "2000-01-01" "2000-04-01" "2000-07-01" "2000-10-01" "2001-01-01"
```

```
seq(as.Date("2001/1/1"), as.Date("2000/7/1"), by = "-2 months")
Out[37]: [1] "2001-01-01" "2000-11-01" "2000-09-01" "2000-07-01"
```


- Get the current date with `Sys.Date()` and the time with `Sys.time()` http://bit.ly/1psl2FW

```
Sys.time()
Out[38]: [1] "2016-06-02 20:48:03 EDT"

Sys.Date()
Out[39]: [1] "2016-06-02"
```
 
- `sample(x)` randomly shuffles the elements of a vector or list x #rstats http://www.inside-r.org/r-doc/base/sample 

```
x <- 1:12
# a random permutation
sample(x)
Out[40]: 
 [1] 12 10 11  8  6  1  7  5  9  3  4  2
```
```
# bootstrap resampling -- only if length(x) > 1 !
sample(x, replace = TRUE)
Out[41]: 
 [1]  8  3  3 10  4  7 11  5  5  5 10  2
```
```
# 10 Bernoulli trials
sample(c(0,1), 10, replace = TRUE)
Out[42]: 
 [1] 1 1 1 1 0 0 1 0 0 0
```

- RevoJoe's updated list of data sets: http://bit.ly/1lcbZps [_drive_](https://drive.google.com/file/d/0B0J1O2jMMERWMmp6TkJlZFFpWkU/view?usp=drivesdk)
- "An Introduction to Statistical Learning" : (James et.al.) is full of R code and free to download http://www-bcf.usc.edu/~gareth/ISL/ 
- Nice short list of useful R functions from Alastair Sanderson http://www.sr.bham.ac.uk/~ajrs/R/r-function_list.html [_drive_](https://drive.google.com/file/d/0B0J1O2jMMERWX0RPcS1iWFFFd28/view?usp=drivesdk) 

- Yes, you can return more than one value from a function: use a list! `return(list(val1, val2, val3))` http://bit.ly/p6epvV 
- `as.data.frame(installed.packages()[,c(1,3)],row.names=F)` : list of installed packages with version bit.ly/RMvSYr

```
as.data.frame(installed.packages()[,c(1,3)],row.names=F)

      Package Version
1          A3   1.0.0
2         abc     2.1
3    abc.data     1.0
4 ABCanalysis   1.1.0
5    abcdeFBA     0.4
6 ABCExtremes     1.0
```

- Run help file examples for a particular function using the 'example' function: `example(plot)` or `example(lm)` http://bit.ly/1hGXhlB

```
example(plot)

plot> require(stats) # for lowess, rpois, rnorm
plot> plot(cars)
plot> lines(lowess(cars))
```

```
example(lm)

lm> require(graphics)

lm> ## Annette Dobson (1990) "An Introduction to Generalized Linear Models".
lm> ## Page 9: Plant Weight Data.
lm> ctl <- c(4.17,5.58,5.18,6.11,4.50,4.61,5.17,4.53,5.33,5.14)
...
```

- Concatenate elements of a vector to a single comma-separated string: `paste(letters, collapse=", ")` http://bit.ly/LEU8S0 

```
paste(letters, collapse=", ")
Out[49]: 
[1] "a, b, c, d, e, f, g, h, i, j, k, l, m, n, o, p, q, r, s, t, u, v, w, x, y, z"
```
```
paste(1:12) # same as as.character(1:12)
Out[50]: 
 [1] "1"  "2"  "3"  "4"  "5"  "6"  "7"  "8"  "9"  "10" "11" "12"
```
```
paste("A", 1:6, sep = "")
Out[51]: 
[1] "A1" "A2" "A3" "A4" "A5" "A6"	
```
```
paste0("A", 1:6)
Out[52]: 
[1] "A1" "A2" "A3" "A4" "A5" "A6"
```
```
identical(paste ("A", 1:6, sep = ""), paste0("A", 1:6))
Out[54]: 
[1] TRUE
```
```
paste("Today is", date())
Out[55]: 
[1] "Today is Thu Jun  2 21:04:37 2016"
```

- Some nice R tutorials from Willian B. King http://ww2.coastal.edu/kingw/statistics/R-tutorials/index.html
- Leave out the `mth` column of a data frame: `dF[,-m]`  http://bit.ly/1pkampQ
- `readHTMLTable(url,which=2)` will read the 2nd table on the webpage specified by the url http://bit.ly/1jEUopG

```
airline = 'http://www.theacsi.org/index.php?option=com_content&view=article&id=147&catid=&Itemid=212&i=Airlines'

tables = XML::readHTMLTable(airline)

names(tables)
Out[74]: 
[1] "NULL"

tables[[1]]
Out[75]: 
                      Base-line 95 ...
1             JetBlue        NM NM ...
2           Southwest        78 76 ...
...

```
_similar `htmltable`_ - http://www.r-datacollection.com/blog/Hassle-free-data-from-HTML-tables-with-the-htmltable-package/ 

- Indexing into a list: `myList[[m]][n]` will yield the nth element of the mth item in list myList #rstats http://bit.ly/1pkampQ
- Not sure why your R function threw an error? Use `traceback()` to find out where it occurred. http://bit.ly/qokamy

```
foo <- function(x) { print(1); bar(2) }
bar <- function(x) { x + a.variable.which.does.not.exist }
 
foo(2) 
traceback()
```
refer article - http://petewerner.blogspot.ca/2013/01/tracking-down-errors-in-r.html

- Tutorial introduction to time series in #rstats: http://www.stat.pitt.edu/stoffer/tsa3/R_toot.htm [_drive_](https://drive.google.com/file/d/0B0J1O2jMMERWUWthaHRKMXdVN1k/view?usp=drivesdk)
- To speed an R function, use `cmpfun` to byte-compile it: http://bit.ly/t2OR0K

```
f <- function(x) x+1
fc = compiler::cmpfun(f)
fc(2)
Out[87]: 
[1] 3
```
- If x is a matrix, vector or list then `x[]<-0` replaces all its values by 0. More uses of the empty bracket in #rstats: http://bit.ly/14E9ulW

```
mat <- matrix(NA, nrow = 5, ncol = 5); mat
Out[91]: 
     [,1] [,2] [,3] [,4] [,5]
[1,]   NA   NA   NA   NA   NA
[2,]   NA   NA   NA   NA   NA
[3,]   NA   NA   NA   NA   NA
[4,]   NA   NA   NA   NA   NA
[5,]   NA   NA   NA   NA   NA

mat[] <- 1; mat
mat[] <- 1; mat
Out[92]: 
     [,1] [,2] [,3] [,4] [,5]
[1,]    1    1    1    1    1
[2,]    1    1    1    1    1
[3,]    1    1    1    1    1
[4,]    1    1    1    1    1
[5,]    1    1    1    1    1

L <- list(a = 1, b = 2, c = 3);L
Out[93]: 
$a
[1] 1

$b
[1] 2

$c
[1] 3

L[] <- 5; L

Out[94]: 
$a
[1] 5

$b
[1] 5

$c
[1] 5
```
 
- A 26 page tutorial on how the `mboost` package can help you use gradient boosting to fit a prediction model https://cran.r-project.org/web/packages/mboost/vignettes/mboost_tutorial.pdf 
- Can't find a package binary on CRAN? Check its build status on all platforms here: http://cran.r-project.org/web/checks/check_summary.html 
- The `rmongodb` package - an interface to the open-source, NoSQL MongoDB database. Great vignettes https://cran.r-project.org/web/packages/rmongodb/index.html 
- The `RSQLite` package embeds the SQLite DB engine in R - great way to get started with DBMS https://cran.r-project.org/web/packages/RSQLite/RSQLite.pdf 
- The `sqldf` package lets you do sql queries on an R dataframe https://github.com/ggrothendieck/sqldf  #rstats

```
library(sqldf)
sqldf("select * from BOD where Time > 4")

Out[97]: 
  Time demand
1    5   15.6
2    7   19.8
```

- The `RMySQL` package is a database interface and MySQL driver for R http://biostat.mc.vanderbilt.edu/wiki/Main/RMySQL



 
- The `RODBC` package provides access to SQL databases - Get started with the vignette https://cran.r-project.org/web/packages/RODBC/vignettes/RODBC.pdf
- `search()` displays the "search list" of packages where R searches for functions and other objects #rstats http://www.inside-r.org/r-doc/base/search 

```
search()
Out[11]: 
[1] ".GlobalEnv"        "package:stats"     "package:graphics" 
[4] "package:grDevices" "package:utils"     "package:datasets" 
[7] "package:methods"   "Autoloads"         "package:base"
```


- Use `NROW`/`NCOL` instead of `nrow`/`ncol` to treat vectors as 1-column matrices http://bit.ly/HDgsfu

```
NCOL(1:12)
Out[12]: 
[1] 1

NROW(1:12)
Out[14]: 
[1] 12
```

- `signif(x,digits=n)` rounds x to n significant digits http://www.inside-r.org/r-doc/base/Round

```
x2 <- pi * 100^(-1:3)
round(x2, 3)
Out[16]: 
[1]       0.031       3.142     314.159   31415.927 3141592.654

signif(x2, 3)
Out[20]: 
[1] 3.14e-02 3.14e+00 3.14e+02 3.14e+04 3.14e+06
```
- Some examples of `get()` to call an R object using a character string:   http://rfunction.com/archives/2105

```
x5 <- "x5 variable"
get("x5")

L3 <- list("list", "of", "three")
get("L3")
Out[23]: 
[[1]]
[1] "list"

[[2]]
[1] "of"

[[3]]
[1] "three"

get("L3[[3]]")
Error in get("L3[[3]]"): object 'L3[[3]]' not found

get("L3")[[3]]
Out[25]: 
[1] "three"


get("mean")(c(1.5, 2.5, 3.5, 4.5))
Out[26]: 
[1] 3
```

- Annotate `ggplot2` graphs by creating an annotation layer with `annotate()` http://bit.ly/1rtSpXd 
- Explore the capabilities of your installed R packages with `browseVignettes()` http://bit.ly/I2UQ0v 

- Avoid namespace clashes! Use `::` to specify an object within a specific package, e.g. MASS::mvnorm http://www.inside-r.org/node/35769

```
base::log
Out[32]: 
function (x, base = exp(1))  .Primitive("log")

base::"+"
Out[32]: 
function (e1, e2)  .Primitive("+")

stats:::coef.default
Out[33]: 
function (object, ...) 
object$coefficients
<bytecode: 0x7fa31691d630>
<environment: namespace:stats>
```

- Merge two data frames with `merge(df1, df2)` for an inner join and `merge(df1,df2,all=TRUE)` for an outer join. http://bit.ly/ILpoTs 

- Determine the class of every variable in a data frame: `sapply(df,class)` http://bit.ly/1hUTWmv
- The `WDI` package provides access to the World Bank's World Development Indicators http://bit.ly/PdIWVF
- How to unshorten a `bit.ly`, `t.co` or other short URL with R: https://tonybreyal.wordpress.com/2011/12/13/unshorten-any-url-created-using-url-shortening-services-decode_shortened_url/


- Use the `plyr` package to easily sort a data frame by a column: `arrange(df, desc(colname))` http://bit.ly/IyE1Wa

- For base graphics, `par(mfrow=c(m,n))` will divide a graphics window into a grid of `m x n` plots http://bit.ly/1hHnLSE 
- `cor.test(x,y)` will tell you if the correlation between vectors x and y is statistically significant http://www.inside-r.org/r-doc/stats/cor.test

```
x <- c(44.4, 45.9, 41.9, 53.3, 44.7, 44.1, 50.7, 45.2, 60.1)
y <- c( 2.6,  3.1,  2.5,  5.0,  3.6,  4.0,  5.2,  2.8,  3.8)
cor.test(x,y)

Out[34]: 
	Pearson's product-moment correlation
data:  x and y
t = 1.8411, df = 7, p-value = 0.1082

alternative hypothesis: true correlation is not equal to 0
95 percent confidence interval:
 -0.1497426  0.8955795
sample estimates:
      cor 
0.5711816
```
- Use `mapply` to call a multi-argument function repeatedly, e.g. `mapply(sample, list(1:56, 1:46), c(5,1))` http://bit.ly/H9AbSs

```
mapply(rep, 1:4, 4:1)
Out[40]: 
[[1]]
[1] 1 1 1 1

[[2]]
[1] 2 2 2

[[3]]
[1] 3 3

[[4]]
[1] 4

mapply(rep, times = 1:4, x = 4:1)
Out[41]: 
[[1]]
[1] 4

[[2]]
[1] 3 3

[[3]]
[1] 2 2 2

[[4]]
[1] 1 1 1 1


```
- Define your own binary operators http://proquestcombo.safaribooksonline.com.ezproxy.torontopubliclibrary.ca/book/programming/r/9780596809287/12dot-useful-tricks/id3432422?uicode=torontopl

R recognizes any text between percent signs (`%...%`) as a binary operator. Create and define a new binary operator by assigning a two-argument function to it. R predefines several such operators, such as `%/% `for integer division and `%*%` for matrix multiplication

```
'%+-%' <- function(x,margin) x + c(-1,+1)*margin
100 %+-% 3
Out[48]: 
[1]  97 103 
```
The expression x %+-% m calculates x ± m.

- `X %in% Y` is the same as `!is.na(match(X,Y))` #rstats http://bit.ly/1q1Pkej

```
1:10 %in% c(1,3,5,9)
Out[52]: 
 [1]  TRUE FALSE  TRUE FALSE  TRUE FALSE FALSE FALSE  TRUE FALSE
 
match(1:10, c(1, 3, 5, 9))
Out[53]: 
 [1]  1 NA  2 NA  3 NA NA NA  4 NA
 
!is.na(match(1:10, c(1, 3, 5, 9)))
Out[54]: 
 [1]  TRUE FALSE  TRUE FALSE  TRUE FALSE FALSE FALSE  TRUE FALSE
```
- Use package `XLConnect` to read and write Excel files http://www.inside-r.org/packages/cran/XLConnect, http://www.milanor.net/blog/steps-connect-r-excel-xlconnect/ [_drive_](https://drive.google.com/file/d/0B0J1O2jMMERWTFdROHJzVGJjRWc/view?usp=drivesdk)

Create empty xlsx file

```
fileXls <- paste(outDir,"newFile.xlsx",sep='/')
unlink(fileXls, recursive = FALSE, force = FALSE)
exc <- loadWorkbook(fileXls, create = TRUE)
createSheet(exc,'Input')
saveWorkbook(exc)
```

Populate an empty xlsx sheet with R

```
input <- data.frame('inputType'=c('Day','Month'),'inputValue'=c(2,5))
writeWorksheet(exc, input, sheet = "input", startRow = 1, startCol = 2)
saveWorkbook(exc)
```

Add other sheets to the workbook

```
require(reshape)
createSheet(exc,'Airquality')
airquality$isCurrent<-NA
createName(exc, name='Airquality',formula='Airquality!$A$1')
writeNamedRegion(exc, airquality, name = 'Airquality', header = TRUE)
saveWorkbook(exc)
```

- Deleting a function from a package will surprise your users. Deprecate it instead: http://www.bioconductor.org/developers/deprecation/  (via @TrestleJeff) 
- Locate the largest number in a vector with `which.max(mydata)` http://www.inside-r.org/r-doc/base/which.min (via @M_T_Patterson) #rstats

```
x <- c(1:4, 0:5, 11);x
Out[57]: 
 [1]  1  2  3  4  0  1  2  3  4  5 11

which.min(x)
Out[55]: 
[1] 5

which.max(x)
Out[55]: 
[1] 11
```


- 2 short tutorials for the `reshape2` package: http://seananderson.ca/2013/10/19/reshape.html,  and https://tgmstat.wordpress.com/2013/10/31/reshape-and-aggregate-data-with-the-r-package-reshape2/ [_drive_](https://drive.google.com/file/d/0B0J1O2jMMERWMFZhRnEtRkFEc1k/view?usp=drivesdk) and [_drive_](https://drive.google.com/file/d/0B0J1O2jMMERWR091M1pmc2ctNzQ/view?usp=drivesdk)
- Type `?Startup` for a description of R's initialization process and how to configure it: http://www.inside-r.org/r-doc/base/Startup #rstats

- Install an R package directly from GitHub with devtools package: http://bit.ly/yUSrZr

```
install_github("klutometis/roxygen")
install_github(c("rstudio/httpuv", "rstudio/shiny"))
install_github(c("hadley/httr@v0.4", "klutometis/roxygen#142",
  "mfrasca/r-logging/pkg"))
install_github("hadley/private", auth_token = "abc")
```


- Sort a vector into decreasing order: `sort(x,decreasing=TRUE)` #rstats http://www.inside-r.org/r-doc/base/sort

```
require(stats)
x <- swiss$Education[1:25]
x; sort(x); sort(x, partial = c(10, 15))

Out[63]: 
 [1] 12  9  5  7 15  7  7  8  7 13  6 12  7 12  5  2  8 28 20  9 10  3 12  6  1
Out[63]: 
 [1]  1  2  3  5  5  6  6  7  7  7  7  7  8  8  9  9 10 12 12 12 12 13 15 20 28
Out[63]: 
 [1]  3  2  5  5  1  6  6  7  7  7  7  8  7  8  9  9 10 12 12 12 12 20 28 13 15
```

- Use the `with()` function to write R code using variables in a data frame, without needing the $ syntax: #rstats http://bit.ly/onqu7V
- Use the stringr package to make life easier when working with strings. http://bit.ly/OqMe7G
- Constants built into #rstats: letters, months and pi: http://bit.ly/zCxy4H
- Every row of a data frame has a unique label, which you can set or retrieve with row.names: http://bit.ly/woMnsV #rstats
- Use readLines() to read a text file line by line #rstats http://bit.ly/1feunH1
- To tell what version of package foo you're running, you can call packageDescription(foo)$Version http://bit.ly/ZX3lPb
- Easily format numbers with fixed precision in strings with sprintf: http://bit.ly/XsdjaS
- For an illuminating discussion of R formulas from the National Park Service see http://bit.ly/MArWaE 
- You can plot a function object to create a chart of its equation, e.g. plot(dnorm, xlim=c(-4,4)) http://bit.ly/H8SSce 
- Calculate running 30-day means of values in vector x: filter(x,rep(1/30,30)) #rstats http://bit.ly/we8tgt  
- Return the row/column indices of positive elements of matrix x: which(x>0, arr.ind=TRUE) http://bit.ly/qCdKKj 
- dplyr makes data manipulation in R easy and fast! Check out my Davis R Users Group talk with easy demos: http://www.michaellevy.name/blog/dplyr-data-manipulation-in-r-made-easy 
- Sourcing code from github SourceURL - http://christophergandrud.blogspot.com/2012/07/sourcing-code-from-github.html
- To change where R exports files, use setwd() to change the current working directory first: http://t.co/NNqZweYWzc #rstats
- Use mode(obj) to determint the storage mode of an object. #rstats http://t.co/tPHYrVrUwR 
- Try: tryCatch(expr,. . . , finally) for handling errors and warnings #rstats http://t.co/tPHYrVrUwR 
- format(x, scientific=TRUE) prints numeric data in exponential format, so 0.0001 prints as 1e-04 http://t.co/W1c7L6LIRJ 
- List of R functions and packages for Time Series Analysis: http://t.co/viVIoWfemJ  #rstats
- Tips on writing efficient #rstats code (from R co-creator R Ihaka): https://www.stat.auckland.ac.nz/~stat782/downloads/10-Efficiency.pdf (via @hadleywickham)
- Compare t-distribution with qnorm(.975) as df increases qt(.975, df = c(10,100,1000,10000)) http://t.co/j5WhOXlcsg  #rstats
- To read a fixed width file: data <- read.fwf("file", widths=c(w1, w2, w3 . . . wn)) http://t.co/KmfLMQ1CXC  #rstats
- Use rpois(n,lambda) to generate n random draws from a poisson distribution with rate lambda http://t.co/eR3AHj2CpC  #rstats
- apropos("lm") will find all objects in your search path with "lm" in the name http://t.co/DqXjju5bht  #rstats
- is.na() tests for missing (NA) values. is.null() tests for NULL values. http://t.co/Xt5b3CgDuY  #rstats
- Use saveRDS(obj,"myfile") and readRDS(obj,"myfile") to save / read an R object to / from a file. http://t.co/CvC4jHb9w1 #rstats
- How to calculate the date of Easter in R: library(timeDate); Easter(2015) #rstats http://t.co/XcE5Sh99rA 
- Use model.matrix to convert factors into binary indicator columns in a matrix: http://t.co/2SFWyh91jj  #rstats
- Add a vertical or horizontal line to a base graphics plot with abline(v=x) or abline(h=y) http://t.co/2OXkjNqeH0  #rstats
- In a "for loop": for(var in seq) expr, seq can be any vector. e.g. seq<-c(100,12,1000) http://t.co/fpaFMZoZfI #rstats
- To create the list of every possible combination of levels in multiple factors, use expand.grid: http://t.co/AltLOChywI  #rstats
- Tomorrow, use package Rmpfr ro get pi to 3321 bits of precision (Pi <- Const("pi", 1000 *log2(10))) http://t.co/OUM9gMPac7 #rstats
- You can use the readHTMLTable function to scrape data from a Web page: http://t.co/pgP0kNQmGp  #rstats
- Stackoverflow r info page http://stackoverflow.com/tags/r/info 
- R Trending repositories on Github -- https://github.com/trending?l=r
- STAT545 - https://stat545-ubc.github.io/ - go to topics
o	Automate an analytical pipeline , e.g. via Make - 
- Running R in batch mode on Linux - http://www.cureffi.org/2014/01/15/running-r-batch-mode-linux/
- Shortcut for assignment operator : Insert an assignment operator (<-), as well as a space on either side, using Alt + -.
- Consider the advice of Randall Munroe of xkcd: “One thing that bothers me is large numbers presented without context… ‘If I added a zero to this number, would the sentence containing it mean something different to me?’ If the answer is ‘no,’ maybe the number has no business being in the sentence in the first place.” 

partykit package for tools to visualize classification tree models. 
partykit: A Toolkit for Recursive Partytioning
A toolkit with infrastructure for representing, summarizing, and visualizing tree-structured regression and classification models. This unified infrastructure can be used for reading/coercing tree models from different sources (rpart, RWeka, PMML) yielding objects that share functionality for print/plot/predict methods. Furthermore, new and improved reimplementations of conditional inference trees (ctree) and model-based recursive partitioning (mob) from the party package are provided based on the new infrastructure
How to plot a large ctree() to avoid overlapping nodes http://stackoverflow.com/a/13779868/2356016
http://stackoverflow.com/q/16581587/2356016

### Testing 
* `testthat` Testing framework for R: http://bit.ly/PJsGZY http://vita.had.co.nz/papers/testthat.pdf 



TODO: 
- Most of the links point to insideR which does not helps much. Add some code snippets and link to it. 
- Difference betweeen different read functions. read.delim;
