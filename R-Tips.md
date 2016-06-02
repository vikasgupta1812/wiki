
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
```

* Use the testthat package to create a testing framework for R packages: http://bit.ly/PJsGZY http://vita.had.co.nz/papers/testthat.pdf 
* ifelse(b,u,v) where b is a boolean expression, is a vectorized if-then-else construct: http://bit.ly/t0Bi1Y 
* Use the subset() function to select rows and columns from a data frame, based on the data it contains: http://bit.ly/qPEhsk 
- Use formals(foo) to get the formal arguments of a function http://bit.ly/1rHzfie 
- Strip non-ASCII characters from a string with iconv(bad.text, to="ASCII", sub="") http://bit.ly/RPbC3U 
- na.omit(df) will omit all rows of a data frame containing NAs http://bit.ly/1mH7iES  
- Type data() to see a list of all available data sets http://bit.ly/n5mCJZ 
- .Library will give you the path to your default R library http://bit.ly/1oiwAr3
- Use unlist() to flatten out a list of lists into a single vector: http://rfunction.com/archives/2238 
- To speed up your R code, use Rprof() to turn on profiling, and summaryRprof() to find the slow parts: http://bit.ly/OrT9bX 
- gregexpr() {base} finds all instances of a pattern http://bit.ly/1mmkytP 
- A very nice discussion of R Environments from Hadley Wickham http://adv-r.had.co.nz/Environments.html 
- Some tips on using R to talk to Twitter from Raffael Vogler http://bit.ly/1iNxJsW  http://www.joyofdata.de/blog/talking-to-twitters-rest-api-v1-1-with-r/ 
- Use object.size() to estimate the amount of memory an R object is consuming http://bit.ly/1dYFk2m 
- Type objects() at the console to see what variables, funcions and data sets you have defined http://bit.ly/1rd8cJF 
- Look here (http://www.revolutionanalytics.com/r-language-resources) for a fairly comprehensive list of R #rstats resources.
- x <- R.version$version.string assign a character string containing the version of R you are running http://bit.ly/1lDnbNz 
- Use the xtable package to convert a table or matrix to HTML: print(xtable(X), type="html") http://bit.ly/zrvgHJ 
- The lubridate package simplifies operations on dates and times: http://bit.ly/zIdY6J
- Generate a sequence of dates: seq(as.Date("2000/1/1"), as.Date("2014/1/1"), "years") http://bit.ly/1p2yZa6 
- Get the current date with Sys.Date() and the time with Sys.time() http://bit.ly/1psl2FW 
- sample(x) randomly shuffles the elements of a vector or list x #rstats http://bit.ly/x9UdLI 
- RevoJoe's updated list of data sets: http://bit.ly/1lcbZps
- "An Introduction to Statistical Learning" : (James et.al.) is full of R code and free to download http://bit.ly/1tPdef9 
- Nice short list of useful R functions from Alastair Sanderson http://bit.ly/1jBROMQ 
- Yes, you can return more than one value from a function: use a list! return(list(val1, val2, val3)) http://bit.ly/p6epvV 
- as.data.frame(installed.packages()[,c(1,3)],row.names=F) : list of installed packages with version bit.ly/RMvSYr
- Run help file examples for a particular function using the 'example' function: example(plot) or example(lm) http://bit.ly/1hGXhlB
- Concatenate elements of a vector to a single comma-separated string: paste(letters, collapse=", ") http://bit.ly/LEU8S0 
- Some nice R tutorials from Willian B. King http://bit.ly/1opEO26
- Leave out the mth column of a data frame: dF[,-m]  http://bit.ly/1pkampQ
- readHTMLTable(url,which=2) will read the 2nd table on the webpage specified by the url http://bit.ly/1jEUopG
- Indexing into a list: myList[[m]][n] will yield the nth element of the mth item in list myList #rstats http://bit.ly/1pkampQ
- Not sure why your R function threw an error? Use traceback() to find out where it occurred. http://bit.ly/qokamy
- Tutorial introduction to time series in #rstats: http://bit.ly/PG1f0n
- To speed an R function, use cmpfun to byte-compile it: http://bit.ly/t2OR0K
- If x is a matrix, vector or list then x[]<-0 replaces all its values by 0. More uses of the empty bracket in #rstats: http://bit.ly/14E9ulW 
- A tutorial on how the mboost package can help you use gradient boosting to fit a prediction model http://bit.ly/1uL1iyk
- Can't find a package binary on CRAN? Check its build status on all platforms here: http://cran.r-project.org/web/checks/check_summary.html 
- The rmongodb package - an interface to the open-source, NoSQL MongoDB database. Great vignettes http://bit.ly/1fXg5w9  
- The RSQLite package embeds the SQLite DB engine in R - great way to get started with DBMs http://bit.ly/RdBfjx 
- The sqldf package lets you do sql queries on an R dataframe http://bit.ly/Rdz6V0  #rstats
- The RMySQL package is a database interface and MySQL driver for R http://bit.ly/1mnh6Wv 
- The RODBC package provides access to SQL databases - Get started with the vignette http://bit.ly/PZosQF
- search() displays the "search list" of packages where R searches for functions and other objects #rstats http://www.inside-r.org/r-doc/base/search 
- Use NROW/NCOL instead of nrow/ncol to treat vectors as 1-column matrices http://bit.ly/HDgsfu 
- signif(x,digits=n) rounds x to n significant digits http://bit.ly/1iW0MrP
- Some examples of get() to call an R object using a character string: http://rfunction.com  (http://bit.ly/1mI1RnG ) 
- Annotate ggplot2 graphs by creating an annotation layer with annotate() http://bit.ly/1rtSpXd 
- Explore the capabilities of your installed R packages with browseVignettes() http://bit.ly/I2UQ0v 
- Avoid namespace clashes! Use :: to specify an object within a specific package, e.g. MASS::mvnorm http://bit.ly/Hft5M9 
- Merge two data frames with merge(df1, df2) for an inner join and merge(df1,df2,all=TRUE) for an outer join. http://bit.ly/ILpoTs 
- Determine the class of every variable in a data frame: sapply(df,class) http://bit.ly/1hUTWmv
- The WDI package provides access to the World Bank's World Development Indicators http://bit.ly/PdIWVF
- How to unshorten a bit.ly, t.co or other short URL with R: https://tonybreyal.wordpress.com/2011/12/13/unshorten-any-url-created-using-url-shortening-services-decode_shortened_url/
- Use the plyr package to easily sort a data frame by a column: arrange(df, desc(colname)) http://bit.ly/IyE1Wa
- For base graphics, par(mfrow=c(m,n)) will divide a graphics window into a grid of m x n plots http://bit.ly/1hHnLSE 
- cor.test(x,y) will tell you if the correlation between vectors x and y is statistically significant http://bit.ly/RaEOHM 
- Use mapply to call a multi-argument function repeatedly, e.g. mapply(sample, list(1:56, 1:46), c(5,1)) http://bit.ly/H9AbSs
- Define your own binary operators http://bit.ly/QMrRnl
- X %in% Y ia the same as !is.na(match(X,Y)) #rstats http://bit.ly/1q1Pkej
- Use package XLConnect to read and write Excel files http://bit.ly/1dYTDqR
- Deleting a function from a package will surprise your users. Deprecate it instead: http://bit.ly/100gwhr  (via @TrestleJeff) 
- Locate the largest number in a vector with which.max(mydata) http://bit.ly/Y21xrG (via @M_T_Patterson) #rstats
- 2 short tutorials for the reshape2 package: http://bit.ly/1rKRWkC and http://bit.ly/1fsF1Lz
- Type ?Startup for a description of R's initialization process and how to configure it: http://bit.ly/pYrMeM #rstats
- Install an R package directly from GitHub with devtools package: http://bit.ly/yUSrZr
- Sort a vector into decreasing order: sort(x,decreasing=TRUE) #rstats http://bit.ly/1hkfw0r
- Use the with() function to write R code using variables in a data frame, without needing the $ syntax: #rstats http://bit.ly/onqu7V
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


TODO: 
- Most of the links point to insideR which does not helps much. Add some code snippets and link to it. 
- Difference betweeen different read functions. read.delim;
