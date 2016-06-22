# Courses and books.

Can write down important learnings from it which are worth reading again. 


## Books 
- [ggplot2 - Hadley](https://onedrive.live.com/redir?resid=46DDE5F519C073A0!15834&authkey=!AG9X4A8YaCPSntY&ithint=file%2cpdf)   [s3](https://s3.amazonaws.com/storagecheckpersonal/ggplot2-book.pdf)


## [Startup Engineering](startup-001)
- [Env Path and Home.md](startup-001\Env Path and Home.md)
- [First application.md](startup-001\First application.md)
- [Windows Powershell commands.md](startup-001\Windows Powershell commands.md)
- [ssh.md](startup-001\ssh.md)


## rcourse [github](https://github.com/pdparker/rcourse)

[notebooks]()
- [Plotting data in R](http://nbviewer.jupyter.org/github/pdparker/rcourse/blob/master/Plots.ipynb#explore)
- [How to Run Basis Analysis in R](http://nbviewer.jupyter.org/github/pdparker/rcourse/blob/master/Regression%20in%20R.ipynb)
- [Missing data](http://nbviewer.jupyter.org/github/pdparker/rcourse/blob/master/Missing%20Data.ipynb)

- [Makefile](https://github.com/pdparker/rcourse/blob/master/lectures/Makefile) to create slides and cleanup repository
- [Basics of debugging](https://github.com/pdparker/rcourse/blob/master/lectures/Day1Part2-session3.md#basics-of-debugging)
- [Writing your own functions](https://github.com/pdparker/rcourse/blob/master/lectures/Day1Part2-session2.md#writing-your-own-functions)
- [Basic Descriptives](https://github.com/pdparker/rcourse/blob/master/lectures/Day1Part2-session1.md#basic-descriptives---single-variable)
- [Example data](https://github.com/pdparker/rcourse/blob/master/lectures/Day1Part1-session3.md#example-data)
- [R basics](https://github.com/pdparker/rcourse/blob/master/lectures/Day1Part1-session2.md#r-basics-the-boring-bits---introduction-to-object-orientated-programming)

Top tips from this [notebook](http://nbviewer.jupyter.org/github/pdparker/rcourse/blob/master/Plots.ipynb#explore) 
- Know the comands for plot, abline, line, hist, density, and curve to quickly generate graphs for exploration and production
- pairs and corrplot will let you look at lots of pairwise relationships quickly
- Start simply with default plot settings and slowly build up
- For likert data use jitter to move points around a bit to better see relationships
- For large data use alpha blending to make points semi transperent to better see where the density of points is (see alpha or rgb functions)
- For big data use heat maps, ggobi, bigvis, or hexbin plots
- Learn ggplots as this seems to be where everyone is heading
- Many many functions have default plots so try throwing output from a function at plot(). For models this will often be plots of assumptions