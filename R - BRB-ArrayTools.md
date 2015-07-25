
[Source](http://webcache.googleusercontent.com/search?q=cache:GoLoejt_LZAJ:taichimd.us/mediawiki/index.php/R&amp;hl=en&amp;gl=us&amp;strip=0&amp;vwsrc=0 "Permalink to R - BRB-ArrayTools")

# R - BRB-ArrayTools

## Install Rtools for Windows users

See <http: goo.gl="" gyh6c=""> for a step-by-step instruction with screenshot.

My preferred way is not to check the option of setting PATH environment. But I manually add the followings to the PATH environment (based on Rtools v3.0)

    c:Rtoolsbin;
    c:Rtoolsgcc-4.6.3bin;
    C:Program FilesRR-2.15.2bini386;

We can make our life easy by creating a file <rcommand.bat> with the content (also useful if you have C:cygwinbin in your PATH although cygwin setup will not do it automatically for you.)

PS. I put <rcommand.bat> under C:Program FilesR folder. I create a shortcut called 'Rcmd' on desktop. I enter **C:WindowsSystem32cmd.exe /K "Rcommand.bat"** in the _Target_ entry and **"C:Program FilesR"** in _Start in_ entry.

    @echo off
    set PATH=C:Rtoolsbin;c:Rtoolsgcc-4.6.3bin;%PATH%
    set PATH=C:Program FilesRR-2.15.2bini386;%PATH%
    set PKG_LIBS=`Rscript -e "Rcpp:::LdFlags()"`
    set PKG_CPPFLAGS=`Rscript -e "Rcpp:::CxxFlags()"`
    echo Setting environment for using R
    cmd

So we can open the Command Prompt anywhere and run <rcommand.bat> to get all environment variables ready! On Windows Vista, 7 and 8, we need to run it as administrator. OR we can change the security of the property so the current user can have an executive right.

### [Windows Toolset][1]

Note that R on Windows supports [Mingw-w64][2] (not Mingw which is a separate project). See [here][3] for the issue of developing a Qt application that links against R using Rcpp. And <http: qt-project.org="" wiki="" mingw=""> is the wiki for compiling Qt using MinGW and MinGW-w64.

### Build R from its source

Reference: <http: cran.r-project.org="" doc="" manuals="" r-admin.html#installing-r-under-windows="">

Download tcl file from <http: www.stats.ox.ac.uk="" pub="" rtools="" r_tcl_8-5-8.zip=""> Unzip and put 'Tcl' into R_HOME folder.

"Open a command prompt as Administrator"

Below it is supposed we have created a directory C:R and the source tar ball is placed under this directory.

    set PATH=c:Rtoolsbin;c:Rtoolsgcc-4.6.3bin;%PATH%

    cd C:R
    tar xzvf R-3.0.2.tar.gz

    cd R-3.0.2srcgnuwin32

    set TMPDIR=C:/tmp

    make all recommended

This builds R (3.0.2) and all recommended packages.

See also [Create_a_standalone_Rmath_library][4] below about how to create and use a standalone Rmath library in your own C/C++/Fortran program. For example, if you want to know the 95-th percentile of a T distribution or generate a bunch of random variables, you don't need to search internet to find a library; you can just use Rmath library.

### Compile and install an R package

**Command line**

    cd C:Documents and Settingsbrb
    wget http://www.bioconductor.org/packages/2.11/bioc/src/contrib/affxparser_1.30.2.tar.gz
    C:progra~1rr-2.15.2binR CMD INSTALL --build affxparser_1.30.2.tar.gz

**R console**

    install.packages("C:/Users/USERNAME/Downloads/DESeq2paper_1.3.tar.gz", repos=NULL, type="source")

See Chapter 6 of [R Installation and Administration][5]

### Check/Upload to CRAN

<http: win-builder.r-project.org="">

### 64 bit toolchain

See January 2010 email <https: stat.ethz.ch="" pipermail="" r-devel="" 2010-january="" 056301.html=""> and [R-Admin manual][6].

From R 2.11.0 there is 64 bit Windows binary for R.

## Install R using binary package on Linux OS

### Ubuntu/Debian

    sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E084DAB9
    sudo nano /etc/apt/sources.list
    # For Ubuntu 14.04 (codename is trusty)
    # deb http://cran.rstudio.com/bin/linux/ubuntu trusty/
    sudo apt-get update
    sudo apt-get install r-base

### Redhat el6

It should be pretty easy to install via the EPEL: <http: fedoraproject.org="" wiki="" epel="">

Just follow the instructions to enable the EPEL and then from the CLI as root:

    yum install R

or via sudo:

    sudo yum install R

## Install R from source (ix86, x86_64 and arm platforms, Linux system)

### Debian system (focus on arm architecture with notes from x86 system)

#### Simplest configuration

On my debian system in [Pogoplug][7] (armv5), [Raspberry Pi][8] (armv6) OR [Beaglebone Black][9] &amp; [Udoo][10](armv7), I can compile R. See R's [admin manual][11]. If the OS needs x11, I just need to install 2 required packages.

* install gfortran: **apt-get install build-essential gfortran** (gfortran is not part of build-essential)
* install readline library: **apt-get install libreadline5-dev** (pogoplug), **apt-get install libreadline6-dev** (raspberry pi/BBB), **apt-get install libreadline-dev** (Ubuntu)

Note: if I need X11, I should install

* libX11 and libX11-devel, libXt, libXt-devel (for fedora)
* **libx11-dev** (for debian) or **xorg-dev** (for pogoplug/raspberry pi/BBB)

and optional

* **texinfo** (to fix 'WARNING: you cannot build info or HTML versions of the R manuals')

Note that it is also safe to install required tools via (please run nano /etc/apt/sources.list to include the repository of your favorite R mirror and also run sudo apt-get update first)

    sudo apt-get build-dep r-base

The above command will install R dependence like jdk, tcl, tex, etc. The _apt-get build-dep_ gave a more complete list than _apt-get install r-base-dev_ for some reasons.

[Arm architecture] I also run **apt-get install readline-common**. I don't know if this is necessary. If x11 is not needed or not available (eg Pogoplug), I can add **\--with-x=no** option in ./configure command. If R will be called from other applications such as [Rserve][12], I can add **\--enable-R-shlib** option in ./configure command. Check out _./configure --help_ to get a complete list of all options.

After running

    wget http://cran.r-project.org/src/base/R-2/R-2.15.2.tar.gz
    tar xzvf R-2.15.2.tar.gz
    cd R-2.15.2
    ./configure --enable-R-shlib

(**\--enable-R-shlib** option will create a shared R library **libR.so** in $RHOME/lib subdirectory. This allows R to be embedded in other applications. See Embedding R.) I got

    R is now configured for armv5tel-unknown-linux-gnueabi

      Source directory:          .
      Installation directory:    /usr/local

      C compiler:                gcc -std=gnu99  -g -O2
      Fortran 77 compiler:       gfortran  -g -O2

      C++ compiler:              g++  -g -O2
      Fortran 90/95 compiler:    gfortran -g -O2
      Obj-C compiler:

      Interfaces supported:
      External libraries:        readline
      Additional capabilities:   NLS
      Options enabled:           shared R library, shared BLAS, R profiling

      Recommended packages:      yes

    configure: WARNING: you cannot build info or HTML versions of the R manuals
    configure: WARNING: you cannot build PDF versions of the R manuals
    configure: WARNING: you cannot build PDF versions of vignettes and help pages
    configure: WARNING: I could not determine a browser
    configure: WARNING: I could not determine a PDF viewer

After that, we can run **make** to create R binary. If the computer has multiple cores, we can run _make_ in parallel by using the **-j** flag (for example, '-j 4' means to run 4 jobs simultaneously). We can also add **time** command in front of _make_ to report the _make_ time (useful for benchmark).

    make
    # make -j 4
    # time make

PS 1. On my raspberry pi machine, it shows **R is now configured for armv6l-unknown-linux-gnueabihf** and on Beaglebone black it shows **R is now configured for armv7l-unknown-linux-gnueabihf**.

PS 2. On my Beaglebone black, it took 2 hours to run 'make' and it only took 5 minutes to run 'make -j 12' on my Xeon W3690 @ 3.47Ghz (6 cores with hyperthread) based on R 3.1.2. The timing is obtained by using 'time' command as described above.

PS 3. On my x86 system, it shows

    R is now configured for x86_64-unknown-linux-gnu

      Source directory:          .
      Installation directory:    /usr/local

      C compiler:                gcc -std=gnu99  -g -O2
      Fortran 77 compiler:       gfortran  -g -O2

      C++ compiler:              g++  -g -O2
      Fortran 90/95 compiler:    gfortran -g -O2
      Obj-C compiler:

      Interfaces supported:      X11, tcltk
      External libraries:        readline, lzma
      Additional capabilities:   PNG, JPEG, TIFF, NLS, cairo
      Options enabled:           shared R library, shared BLAS, R profiling, Java

      Recommended packages:      yes

[arm] <strike>However, **make** gave errors for recommanded packages like KernSmooth, MASS, boot, class, cluster, codetools, foreign, lattice, mgcv, nlme, nnet, rpart, spatial, and survival. The error stems from **gcc: SHLIB_LIBADD: No such file or directory**. Note that I can get this error message even I try **install.packages("MASS", type="source")**. A suggested fix is [here][13]; adding **perl = TRUE** in sub() call for two lines in **src/library/tools/R/install.R** file. However, I got another error **shared object 'MASS.so' not found**. See also <http: ftp.debian.org="" debian="" pool="" main="" r="" r-base="">. </http:></strike>To build R without recommended packages like **./configure --without-recommended**.

    make[1]: Entering directory `/mnt/usb/R-2.15.2/src/library/Recommended'
    make[2]: Entering directory `/mnt/usb/R-2.15.2/src/library/Recommended'
    begin installing recommended package MASS
    * installing *source* package 'MASS' ...
    ** libs
    make[3]: Entering directory `/tmp/Rtmp4caBfg/R.INSTALL1d1244924c77/MASS/src'
    gcc -std=gnu99 -I/mnt/usb/R-2.15.2/include -DNDEBUG  -I/usr/local/include    -fpic  -g -O2  -c MASS.c -o MASS.o
    gcc -std=gnu99 -I/mnt/usb/R-2.15.2/include -DNDEBUG  -I/usr/local/include    -fpic  -g -O2  -c lqs.c -o lqs.o
    gcc -std=gnu99 -shared -L/usr/local/lib -o MASSSHLIB_EXT MASS.o lqs.o SHLIB_LIBADD -L/mnt/usb/R-2.15.2/lib -lR
    gcc: SHLIB_LIBADD: No such file or directory
    make[3]: *** [MASSSHLIB_EXT] Error 1
    make[3]: Leaving directory `/tmp/Rtmp4caBfg/R.INSTALL1d1244924c77/MASS/src'
    ERROR: compilation failed for package 'MASS'
    * removing '/mnt/usb/R-2.15.2/library/MASS'
    make[2]: *** [MASS.ts] Error 1
    make[2]: Leaving directory `/mnt/usb/R-2.15.2/src/library/Recommended'
    make[1]: *** [recommended-packages] Error 2
    make[1]: Leaving directory `/mnt/usb/R-2.15.2/src/library/Recommended'
    make: *** [stamp-recommended] Error 2
    root@debian:/mnt/usb/R-2.15.2#
    root@debian:/mnt/usb/R-2.15.2# bin/R

    R version 2.15.2 (2012-10-26) -- "Trick or Treat"
    Copyright (C) 2012 The R Foundation for Statistical Computing
    ISBN 3-900051-07-0
    Platform: armv5tel-unknown-linux-gnueabi (32-bit)

    R is free software and comes with ABSOLUTELY NO WARRANTY.
    You are welcome to redistribute it under certain conditions.
    Type 'license()' or 'licence()' for distribution details.

    R is a collaborative project with many contributors.
    Type 'contributors()' for more information and
    'citation()' on how to cite R or R packages in publications.

    Type 'demo()' for some demos, 'help()' for on-line help, or
    'help.start()' for an HTML browser interface to help.
    Type 'q()' to quit R.

    &gt; library(MASS)
    Error in library(MASS)&nbsp;: there is no package called 'MASS'
    &gt; library()
    Packages in library '/mnt/usb/R-2.15.2/library':

    base                    The R Base Package
    compiler                The R Compiler Package
    datasets                The R Datasets Package
    grDevices               The R Graphics Devices and Support for Colours
                            and Fonts
    graphics                The R Graphics Package
    grid                    The Grid Graphics Package
    methods                 Formal Methods and Classes
    parallel                Support for Parallel computation in R
    splines                 Regression Spline Functions and Classes
    stats                   The R Stats Package
    stats4                  Statistical Functions using S4 Classes
    tcltk                   Tcl/Tk Interface
    tools                   Tools for Package Development
    utils                   The R Utils Package
    &gt; Sys.info()["machine"]
       machine
    "armv5tel"
    &gt; gc()
             used (Mb) gc trigger (Mb) max used (Mb)
    Ncells 170369  4.6     350000  9.4   350000  9.4
    Vcells 163228  1.3     905753  7.0   784148  6.0

See <http: bugs.debian.org="" cgi-bin="" bugreport.cgi?bug="679180">

PS 4. The complete log of building R from source is in here [File:Build R log.txt][14]

#### Full configuration

      Interfaces supported:      X11, tcltk
      External libraries:        readline
      Additional capabilities:   PNG, JPEG, TIFF, NLS, cairo
      Options enabled:           shared R library, shared BLAS, R profiling, Java

#### Update: R 3.0.1 on Beaglebone Black (armv7a) + Ubuntu 13.04

See the page [here][15].

#### Update: R 3.1.3 &amp; R 3.2.0 on Raspberry Pi 2

It took 134m to run 'make -j 4' on RPi 2 using R 3.1.3.

But I got an error when I ran './configure; make -j 4' using R 3.2.0. The errors start from compiling <main connections.c=""> file with 'undefined reference to ....'. The gcc version is 4.6.3.

### Install all dependencies for building R

This is a comprehensive list. This list is even larger than r-base-dev.

    root@debian:/mnt/usb/R-2.15.2# apt-get build-dep r-base
    Reading package lists... Done
    Building dependency tree
    Reading state information... Done
    The following packages will be REMOVED:
      libreadline5-dev
    The following NEW packages will be installed:
      bison ca-certificates ca-certificates-java debhelper defoma ed file fontconfig gettext
      gettext-base html2text intltool-debian java-common libaccess-bridge-java
      libaccess-bridge-java-jni libasound2 libasyncns0 libatk1.0-0 libaudit0 libavahi-client3
      libavahi-common-data libavahi-common3 libblas-dev libblas3gf libbz2-dev libcairo2
      libcairo2-dev libcroco3 libcups2 libdatrie1 libdbus-1-3 libexpat1-dev libflac8
      libfontconfig1-dev libfontenc1 libfreetype6-dev libgif4 libglib2.0-dev libgtk2.0-0
      libgtk2.0-common libice-dev libjpeg62-dev libkpathsea5 liblapack-dev liblapack3gf libnewt0.52
      libnspr4-0d libnss3-1d libogg0 libopenjpeg2 libpango1.0-0 libpango1.0-common libpango1.0-dev
      libpcre3-dev libpcrecpp0 libpixman-1-0 libpixman-1-dev libpng12-dev libpoppler5 libpulse0
      libreadline-dev libreadline6-dev libsm-dev libsndfile1 libthai-data libthai0 libtiff4-dev
      libtiffxx0c2 libunistring0 libvorbis0a libvorbisenc2 libxaw7 libxcb-render-util0
      libxcb-render-util0-dev libxcb-render0 libxcb-render0-dev libxcomposite1 libxcursor1
      libxdamage1 libxext-dev libxfixes3 libxfont1 libxft-dev libxi6 libxinerama1 libxkbfile1
      libxmu6 libxmuu1 libxpm4 libxrandr2 libxrender-dev libxss-dev libxt-dev libxtst6 luatex m4
      openjdk-6-jdk openjdk-6-jre openjdk-6-jre-headless openjdk-6-jre-lib openssl pkg-config
      po-debconf preview-latex-style shared-mime-info tcl8.5-dev tex-common texi2html texinfo
      texlive-base texlive-binaries texlive-common texlive-doc-base texlive-extra-utils
      texlive-fonts-recommended texlive-generic-recommended texlive-latex-base texlive-latex-extra
      texlive-latex-recommended texlive-pictures tk8.5-dev tzdata-java whiptail x11-xkb-utils
      x11proto-render-dev x11proto-scrnsaver-dev x11proto-xext-dev xauth xdg-utils xfonts-base
      xfonts-encodings xfonts-utils xkb-data xserver-common xvfb zlib1g-dev
    0 upgraded, 136 newly installed, 1 to remove and 0 not upgraded.
    Need to get 139 MB of archives.
    After this operation, 410 MB of additional disk space will be used.
    Do you want to continue [Y/n]?

### Instruction of installing a development version of R under Ubuntu

<https: github.com="" wch="" r-source="" wiki=""> (works on Ubuntu 12.04)

Note that texi2dvi has to be installed first to avoid the following error. It is better to follow the Ubuntu instruction (<https: github.com="" wch="" r-source="" wiki="" ubuntu-build-instructions="">) when we work on Ubuntu OS.

    $ (cd doc/manual &amp;&amp; make front-matter html-non-svn)
    creating RESOURCES
    /bin/bash: number-sections: command not found
    make: [../../doc/RESOURCES] Error 127 (ignored)

To build R, run the following script. To run the built R, type 'bin/R'.

    # Get recommended packages if necessary
    tools/rsync-recommended

    R_PAPERSIZE=letter
    R_BATCHSAVE="--no-save --no-restore"
    R_BROWSER=xdg-open
    PAGER=/usr/bin/pager
    PERL=/usr/bin/perl
    R_UNZIPCMD=/usr/bin/unzip
    R_ZIPCMD=/usr/bin/zip
    R_PRINTCMD=/usr/bin/lpr
    LIBnn=lib
    AWK=/usr/bin/awk
    CC="ccache gcc"
    CFLAGS="-ggdb -pipe -std=gnu99 -Wall -pedantic"
    CXX="ccache g++"
    CXXFLAGS="-ggdb -pipe -Wall -pedantic"
    FC="ccache gfortran"
    F77="ccache gfortran"
    MAKE="make"
    ./configure
        --prefix=/usr/local/lib/R-devel
        --enable-R-shlib
        --with-blas
        --with-lapack
        --with-readline

    #CC="clang -O3"
    #CXX="clang++ -03"

    # Workaround for explicit SVN check introduced by
    # https://github.com/wch/r-source/commit/4f13e5325dfbcb9fc8f55fc6027af9ae9c7750a3

    # Need to build FAQ
    (cd doc/manual &amp;&amp; make front-matter html-non-svn)

    rm -f non-tarball

    # Get current SVN revsion from git log and save in SVN-REVISION
    echo -n 'Revision: ' &gt; SVN-REVISION
    git log --format=%B -n 1
      | grep "^git-svn-id"
      | sed -E 's/^git-svn-id: https://svn.r-project.org/R/.*?@([0-9]+).*$/1/'
      &gt;&gt; SVN-REVISION
    echo -n 'Last Changed Date: ' &gt;&gt;  SVN-REVISION
    git log -1 --pretty=format:"%ad" --date=iso | cut -d' ' -f1 &gt;&gt; SVN-REVISION

    # End workaround

    # Set this to the number of cores on your computer
    make --jobs=4

If we DO NOT use -depth option in git clone command, we can use git checkout SHA1 (40 characters) to get a certain version of code.

    git checkout f1d91a0b34dbaa6ac807f3852742e3d646fbe95e  # plot(<dendrogram>): Bug 15215 fixed 5/2/2015
    git checkout trunk                                     # switch back to trunk

The svn revision number for a certain git revision can be found in the blue box on the github website (git-svn-id). For example, [this revision][16] has an svn revision number 68302 even the current trunk is 68319.

Now suppose we have run 'git check trunk', create a devel'R successfully. If we want to build R for a certain svn or git revision, we run 'git checkout SHA1', 'make distclean', code to generate the _SVN-REVISION_ file (it will update this number) and finally './configure' &amp; 'make'.

    time (./configure --with-recommended-packages=no &amp;&amp; make --jobs=5)

The timing is 4m36s if I skip recommended packages and 7m37s if I don't skip. This is based on Xeon W3690 @ 3.47GHz.

The full bash script is available on [Github Gist][17].

### Install multiple versions of R on Ubuntu

To install the devel version of R alongside the current version of R. See [this post][18]. For example you need a script that will build r-devel, but install it in a location different from the stable version of R (eg use --prefix=/usr/local/R-X.Y.Z in the _config_ command). Note that the executable is installed in "/usr/local/lib/R-devel/bin", but that can be changed to others like "/usr/local/bin".

Another fancy way is to use **docker**.

### Minimal installation of R from source

Assume we have installed g++ (or build-essential) and gfortran (Ubuntu has only gcc pre-installed, but not g++),

    sudo apt-get install build-essential gfortran

we can go ahead to build a minimal R.

    wget http://cran.rstudio.com/src/base/R-3/R-3.1.1.tar.gz
    tar -xzvf R-3.1.1.tar.gz; cd R-3.1.1
    ./configure --with-x=no --with-recommended-packages=no --with-readline=no

See ./configure --help. This still builds the essential packages like base, compiler, datasets, graphics, grDevices, grid, methods, parallel, splines, stats, stats4, tcltk, tools, and utils.

Note that at the end of 'make', it shows an error of 'cannot find any java interpreter. Please make sure java is on your PATH or set JAVA_HOME correspondingly'. Even with the error message, we can use R by typing bin/R.

To check whether we have Java installed, type 'java -version'.

    $ java -version
    java version "1.6.0_32"
    OpenJDK Runtime Environment (IcedTea6 1.13.4) (6b32-1.13.4-4ubuntu0.12.04.2)
    OpenJDK 64-Bit Server VM (build 23.25-b01, mixed mode)

### R CMD

* R CMD build someDirectory - create a package
* R CMD check somePackage_1.2-3.tar.gz - check a package
* R CMD INSTALL somePackage_1.2-3.tar.gz - install a package from its source

### bin/R (shell script) and bin/exec/R (binary executable) on Linux OS

**bin/R** is just a shell script to launch **bin/exec/R** program. So if we try to run the following program

    # test.R
    cat("-- reading argumentsn", sep = "");
    cmd_args = commandArgs();
    for (arg in cmd_args) cat("  ", arg, "n", sep="");

from command line like

    brb@brb-P45T-A:~/Downloads$ ~/R-3.0.1/bin/R --slave --no-save --no-restore --no-environ --silent --args arg1=abc &lt; test.R
    -- reading arguments
      /home/brb/R-3.0.1/bin/exec/R
      --slave
      --no-save
      --no-restore
      --no-environ
      --silent
      --args
      arg1=abc

we can see R actually call **bin/exec/R** program.

### Ubuntu/Debian

Since the R packages **XML** &amp; **RCurl** are frequently used by other packages (e.g. miniCRAN), it is useful to run the following so the _install.packages("RCurl")_ and _install.packages("XML")_ can work without hiccups.

    sudo apt-get update
    sudo apt-get install libxml2-dev
    sudo apt-get install libcurl4-openssl-dev

### CentOS 6.x

Install build-essential (make, gcc, gdb, ...).

    su
    yum groupinstall "Development Tools"
    yum install kernel-devel kernel-headers

Install readline and X11 (probably not necessary if we use **./configure --with-x=no**)

    yum install readline-devel
    yum install libX11 libX11-devel libXt libXt-devel

Install libpng (already there) and libpng-devel library. This is for web application purpose because png (and possibly svg) is a standard and preferred graphics format. If we want to output different graphics formats, we have to follow the guide in [R-admin manual][19] to install extra graphics libraries in Linux.

    yum install libpng-devel
    rpm -qa | grep "libpng"
    # make sure both libpng and libpng-devel exist.

Install Java. One possibility is to download from [Oracle][20]. We want to download jdk-7u45-linux-x64.rpm and jre-7u45-linux-x64.rpm (assume 64-bit OS).

    rpm -Uvh jdk-7u45-linux-x64.rpm
    rpm -Uvh jre-7u45-linux-x64.rpm
    # Check
    java -version

Now we are ready to build R by using "./configure" and then "make" commands.

We can make R accessible from any directory by either run "make install" command or creating an R_HOME environment variable and export it to PATH environment variable, such as

    export R_HOME="path to R"
    export PATH=$PATH:$R_HOME/bin

## Web Applications

See also CRAN Task View: [Web Technologies and Services][21]

### Create HTML5 web and slides using knitr and pandoc

<http: www.gastonsanchez.com="" depot="" knitr-slides="">. The HTML5 slides work on my IE 8 too.

HTML5 slides examples

Software requirement

Slide #22 gives an instruction to create

* regular html file by using RStudio -&gt; Knit HTML button
* HTML5 slides by using pandoc from command line.

Files:

* Rcmd source: [009-slides.Rmd][22] Note that IE 8 was not supported by github. For IE 9, be sure to turn off "Compatibility View".
* markdown output: 009-slides.md
* HTML output: 009-slides.html

We can create Rcmd source in Rstudio by File -&gt; New -&gt; R Markdown.

There are 4 ways to produce slides with pandoc

* S5
* DZSlides
* Slidy
* Slideous

Use the markdown file (md) and convert it with pandoc

    pandoc -s -S -i -t dzslides --mathjax html5_slides.md -o html5_slides.html

If we are comfortable with HTML and CSS code, open the html file (generated by pandoc) and modify the CSS style at will.

#### Examples

Some examples of creating papers (with references) based on knitr can be found on the [Papers and reports][23] section of the knitr website.

### Markdown language

According to [wikipedia][24]:

_Markdown is a lightweight markup language, originally created by John Gruber with substantial contributions from Aaron Swartz, allowing people "to write using an easy-to-read, easy-to-write plain text format, then convert it to structurally valid XHTML (or HTML)"._

* Markup is a general term for content formatting - such as HTML - but markdown is a library that generates HTML markup.
* Convert mediawiki to markdown using online conversion tool from [pandoc][25].

### [HTTP protocol][26]

An HTTP server is conceptually simple:

1. Open port 80 for listening
2. When contact is made, gather a little information (get mainly - you can ignore the rest for now)
3. Translate the request into a file request
4. Open the file and spit it back at the client

It gets more difficult depending on how much of HTTP you want to support - POST is a little more complicated, scripts, handling multiple requests, etc.

#### Example in R

    &gt; co &lt;- socketConnection(port=8080, server=TRUE, blocking=TRUE)
    &gt; # Now open a web browser and type http://localhost:8080/index.html
    &gt; readLines(co,1)
    [1] "GET /index.html HTTP/1.1"
    &gt; readLines(co,1)
    [1] "Host: localhost:8080"
    &gt; readLines(co,1)
    [1] "User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux i686; rv:23.0) Gecko/20100101 Firefox/23.0"
    &gt; readLines(co,1)
    [1] "Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8"
    &gt; readLines(co,1)
    [1] "Accept-Language: en-US,en;q=0.5"
    &gt; readLines(co,1)
    [1] "Accept-Encoding: gzip, deflate"
    &gt; readLines(co,1)
    [1] "Connection: keep-alive"
    &gt; readLines(co,1)
    [1] ""

#### Example in C ([Very simple http server written in C][27], 187 lines)

Create a simple hello world html page and save it as &lt;[index.html][28]&gt; in the current directory (/home/brb/Downloads/)

Launch the server program (assume we have done _gcc http_server.c -o http_server_)

    $ ./http_server -p 50002
    Server started at port no. 50002 with root directory as /home/brb/Downloads

Secondly open a browser and type <http: localhost:50002="" index.html="">. The server will respond

    GET /index.html HTTP/1.1
    Host: localhost:50002
    User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux i686; rv:23.0) Gecko/20100101 Firefox/23.0
    Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
    Accept-Language: en-US,en;q=0.5
    Accept-Encoding: gzip, deflate
    Connection: keep-alive

    file: /home/brb/Downloads/index.html
    GET /favicon.ico HTTP/1.1
    Host: localhost:50002
    User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux i686; rv:23.0) Gecko/20100101 Firefox/23.0
    Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
    Accept-Language: en-US,en;q=0.5
    Accept-Encoding: gzip, deflate
    Connection: keep-alive

    file: /home/brb/Downloads/favicon.ico
    GET /favicon.ico HTTP/1.1
    Host: localhost:50003
    User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux i686; rv:23.0) Gecko/20100101 Firefox/23.0
    Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
    Accept-Language: en-US,en;q=0.5
    Accept-Encoding: gzip, deflate
    Connection: keep-alive

    file: /home/brb/Downloads/favicon.ico

The browser will show the page from <index.html> in server.

The only bad thing is the code does not close the port. For example, if I have use Ctrl+C to close the program and try to re-launch with the same port, it will complain **socket() or bind(): Address already in use**.

#### Another Example in C (55 lines)

<http: mwaidyanatha.blogspot.com="" 2011="" 05="" writing-simple-web-server-in-c.html="">

The response is embedded in the C code.

If we test the server program by opening a browser and type "<http: localhost:15000="">", the server received the follwing 7 lines

    GET / HTTP/1.1
    Host: localhost:15000
    User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux i686; rv:23.0) Gecko/20100101 Firefox/23.0
    Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
    Accept-Language: en-US,en;q=0.5
    Accept-Encoding: gzip, deflate
    Connection: keep-alive

If we include a non-executable file's name in the url, we will be able to download that file. Try "<http: localhost:15000="" client.c="">".

If we use telnet program to test, wee need to type anything we want

    $ telnet localhost 15000
    Trying 127.0.0.1...
    Connected to localhost.
    Escape character is '^]'.
    ThisCanBeAnything        &lt;=== This is what I typed in the client and it is also shown on server
    HTTP/1.1 200 OK          &lt;=== From here is what I got from server
    Content-length: 37Content-Type: text/html

    HTML_DATA_HERE_AS_YOU_MENTIONED_ABOVE &lt;=== The html tags are not passed from server, interesting!
    Connection closed by foreign host.
    $

See also more examples under [C page][29].

#### Others

* <http: rosettacode.org="" wiki="" hello_world=""> (Different languages)
* <http: kperisetla.blogspot.com="" 2012="" 07="" simple-http-web-server-in-c.html=""> (Windows web server)
* <http: css.dzone.com="" articles="" web-server-c=""> (handling HTTP GET request, handling content types(txt, html, jpg, zip. rar, pdf, php etc.), sending proper HTTP error codes, serving the files from a web root, change in web root in a config file, zero copy optimization using sendfile method and php file handling.)
* <https: github.com="" gtungatkar="" simple-http-server="">
* <https: github.com="" davidmoreno="" onion="">

### [shiny][30]

The following is what we see on a browser after we run an example from shiny package. See <http: rstudio.github.com="" shiny="" tutorial="" #hello-shiny="">. Note that the R session needs to be on; i.e. R command prompt will not be returned unless we press Ctrl+C or ESC.

![ShinyHello.png][31] ![Shinympg.png][32] ![ShinyReactivity.png][33] ![ShinyTabsets.png][34] ![ShinyUpload.png][35]

shiny depends on [websockets][36], caTools, bitops, digest packages.

Q &amp; A:

* Q: If we run _runExample('01_hello')_ in Rserve from an R client, we can continue our work in R client without losing the functionality of the GUI from shiny. Question: how do we kill the job?
* If I run the example "01_hello", the browser only shows the control but not graph on Firefox? A: Use Chrome or Opera as the default browser.
* If I run the example "01_hello" on RHEL the first time, it works fine. But if I click 'Ctrl + C' to stop it and run it again, I got a message

    Warning in .SOCK_SERVE(port)&nbsp;: R-Websockets(tcpserv): bind() failed.
    Error in createContext(port, webpage, is.binary = is.binary)&nbsp;:
      Unable to bind socket on port 8100; is it realsy in use?

A simple solution is to close R and open it again.

* Q: Deployment on web. A: Not ready yet. Shiny server platform is still under beta testing. Shiny apps are hosted using the R websockets package which acts more like a tcp server than a web server, and that architecture just doesn't fit with rApache, or even apache for that matter.
* Q: How difficult to put the code in Gist:github? A: Just create an account. Do not even need to create a repository. Just go to <http: gist.github.com=""> and create a new gist. The new gist can be secret or public. A secret gist can not be edited again after it is created although it works fine when it was used in runGist() function.

#### Deploy to run locally

Follow the instruction [here][37], we can do as following (Tested on Windows OS)

1. Create a desktop shortcut with target **"C:Program FilesRR-3.0.2binR.exe" -e "shiny::runExample('01_hello')" **. We can name the shortcut as we like, e.g. **R+shiny**
2. Double click the shortcut. The Windows Firewall will be popped up and say it block some features of the program. It does not matter if we choose Allow access or Cancel.
3. Look at the command prompt window (black background console window), it will say something like

        Listening on port 7510

at the last line of the console.
4. Open your browser (Chrome or Firefox works), and type the address **<http: localhost:7510="">**. You will see something magic happen.
5. If we don't want to play with it, we can close the browser and close the command console (hit 'x')too.

#### Deploy to run remotely -shiny server

If we want to deploy our shiny apps to WWW, we need to install [shiny server][38].

Following the guide on [here][39], shiny-server is up smoothly on my Ubuntu machine. After I run the command **sudo gdebi shiny-server-0.4.0.8-amd64.deb**, shiny-server is started. Thanks to **upstart** in Ubuntu, shiny-server is automatically started whenever the machine is started.

Each app directory needs to be copied to **/srv/shiny-server/** directory using sudo.

The default port is 3838. That is, the remote computer can access the website using <http: xxx.xxx.x.xx:3838="" appname="">.

Last but not the least, according to its web page, shiny-server is **Experimental quality. Use at your own risk!**.

#### [shinydashboard][40]

#### [shinyapps.io][41]

<http: www.rstudio.com="" products="" shinyapps="">

#### websocket

<http: illposed.net="" jsm2012.pdf="">

#### CentOS

<http: blog.supstat.com="" 2014="" 05="" install-rstudio-server-on-centos6-5="">

#### Gallery

### [httpuv][42]

http and WebSocket library.

### [RApache][43]

### [gWidgetsWWW][44]

### [Rook][45]

Since R 2.13, the internal web server was exposed.

[Tutorual from useR2012][46] and [Jeffrey Horner][47]

Here is another [one][48] from <http: www.rinfinance.com="">.

Rook is also supported by [rApache too. See <http: rapache.net="" manual.html="">.

Google group. <https: groups.google.com="" forum="" ?fromgroups#!forum="" rrook="">

Advantage

* the web applications are created on desktop, whether it is Windows, Mac or Linux.
* No Apache is needed.
* create [multiple applications][49] at the same time. This complements the limit of rApache.
* * *

4 lines of code [example][50].

    library(Rook)
    s &lt;- Rhttpd$new()
    s$start(quiet=TRUE)
    s$print()
    s$browse(1)  # OR s$browse("RookTest")

Notice that after s$browse() command, the cursor will return to R because the command just a shortcut to open the web page <http: 127.0.0.1:10215="" custom="" rooktest="">.

![Rook.png][51] ![Rook2.png][52] ![Rookapprnorm.png][53]

We can add Rook **application** to the server; see&nbsp;?Rhttpd.

    s$add(
        app=system.file('exampleApps/helloworld.R',package='Rook'),name='hello'
    )
    s$add(
        app=system.file('exampleApps/helloworldref.R',package='Rook'),name='helloref'
    )
    s$add(
        app=system.file('exampleApps/summary.R',package='Rook'),name='summary'
    )

    s$print()

    #Server started on 127.0.0.1:10221
    #[1] RookTest http://127.0.0.1:10221/custom/RookTest
    #[2] helloref http://127.0.0.1:10221/custom/helloref
    #[3] summary  http://127.0.0.1:10221/custom/summary
    #[4] hello    http://127.0.0.1:10221/custom/hello

    #  Stops the server but doesn't uninstall the app
    ## Not run:
    s$stop()

    ## End(Not run)
    s$remove(all=TRUE)
    rm(s)

For example, the interface and the source code of _summary_ app are given below

![Rookappsummary.png][54]

    app &lt;- function(env) {
        req &lt;- Rook::Request$new(env)
        res &lt;- Rook::Response$new()
        res$write('Choose a CSV file:n')
        res$write('<form method="POST" enctype="multipart/form-data">n')
        res$write('<input type="file" name="data">n')
        res$write('<input type="submit" name="Upload">n</form>n<br>')

        if (!is.null(req$POST())){
    	data &lt;- req$POST()[['data']]
    	res$write("<h3>Summary of Data</h3>");
    	res$write("<pre>")
    	res$write(paste(capture.output(summary(read.csv(data$tempfile,stringsAsFactors=FALSE)),file=NULL),collapse='n'))
    	res$write("</pre>")
    	res$write("<h3>First few lines (head())</h3>");
    	res$write("<pre>")
    	res$write(paste(capture.output(head(read.csv(data$tempfile,stringsAsFactors=FALSE)),file=NULL),collapse='n'))
    	res$write("</pre>")
        }
        res$finish()
    }

More example:

### [sumo][55]

Sumo is a fully-functional web application template that exposes an authenticated user's R session within java server pages. See the paper <http: journal.r-project.org="" archive="" 2012-1="" rjournal_2012-1_bergsma+smith.pdf="">.

### [Stockplot][56]

### [FastRWeb][57]

### [Rwui][58]

### [CGHWithR][59] and [WebDevelopR][60]

CGHwithR is still working with old version of R although it is removed from CRAN. Its successor is WebDevelopR. Its The vignette (year 2013) provides a review of several available methods.

### [manipulate][61] from RStudio

This is not a web application. But the **manipulate** package can be used to create interactive plot within R(Studio) environment easily. Its source is available at [here][62].

Mathematica also has manipulate function for plotting; see [here][63].

### [RCloud][64]

RCloud is an environment for collaboratively creating and sharing data analysis scripts. RCloud lets you mix analysis code in R, HTML5, Markdown, Python, and others. Much like Sage, iPython notebooks and Mathematica, RCloud provides a notebook interface that lets you easily record a session and annotate it with text, equations, and supporting images.

See also the [Talk][65] in UseR 2014.

### Web page scraping

[rvest][66] package.

### Send email

### [GEO (Gene Expression Omnibus)][67]

See [this internal link][68].

### Interactive html output

#### [sendplot][69]

#### [RIGHT][70]

The supported plot types include scatterplot, barplot, box plot, line plot and pie plot.

In addition to tooltip boxes, the package can create a [table showing all information about selected nodes][71].

#### [d3Network][72]

    library(d3Network)

    Source &lt;- c("A", "A", "A", "A", "B", "B", "C", "C", "D")
    Target &lt;- c("B", "C", "D", "J", "E", "F", "G", "H", "I")
    NetworkData &lt;- data.frame(Source, Target)

    d3SimpleNetwork(NetworkData, height = 800, width = 1024, file="tmp.html")

#### [htmlwidgets for R][73]

Embed widgets in R Markdown documents and Shiny web applications.

#### DT: An R interface to the DataTables library

## Creating local repository for CRAN and Bioconductor (focus on Windows binary packages only)

### How to set up a local repository

General guide: <http: cran.r-project.org="" doc="" manuals="" r-admin.html#setting-up-a-package-repository="">

Utilities such as install.packages can be pointed at any CRAN-style repository, and R users may want to set up their own. The 'base' of a repository is a URL such as <http: www.omegahat.org="" r="">: this must be an URL scheme that download.packages supports (which also includes '<ftp: '=""> and 'file://', but not on most systems '<https: '="">). **Under that base URL there should be directory trees for one or more of the following types of package distributions:**

* "source": located at src/contrib and containing .tar.gz files. Other forms of compression can be used, e.g. .tar.bz2 or .tar.xz files.
* **"win.binary": located at bin/windows/contrib/x.y for R versions x.y.z and containing .zip files for Windows.**
* "mac.binary.leopard": located at bin/macosx/leopard/contrib/x.y for R versions x.y.z and containing .tgz files.

Each terminal directory must also contain a PACKAGES file. This can be a concatenation of the DESCRIPTION files of the packages separated by blank lines, but only a few of the fields are needed. The simplest way to set up such a file is to use function write_PACKAGES in the tools package, and its help explains which fields are needed. Optionally there can also be a PACKAGES.gz file, a gzip-compressed version of PACKAGES—as this will be downloaded in preference to PACKAGES it should be included for large repositories. (If you have a mis-configured server that does not report correctly non-existent files you will need PACKAGES.gz.)

To add your repository to the list offered by setRepositories(), see the help file for that function.

A repository can contain subdirectories, when the descriptions in the PACKAGES file of packages in subdirectories must include a line of the form

Path: path/to/subdirectory

—once again write_PACKAGES is the simplest way to set this up.

#### Space requirement if we want to mirror WHOLE repository

* Whole CRAN takes about 92GB (rsync -avn cran.r-project.org::CRAN &gt; ~/Downloads/cran).
* Bioconductor is big (&gt; 64G for BioC 2.11). Please check the size of what will be transferred with e.g. (rsync -avn bioconductor.org::2.11 &gt; ~/Downloads/bioc) and make sure you have enough room on your local disk before you start.

On the other hand, we if only care about Windows binary part, the space requirement is largely reduced.

* CRAN: 2.7GB
* Bioconductor: 28GB.

#### Misc notes

* If the binary package was built on R 2.15.1, then it cannot be installed on R 2.15.2. But vice is OK.
* Remember to issue "--delete" option in rsync, otherwise old version of package will be installed.
* The repository still need src directory. If it is missing, we will get an error

    Warning: unable to access index for repository http://arraytools.no-ip.org/CRAN/src/contrib
    Warning message:
    package 'glmnet' is not available (for R version 2.15.2)

The error was given by available.packages() function.

To bypass the requirement of src directory, I can use

    install.packages("glmnet", contriburl = contrib.url(getOption('repos'), "win.binary"))

but there may be a problem when we use biocLite() command.

I find a workaround. Since the error comes from missing CRAN/src directory, we just need to make sure the directory CRAN/src/contrib exists AND either CRAN/src/contrib/PACKAGES or CRAN/src/contrib/PACKAGES.gz exists.

#### To create CRAN repository

Before creating a local repository please give a dry run first. You don't want to be surprised how long will it take to mirror a directory.

Dry run (-n option). Pipe out the process to a text file for an examination.

    rsync -avn cran.r-project.org::CRAN &gt; crandryrun.txt

To mirror only partial repository, it is necessary to create directories before running rsync command.

    cd
    mkdir -p ~/Rmirror/CRAN/bin/windows/contrib/2.15
    rsync -rtlzv --delete cran.r-project.org::CRAN/bin/windows/contrib/2.15/ ~/Rmirror/CRAN/bin/windows/contrib/2.15
    (one line with space before ~/Rmirror)

    # src directory is very large (~27GB) since it contains source code for each R version.
    # We just need the files PACKAGES and PACKAGES.gz in CRAN/src/contrib. So I comment out the following line.
    # rsync -rtlzv --delete cran.r-project.org::CRAN/src/ ~/Rmirror/CRAN/src/
    mkdir -p ~/Rmirror/CRAN/src/contrib
    rsync -rtlzv --delete cran.r-project.org::CRAN/src/contrib/PACKAGES ~/Rmirror/CRAN/src/contrib/
    rsync -rtlzv --delete cran.r-project.org::CRAN/src/contrib/PACKAGES.gz ~/Rmirror/CRAN/src/contrib/

And optionally

    library(tools)
    write_PACKAGES("~/Rmirror/CRAN/bin/windows/contrib/2.15", type="win.binary")

and if we want to get src directory

    rsync -rtlzv --delete cran.r-project.org::CRAN/src/contrib/*.tar.gz ~/Rmirror/CRAN/src/contrib/
    rsync -rtlzv --delete cran.r-project.org::CRAN/src/contrib/2.15.3 ~/Rmirror/CRAN/src/contrib/

We can use **du -h** to check the folder size.

For example (as of 1/7/2013),

    $ du -k ~/Rmirror --max-depth=1 --exclude ".*" | sort -nr | cut -f2 | xargs -d 'n' du -sh
    30G	/home/brb/Rmirror
    28G	/home/brb/Rmirror/Bioc
    2.7G	/home/brb/Rmirror/CRAN

#### To create Bioconductor repository

Dry run

    rsync -avn bioconductor.org::2.11 &gt; biocdryrun.txt

Then creates directories before running rsync.

    cd
    mkdir -p ~/Rmirror/Bioc
    wget -N http://www.bioconductor.org/biocLite.R -P ~/Rmirror/Bioc

where **-N** is to overwrite original file if the size or timestamp change and **-P** in wget means an output directory, not a file name.

Optionally, we can add the following in order to see the Bioconductor front page.

    rsync -zrtlv  --delete bioconductor.org::2.11/BiocViews.html ~/Rmirror/Bioc/packages/2.11/
    rsync -zrtlv  --delete bioconductor.org::2.11/index.html ~/Rmirror/Bioc/packages/2.11/

The software part (aka bioc directory) installation:

    cd
    mkdir -p ~/Rmirror/Bioc/packages/2.11/bioc/bin/windows
    mkdir -p ~/Rmirror/Bioc/packages/2.11/bioc/src
    rsync -zrtlv  --delete bioconductor.org::2.11/bioc/bin/windows/ ~/Rmirror/Bioc/packages/2.11/bioc/bin/windows
    # Either rsync whole src directory or just essential files
    # rsync -zrtlv  --delete bioconductor.org::2.11/bioc/src/ ~/Rmirror/Bioc/packages/2.11/bioc/src
    rsync -zrtlv  --delete bioconductor.org::2.11/bioc/src/contrib/PACKAGES ~/Rmirror/Bioc/packages/2.11/bioc/src/contrib/
    rsync -zrtlv  --delete bioconductor.org::2.11/bioc/src/contrib/PACKAGES.gz ~/Rmirror/Bioc/packages/2.11/bioc/src/contrib/
    # Optionally the html part
    mkdir -p ~/Rmirror/Bioc/packages/2.11/bioc/html
    rsync -zrtlv  --delete bioconductor.org::2.11/bioc/html/ ~/Rmirror/Bioc/packages/2.11/bioc/html
    mkdir -p ~/Rmirror/Bioc/packages/2.11/bioc/vignettes
    rsync -zrtlv  --delete bioconductor.org::2.11/bioc/vignettes/ ~/Rmirror/Bioc/packages/2.11/bioc/vignettes
    mkdir -p ~/Rmirror/Bioc/packages/2.11/bioc/news
    rsync -zrtlv  --delete bioconductor.org::2.11/bioc/news/ ~/Rmirror/Bioc/packages/2.11/bioc/news
    mkdir -p ~/Rmirror/Bioc/packages/2.11/bioc/licenses
    rsync -zrtlv  --delete bioconductor.org::2.11/bioc/licenses/ ~/Rmirror/Bioc/packages/2.11/bioc/licenses
    mkdir -p ~/Rmirror/Bioc/packages/2.11/bioc/manuals
    rsync -zrtlv  --delete bioconductor.org::2.11/bioc/manuals/ ~/Rmirror/Bioc/packages/2.11/bioc/manuals
    mkdir -p ~/Rmirror/Bioc/packages/2.11/bioc/readmes
    rsync -zrtlv  --delete bioconductor.org::2.11/bioc/readmes/ ~/Rmirror/Bioc/packages/2.11/bioc/readmes

and annotation (aka data directory) part:

    mkdir -p ~/Rmirror/Bioc/packages/2.11/data/annotation/bin/windows
    mkdir -p ~/Rmirror/Bioc/packages/2.11/data/annotation/src/contrib
    # one line for each of the following
    rsync -zrtlv --delete bioconductor.org::2.11/data/annotation/bin/windows/ ~/Rmirror/Bioc/packages/2.11/data/annotation/bin/windows
    rsync -zrtlv --delete bioconductor.org::2.11/data/annotation/src/contrib/PACKAGES ~/Rmirror/Bioc/packages/2.11/data/annotation/src/contrib/
    rsync -zrtlv --delete bioconductor.org::2.11/data/annotation/src/contrib/PACKAGES.gz ~/Rmirror/Bioc/packages/2.11/data/annotation/src/contrib/

and experiment directory:

    mkdir -p ~/Rmirror/Bioc/packages/2.11/data/experiment/bin/windows/contrib/2.15
    mkdir -p ~/Rmirror/Bioc/packages/2.11/data/experiment/src/contrib
    # one line for each of the following
    # Note that we are cheating by only downloading PACKAGES and PACKAGES.gz files
    rsync -zrtlv --delete bioconductor.org::2.11/data/experiment/bin/windows/contrib/2.15/PACKAGES ~/Rmirror/Bioc/packages/2.11/data/experiment/bin/windows/contrib/2.15/
    rsync -zrtlv --delete bioconductor.org::2.11/data/experiment/bin/windows/contrib/2.15/PACKAGES.gz ~/Rmirror/Bioc/packages/2.11/data/experiment/bin/windows/contrib/2.15/
    rsync -zrtlv --delete bioconductor.org::2.11/data/experiment/src/contrib/PACKAGES ~/Rmirror/Bioc/packages/2.11/data/experiment/src/contrib/
    rsync -zrtlv --delete bioconductor.org::2.11/data/experiment/src/contrib/PACKAGES.gz ~/Rmirror/Bioc/packages/2.11/data/experiment/src/contrib/

and extra directory:

    mkdir -p ~/Rmirror/Bioc/packages/2.11/extra/bin/windows/contrib/2.15
    mkdir -p ~/Rmirror/Bioc/packages/2.11/extra/src/contrib
    # one line for each of the following
    # Note that we are cheating by only downloading PACKAGES and PACKAGES.gz files
    rsync -zrtlv --delete bioconductor.org::2.11/extra/bin/windows/contrib/2.15/PACKAGES ~/Rmirror/Bioc/packages/2.11/extra/bin/windows/contrib/2.15/
    rsync -zrtlv --delete bioconductor.org::2.11/extra/bin/windows/contrib/2.15/PACKAGES.gz ~/Rmirror/Bioc/packages/2.11/extra/bin/windows/contrib/2.15/
    rsync -zrtlv --delete bioconductor.org::2.11/extra/src/contrib/PACKAGES ~/Rmirror/Bioc/packages/2.11/extra/src/contrib/
    rsync -zrtlv --delete bioconductor.org::2.11/extra/src/contrib/PACKAGES.gz ~/Rmirror/Bioc/packages/2.11/extra/src/contrib/

### To test local repository

#### Create soft links in Apache server

    su
    ln -s /home/brb/Rmirror/CRAN /var/www/html/CRAN
    ln -s /home/brb/Rmirror/Bioc /var/www/html/Bioc
    ls -l /var/www/html

The soft link mode should be 777.

#### To test CRAN

Replace the host name arraytools.no-ip.org by IP address 10.133.2.111 if necessary.

    r &lt;- getOption("repos"); r["CRAN"] &lt;- "http://arraytools.no-ip.org/CRAN"
    options(repos=r)
    install.packages("glmnet")

We can test if the backup server is working or not by installing a package which was removed from the CRAN. For example, 'ForImp' was removed from CRAN in 11/8/2012, but I still a local copy built on R 2.15.2 (run rsync on 11/6/2012).

    r &lt;- getOption("repos"); r["CRAN"] &lt;- "http://cran.r-project.org"
    r &lt;- c(r, BRB='http://arraytools.no-ip.org/CRAN')
    #                        CRAN                            CRANextra                                  BRB
    # "http://cran.r-project.org" "http://www.stats.ox.ac.uk/pub/RWin"   "http://arraytools.no-ip.org/CRAN"
    options(repos=r)
    install.packages('ForImp')

Note by default, CRAN mirror is selected interactively.

    &gt; getOption("repos")
                                    CRAN                            CRANextra
                                "@CRAN@" "http://www.stats.ox.ac.uk/pub/RWin"

#### To test Bioconductor

    # CRAN part:
    r &lt;- getOption("repos"); r["CRAN"] &lt;- "http://arraytools.no-ip.org/CRAN"
    options(repos=r)
    # Bioconductor part:
    options("BioC_mirror" = "http://arraytools.no-ip.org/Bioc")
    source("http://bioconductor.org/biocLite.R")
    # This source biocLite.R line can be placed either before or after the previous 2 lines
    biocLite("aCGH")

If there is a connection problem, check folder attributes.

    chmod -R 755 ~/CRAN/bin

* Note that if a binary package was created for R 2.15.1, then it can be installed under R 2.15.1 but not R 2.15.2. The R console will show package xxx is not available (for R version 2.15.2).
* For binary installs, the function also checks for the availability of a source package on the same repository, and reports if the source package has a later version, or is available but no binary version is.

So for example, if the mirror does not have contents under src directory, we need to run the following line in order to successfully run _install.packages()_ function.

    options(install.packages.check.source = "no")

* If we only mirror the essential directories, we can run biocLite() successfully. However, the R console will give some warning

    &gt; biocLite("aCGH")
    BioC_mirror: http://arraytools.no-ip.org/Bioc
    Using Bioconductor version 2.11 (BiocInstaller 1.8.3), R version 2.15.
    Installing package(s) 'aCGH'
    Warning: unable to access index for repository http://arraytools.no-ip.org/Bioc/packages/2.11/data/experiment/src/contrib
    Warning: unable to access index for repository http://arraytools.no-ip.org/Bioc/packages/2.11/extra/src/contrib
    Warning: unable to access index for repository http://arraytools.no-ip.org/Bioc/packages/2.11/data/experiment/bin/windows/contrib/2.15
    Warning: unable to access index for repository http://arraytools.no-ip.org/Bioc/packages/2.11/extra/bin/windows/contrib/2.15
    trying URL 'http://arraytools.no-ip.org/Bioc/packages/2.11/bioc/bin/windows/contrib/2.15/aCGH_1.36.0.zip'
    Content type 'application/zip' length 2431158 bytes (2.3 Mb)
    opened URL
    downloaded 2.3 Mb

    package 'aCGH' successfully unpacked and MD5 sums checked

    The downloaded binary packages are in
            C:UserslimingcAppDataLocalTempRtmp8IGGyGdownloaded_packages
    Warning: unable to access index for repository http://arraytools.no-ip.org/Bioc/packages/2.11/data/experiment/bin/windows/contrib/2.15
    Warning: unable to access index for repository http://arraytools.no-ip.org/Bioc/packages/2.11/extra/bin/windows/contrib/2.15
    &gt; library()

### CRAN repository directory structure

The information below is specific to R 2.15.2. There are linux and macosx subdirecotries whenever there are windows subdirectory.

    bin/winows/contrib/2.15
    src/contrib
       /contrib/2.15.2
       /contrib/Archive
    web/checks
       /dcmeta
       /packages
       /views

A clickable map [[1]][74]

### Bioconductor package download statistics

<http: bioconductor.org="" packages="" stats="">

### Bioconductor repository directory structure

The information below is specific to Bioc 2.11 (R 2.15). There are linux and macosx subdirecotries whenever there are windows subdirectory.

    bioc/bin/windows/contrib/2.15
        /html
        /install
        /license
        /manuals
        /news
        /src
        /vignettes
    data/annotation/bin/windows/contrib/2.15
                   /html
                   /licenses
                   /manuals
                   /src
                   /vignettes
         /experiment/bin/windows/contrib/2.15
                    /html
                    /manuals
                    /src/contrib
                    /vignettes
    extra/bin/windows/contrib
         /html
         /src
         /vignettes

### List all R packages from CRAN/Bioconductor

<s> Check my daily result based on R 2.15 and Bioc 2.11 in [[2]][75] </s>

1. [CRAN][76]
2. [Bioc software][77]
3. [Bioc annotation][78]
4. [Bioc experiment][79]

See [METACRAN][80] for packages hosted on CRAN. The '<https: github.com="" metacran="" packages'=""> file contains the latest update.

## Parallel Computing

1. [Example code][81] for the book Parallel R by McCallum and Weston.
2. [An introduction to distributed memory parallelism in R and C][82]
3. [Processing: When does it worth?][83]

### Windows Security Warning

It seems it is safe to choose 'Cancel' when Windows Firewall tried to block R program when we use **makeCluster()** to create a socket cluster.

    library(parallel)
    cl &lt;- makeCluster(2)
    clusterApply(cl, 1:2, get("+"), 3)
    stopCluster(cl)

![WindowsSecurityAlert.png][84]

If we like to see current firewall settings, just click Windows Start button, search 'Firewall' and choose 'Windows Firewall with Advanced Security'. In the 'Inbound Rules', we can see what programs (like, R for Windows GUI front-end, or Rserve) are among the rules. These rules are called 'private' in the 'Profile' column. Note that each of them may appear twice because one is 'TCP' protocol and the other one has a 'UDP' protocol.

### parallel package

Parallel package was included in R 2.14.0. It is derived from the snow and multicore packages and provides many of the same functions as those packages.

The parallel package provides several *apply functions for R users to quickly modify their code using parallel computing.

* makeCluster(makePSOCKcluster, makeForkCluster), stopCluster. Other cluster types are passed to package **snow**.
* clusterCall, clusterEvalQ, clusterSplit
* clusterApply, clusterApplyLB
* clusterExport
* clusterMap
* parLapply, parSapply, parApply, parRapply, parCapply
* parLapplyLB, parSapplyLB (load balance version)
* clusterSetRNGStream, nextRNGStream, nextRNGSubStream

Examples (See&nbsp;?[clusterApply][85])

    library(parallel)
    cl &lt;- makeCluster(2, type = "SOCK")
    clusterApply(cl, 1:2, function(x) x*3)    # OR clusterApply(cl, 1:2, get("*"), 3)
    # [[1]]
    # [1] 3
    #
    # [[2]]
    # [1] 6
    parSapply(cl, 1:20, get("+"), 3)
    #  [1]  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23
    stopCluster(cl)

### [snow][86] package

Supported cluster types are "SOCK", "PVM", "MPI", and "NWS".

### [multicore][87] package

This package is removed from CRAN.

Consider using package 'parallel' instead.

### [foreach][88] package

This package depends on one of the following

* doParallel - Foreach parallel adaptor for the parallel package
* doSNOW - Foreach parallel adaptor for the snow package
* doMC - Foreach parallel adaptor for the multicore package
* doMPI - Foreach parallel adaptor for the Rmpi package
* doRedis - Foreach parallel adapter for the rredis package

as a backend.

    library(foreach)
    library(doParallel)

    m &lt;- matrix(rnorm(9), 3, 3)

    cl &lt;- makeCluster(2, type = "SOCK")
    registerDoParallel(cl)
    foreach(i=1:nrow(m), .combine=rbind)&nbsp;%dopar%
      (m[i,] / mean(m[i,]))

    stopCluster(cl)

### snowfall package

<http: www.imbi.uni-freiburg.de="" parallel="" docs="" reisensburg2009_tutparallelcomputing_knaus_porzelius.pdf="">

### [Rmpi][89] package

Some examples/tutorials

## Cloud Computing

### Install R on Amazon EC2

<http: randyzwitch.com="" r-amazon-ec2="">

### Bioconductor on Amazon EC2

<http: www.bioconductor.org="" help="" bioconductor-cloud-ami="">

## Big Data Analysis

<http: blog.comsysto.com="" 2013="" 02="" 14="" my-favorite-community-links="">

## Useful R packages

### RInside

#### Ubuntu

With RInside, R can be embedded in a graphical application. For example, $HOME/R/x86_64-pc-linux-gnu-library/3.0/RInside/examples/qt directory includes source code of a Qt application to show a kernel density plot with various options like kernel functions, bandwidth and an R command text box to generate the random data. See my demo on [Youtube][90]. I have tested this **qtdensity** example successfully using Qt 4.8.5.

1. Follow the instruction cairoDevice to install required libraries for cairoDevice package and then cairoDevice itself.
2. Install [Qt][91]. Check 'qmake' command becomes available by typing 'whereis qmake' or 'which qmake' in terminal.
3. Open Qt Creator from Ubuntu start menu/Launcher. Open the project file $HOME/R/x86_64-pc-linux-gnu-library/3.0/RInside/examples/qt/qtdensity.pro in Qt Creator.
4. Under Qt Creator, hit 'Ctrl + R' or the big green triangle button on the lower-left corner to build/run the project. If everything works well, you shall see the _interactive_ program qtdensity appears on your desktop.

![Qtdensity.png][92].

With RInside + [Wt web toolkit][93] installed, we can also create a web application. To demonstrate the example in _examples/wt_ directory, we can do

    cd ~/R/x86_64-pc-linux-gnu-library/3.0/RInside/examples/wt
    make
    sudo ./wtdensity --docroot . --http-address localhost --http-port 8080

Then we can go to the browser's address bar and type _<http: localhost:8080="">_ to see how it works (a screenshot is in [here][94]).

#### Windows 7

To make RInside works on Windows OS, try the following

1. Make sure R is installed under **C:** instead of **C:Program Files** if we don't want to get an error like _g++.exe: error: Files/R/R-3.0.1/library/RInside/include: No such file or directory_.
2. Install RTools
3. Instal RInside package from source (the binary version will give an [error ][95])
4. Create a DOS batch file containing necessary paths in PATH environment variable

    @echo off
    set PATH=C:Rtoolsbin;c:Rtoolsgcc-4.6.3bin;%PATH%
    set PATH=C:RR-3.0.1bini386;%PATH%
    set PKG_LIBS=`Rscript -e "Rcpp:::LdFlags()"`
    set PKG_CPPFLAGS=`Rscript -e "Rcpp:::CxxFlags()"`
    set R_HOME=C:RR-3.0.1
    echo Setting environment for using R
    cmd

In the Windows command prompt, run

    cd C:RR-3.0.1libraryRInsideexamplesstandard
    make -f Makefile.win

Now we can test by running any of executable files that **make** generates. For example, _rinside_sample0_.

    rinside_sample0

As for the Qt application qdensity program, we need to make sure the same version of MinGW was used in building RInside/Rcpp and Qt. See some discussions in

So the Qt and Wt web tool applications on Windows may or may not be possible.

### GUI

#### Qt and R

### tkrplot

On Ubuntu, we need to install tk packages, such as by

    sudo apt-get install tk-dev

### Hadoop (eg ~100 terabytes)

See also [HighPerformanceComputing][96]

#### [RHadoop][97]

#### Snowdoop: an alternative to MapReduce algorithm

### [XML][98]

On Ubuntu, we need to install libxml2-dev before we can install XML package.

    sudo apt-get update
    sudo apt-get install libxml2-dev

On CentOS,

    yum -y install libxml2 libxml2-devel

### RCurl

On Ubuntu, we need to install one package

    sudo apt-get install libcurl4-openssl-dev

#### [httr][99]

httr depends on RCurl package.

#### [curl][100]

curl is independent of RCurl package.

### DirichletMultinomial

On Ubuntu, we do

    sudo apt-get install libgsl0-dev

### Create GUI

#### [gWidgets][101]

### [GenOrd][102]: Generate ordinal and discrete variables with given correlation matrix and marginal distributions

[here][103]

### [rjson][104]

<http: heuristically.wordpress.com="" 2013="" 05="" 20="" geolocate-ip-addresses-in-r="">

### [RJSONIO][105]

#### Plot IP on google map

The following example is modified from the first of above list.

    require(RJSONIO) # fromJSON
    require(RCurl)   # getURL

    temp = getURL("https://gist.github.com/arraytools/6743826/raw/23c8b0bc4b8f0d1bfe1c2fad985ca2e091aeb916/ip.txt",
                               ssl.verifypeer = FALSE)
    ip &lt;- read.table(textConnection(temp), as.is=TRUE)
    names(ip) &lt;- "IP"
    nr = nrow(ip)

    Lon &lt;- as.numeric(rep(NA, nr))
    Lat &lt;- Lon
    Coords &lt;- data.frame(Lon, Lat)

    ip2coordinates &lt;- function(ip) {
      api &lt;- "http://freegeoip.net/json/"
      get.ips &lt;- getURL(paste(api, URLencode(ip), sep=""))
      # result &lt;- ldply(fromJSON(get.ips), data.frame)
      result &lt;- data.frame(fromJSON(get.ips))
      names(result)[1] &lt;- "ip.address"
      return(result)
    }

    for (i in 1:nr){
      cat(i, "n")
      try(
      Coords[i, 1:2] &lt;- ip2coordinates(ip$IP[i])[c("longitude", "latitude")]
      )
    }

    # append to log-file:
    logfile &lt;- data.frame(ip, Lat = Coords$Lat, Long = Coords$Lon,
                                           LatLong = paste(round(Coords$Lat, 1), round(Coords$Lon, 1), sep = ":"))
    log_gmap &lt;- logfile[!is.na(logfile$Lat), ]

    require(googleVis) # gvisMap
    gmap &lt;- gvisMap(log_gmap, "LatLong",
                    options = list(showTip = TRUE, enableScrollWheel = TRUE,
                                   mapType = 'hybrid', useMapTypeControl = TRUE,
                                   width = 1024, height = 800))
    plot(gmap)

![GoogleVis.png][106]

The plot.gvis() method in googleVis packages also teaches the startDynamicHelp() function in the tools package, which was used to launch a http server. See [Jeffrey Horner's note about deploying Rook App][107].

### Map

#### leaflet

rstudio.github.io/leaflet/#installation-and-use

#### choroplethr

### [googleVis][108]

See an example from [RJSONIO][109] above.

### [Rcpp][110]

[Discussion archive][111]

#### Use Rcpp in RStudio

RStudio makes it easy to use Rcpp package.

Open RStudio, click New File -&gt; C++ File. It will create a C++ template on the RStudio editor

    #include <rcpp.h>
    using namespace Rcpp;

    // Below is a simple example of exporting a C++ function to R. You can
    // source this function into an R session using the Rcpp::sourceCpp
    // function (or via the Source button on the editor toolbar)

    // For more on using Rcpp click the Help button on the editor toolbar

    // [[Rcpp::export]]
    int timesTwo(int x) {
       return x * 2;
    }

Now in R console, type

    library(Rcpp)
    sourceCpp("~/Downloads/timesTwo.cpp")
    timesTwo(9)
    # [1] 18

See more examples on <http: adv-r.had.co.nz="" rcpp.html="">.

If we wan to test Boost library, we can try it in RStudio. Consider the following example in [stackoverflow.com][112].

    // [[Rcpp::depends(BH)]]
    #include <rcpp.h>
    #include <boost foreach.hpp="">
    #include <boost math="" special_functions="" gamma.hpp="">

    #define foreach BOOST_FOREACH

    using namespace boost::math;

    //[[Rcpp::export]]
    Rcpp::NumericVector boost_gamma( Rcpp::NumericVector x ) {
      foreach( double&amp; elem, x ) {
        elem = boost::math::tgamma(elem);
      };

      return x;
    }

Then the R console

    boost_gamma(0:10 + 1)
    #  [1]       1       1       2       6      24     120     720    5040   40320
    # [10]  362880 3628800

    identical( boost_gamma(0:10 + 1), factorial(0:10) )
    # [1] TRUE

#### Example 1. convolution example

First, Rcpp package should be installed (I am working on Linux system). Next we try one example shipped in Rcpp package.

PS. If R was not available in global environment (such as built by ourselves), we need to modify 'Makefile' file by replacing 'R' command with its complete path (4 places).

    cd ~/R/x86_64-pc-linux-gnu-library/3.0/Rcpp/examples/ConvolveBenchmarks/
    make
    R

Then type the following in an R session to see how it works. Note that we don't need to issue **library(Rcpp)** in R.

    dyn.load("convolve3_cpp.so")
    x &lt;- .Call("convolve3cpp", 1:3, 4:6)
    x # 4 13 28 27 18

If we have our own cpp file, we need to use the following way to create dynamic loaded library file. Note that the character ([grave accent][113]) ` is not (single quote)'. If you mistakenly use ', it won't work.

    export PKG_CXXFLAGS=`Rscript -e "Rcpp:::CxxFlags()"`
    export PKG_LIBS=`Rscript -e "Rcpp:::LdFlags()"`
    R CMD SHILB xxxx.cpp

#### Example 2. Use together with inline package

    library(inline)
    src &lt;-'
     Rcpp::NumericVector xa(a);
     Rcpp::NumericVector xb(b);
     int n_xa = xa.size(), n_xb = xb.size();

     Rcpp::NumericVector xab(n_xa + n_xb - 1);
     for (int i = 0; i &lt; n_xa; i++)
     for (int j = 0; j &lt; n_xb; j++)
     xab[i + j] += xa[i] * xb[j];
     return xab;
    '
    fun &lt;- cxxfunction(signature(a = "numeric", b = "numeric"),
     src, plugin = "Rcpp")
    fun(1:3, 1:4)
    # [1]  1  4 10 16 17 12

#### Example 3. Calling an R function

#### [RcppParallel][114]

### [caret][115]

### Read/Write Excel files package

* [xlsx][116]: depends on Java
* [openxlsx][117]: not depend on Java. Depend on zip application. On Windows, it seems to be OK without installing Rtools. But it can not read xls file; it works on xlsx file.
* [readxl][118]: it does not depend on anything although it can only read but not write Excel files. Tested it on Ubuntu machine with R 3.1.3 using <brca.xls> file. Usage:

    read_excel(path, sheet = 1, col_names = TRUE, col_types = NULL, na = "", skip = 0)

For the Chromosome column, integer values becomes strings (but converted to double, so 5 becomes 5.000000) or NA (empty on sheets).

    &gt; head(read_excel("~/Downloads/BRCA.xls", 4)[ , -9], 3)
      UniqueID (Double-click) CloneID UGCluster
    1                   HK1A1   21652 Hs.445981
    2                   HK1A2   22012 Hs.119177
    3                   HK1A4   22293 Hs.501376
                                                        Name Symbol EntrezID
    1 Catenin (cadherin-associated protein), alpha 1, 102kDa CTNNA1     1495
    2                              ADP-ribosylation factor 3   ARF3      377
    3                          Uroporphyrinogen III synthase   UROS     7390
      Chromosome      Cytoband ChimericClusterIDs Filter
    1   5.000000        5q31.2               <na>      1
    2  12.000000         12q13               <na>      1
    3       <na> 10q25.2-q26.3               <na>      1

The hidden worksheets become visible (Not sure what are those first rows mean in the output).

    &gt; excel_sheets("~/Downloads/BRCA.xls")
    DEFINEDNAME: 21 00 00 01 0b 00 00 00 02 00 00 00 00 00 00 0d 3b 01 00 00 00 9a 0c 00 00 1a 00
    DEFINEDNAME: 21 00 00 01 0b 00 00 00 04 00 00 00 00 00 00 0d 3b 03 00 00 00 9b 0c 00 00 0a 00
    DEFINEDNAME: 21 00 00 01 0b 00 00 00 03 00 00 00 00 00 00 0d 3b 02 00 00 00 9a 0c 00 00 06 00
    [1] "Experiment descriptors" "Filtered log ratio"     "Gene identifiers"
    [4] "Gene annotations"       "CollateInfo"            "GeneSubsets"
    [7] "GeneSubsetsTemp"

The Chinese character works too.

    &gt; read_excel("~/Downloads/testChinese.xlsx", 1)
       中文 B C
    1     a b c
    2     1 2 3

### [ggplot2][119]

Some examples:

Introduction

#### [ggthemr][120]: Themes for ggplot2

### Data Manipulation

A [summary of 5 most useful data manipulation functions][121] published in rpubs.com.

* subset() for making subsets of data (natch)
* merge() for combining data sets in a smart and easy way
* melt()-reshape2 package for converting from wide to long data formats
* dcast()-reshape2 package for converting from long to wide data formats, and for making summary tables
* ddply()-plyr package for doing split-apply-combine operations, which covers a huge swath of the most tricky data operations

#### stringr and plyr packages

<http: martinsbioblogg.wordpress.com="" 2013="" 03="" 24="" using-r-reading-tables-that-need-a-little-cleaning="">

A data.frame is pretty much a list of vectors, so we use plyr to apply over the list and stringr to search and replace in the vectors.

#### reshape2

Use **acast()** function in reshape2 package. It will convert data.frame used for analysis to a table-like data.frame good for display.

#### [magrittr][122]

Instead of nested statements, it is using pipe operator **%&gt;%**. So the code is easier to read. Impressive!

### [jpeg][123]

If we want to create the image on this wiki left hand side panel, we can use jpeg package to read an existing plot and then edit and save it.

### cairoDevice

PS. Not sure the advantage of functions in this package compared to R's functions (eg. Cairo_svg() vs svg()) or even [Cairo][124] package.

For ubuntu OS, we need to install 2 libraries and 1 R package **RGtk2**.

    sudo apt-get install libgtk2.0-dev libcairo2-dev

On Windows OS, we may got the error: **unable to load shared object 'C:/Program Files/R/R-3.0.2/library/cairoDevice/libs/x64/cairoDevice.dll' **. We need to follow the instruction in [here][125].

### [iterators][126]

Iterator is useful over for-loop if the data is already a **collection**. It can be used to iterate over a vector, data frame, matrix, file

Iterator can be combined to use with foreach package <http: www.exegetic.biz="" blog="" 2013="" 11="" iterators-in-r=""> has more elaboration.

### [colortools][127]

Tools that allow users generate color schemes and palettes

### [rex][128]

Friendly Regular Expressions

### [formatR][129]

**The best strategy to avoid failure is to put comments in complete lines or after complete R expressions.**

See also [this discussion][130] on stackoverflow talks about R code reformatting.

    library(formatR)
    tidy_source("Input.R", file = "output.R", width.cutoff=70)
    tidy_source("clipboard")
    # default width is getOption("width") which is 127 in my case.

Some issues

* Comments appearing at the beginning of a line within a long complete statement. This will break tidy_source().

    cat("abcd",
        # This is my comment
        "defg")

will result in

    &gt; tidy_source("clipboard")
    Error in base::parse(text = code, srcfile = NULL)&nbsp;:
      3:1: unexpected string constant
    2: invisible(".BeGiN_TiDy_IdEnTiFiEr_HaHaHa# This is my comment.HaHaHa_EnD_TiDy_IdEnTiFiEr")
    3: "defg"
       ^

* Comments appearing at the end of a line within a long complete statement _won't break_ tidy_source() but tidy_source() cannot re-locate/tidy the comma sign.

```
    cat("abcd"
        ,"defg"   # This is my comment
      ,"ghij")
```

will become

```
    cat("abcd", "defg"  # This is my comment
    , "ghij")
```

Still bad!!

* Comments appearing at the end of a line within a long complete statement _breaks_ tidy_source() function. For example,

```
    cat("<p></p>",
    	"<hr size="5" width="100%" noshade="">",
    	ifelse(codeSurv == 0,"<h3><a name="Genes"><b><u>Genes which are differentially expressed among classes:</u></b></a></h3>", #4/9/09
    	                     "<h3><a name="Genes"><b><u>Genes significantly associated with survival:</u></b></a></h3>"),
    	file=ExternalFileName, sep="n", append=T)
```

will result in

```
    &gt; tidy_source("clipboard", width.cutoff=70)
    Error in base::parse(text = code, srcfile = NULL)&nbsp;:
      3:129: unexpected SPECIAL
    2: "<hr size="5" width="100%" noshade="">" ,
    3: ifelse ( codeSurv == 0 , "<h3><a name="Genes"><b><u>Genes which are differentially expressed among classes:</u></b></a></h3>" ,&nbsp;%InLiNe_IdEnTiFiEr%
```

* _width.cutoff_ parameter is not always working. For example, there is no any change for the following snippet though I hope it will move the cat() to the next line.

```
    if (codePF &amp;&nbsp;!GlobalTest &amp;&nbsp;!DoExactPermTest) cat(paste("Multivariate Permutations test was computed based on",
        NumPermutations, "random permutations"), "<br>", " ", file = ExternalFileName,
        sep = "n", append = T)
```
* It merges lines though I don't always want to do that. For example

```
    cat("abcd"
        ,"defg"
      ,"ghij")
```

will become

```
    cat("abcd", "defg", "ghij")
```

### Download papers

#### [biorxivr][131]

Search and Download Papers from the bioRxiv Preprint Server

#### [aRxiv][132]

Interface to the arXiv API

## Different ways of using R

### R call C/C++

Mainly talks about .C() and .Call().

### R call Fortran 90

### Embedding R

#### An very simple example (do not return from shell) from Writing R Extensions manual

The command-line R front-end, R_HOME/bin/exec/R, is one such example. Its source code is in file <src main="" rmain.c="">.

This example can be run by

    R_HOME/bin/R CMD R_HOME/bin/exec/R

Note:

1. **R_HOME/bin/exec/R** is the R binary. However, it couldn't be launched directly unless R_HOME and LD_LIBRARY_PATH are set up. Again, this is explained in Writing R Extension manual.
2. **R_HOME/bin/R** is a shell-script front-end where users can invoke it. It sets up the environment for the executable. It can be copied to _/usr/local/bin/R_. When we run _R_HOME/bin/R_, it actually runs _R_HOME/bin/R CMD R_HOME/bin/exec/R_ (see line 259 of _R_HOME/bin/R_ as in R 3.0.2) so we know the important role of _R_HOME/bin/exec/R_.

More examples of embedding can be found in _tests/Embedding_ directory. Read <index.html> for more information about these test examples.

#### An example from Bioconductor workshop

Example: Create <embed.c> file

    #include <rembedded.h>
    #include <rdefines.h>

    static void doSplinesExample();
    int
    main(int argc, char *argv[])
    {
        Rf_initEmbeddedR(argc, argv);
        doSplinesExample();
        Rf_endEmbeddedR(0);
        return 0;
    }
    static void
    doSplinesExample()
    {
        SEXP e, result;
        int errorOccurred;

        // create and evaluate 'library(splines)'
        PROTECT(e = lang2(install("library"), mkString("splines")));
        R_tryEval(e, R_GlobalEnv, &amp;errorOccurred);
        if (errorOccurred) {
            // handle error
        }
        UNPROTECT(1);

        // 'options(FALSE)' ...
        PROTECT(e = lang2(install("options"), ScalarLogical(0)));
        // ... modified to 'options(example.ask=FALSE)' (this is obscure)
        SET_TAG(CDR(e), install("example.ask"));
        R_tryEval(e, R_GlobalEnv, NULL);
        UNPROTECT(1);

        // 'example("ns")'
        PROTECT(e = lang2(install("example"), mkString("ns")));
        R_tryEval(e, R_GlobalEnv, &amp;errorOccurred);
        UNPROTECT(1);
    }

Then build the executable. Note that I don't need to create R_HOME variable.

    cd
    tar xzvf
    cd R-3.0.1
    ./configure --enable-R-shlib
    make
    cd tests/Embedding
    make
    ~/R-3.0.1/bin/R CMD ./Rtest

    nano embed.c
    # Using a single line will give an error and cannot not show the real problem.
    # ../../bin/R CMD gcc -I../../include -L../../lib -lR embed.c
    # A better way is to run compile and link separately
    gcc -I../../include -c embed.c
    gcc -o embed embed.o -L../../lib -lR -lRblas
    ../../bin/R CMD ./embed

Note that if we want to call the executable file ./embed directly, we shall set up R environment by specifying **R_HOME** variable and including the directories used in linking R in **LD_LIBRARY_PATH**. This is based on the inform provided by [Writing R Extensions][133].

    export R_HOME=/home/brb/Downloads/R-3.0.2
    export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/home/brb/Downloads/R-3.0.2/lib
    ./embed # No need to include R CMD in front.

Question: Create a data frame in C? Answer: [Use data.frame() via an eval() call from C][134]. Or see the code is stats/src/model.c, as part of model.frame.default. Or using Rcpp as [here][135].

Reference <http: bioconductor.org="" help="" course-materials="" 2012="" seattle-oct-2012="" advancedr.pdf="">

#### Create a Simple Socket Server in R

This example is coming from this [paper][136].

Create an R function

    simpleServer &lt;- function(port=6543)
    {
      sock &lt;- socketConnection ( port=port , server=TRUE)
      on.exit(close( sock ))
      cat("nWelcome to R!nR&gt;" ,file=sock )
      while(( line &lt;- readLines ( sock , n=1))&nbsp;!= "quit")
      {
        cat(paste("socket &gt;" , line , "n"))
        out&lt;- capture.output (try(eval(parse(text=line ))))
        writeLines ( out , con=sock )
        cat("nR&gt; " ,file =sock )
      }
    }

Then run simpleServer(). Open another terminal and try to communicate with the server

    $ telnet localhost 6543
    Trying 127.0.0.1...
    Connected to localhost.
    Escape character is '^]'.

    Welcome to R!
    R&gt; summary(iris[, 3:5])
      Petal.Length    Petal.Width          Species
     Min.  &nbsp;:1.000   Min.  &nbsp;:0.100   setosa   &nbsp;:50
     1st Qu.:1.600   1st Qu.:0.300   versicolor:50
     Median&nbsp;:4.350   Median&nbsp;:1.300   virginica&nbsp;:50
     Mean  &nbsp;:3.758   Mean  &nbsp;:1.199
     3rd Qu.:5.100   3rd Qu.:1.800
     Max.  &nbsp;:6.900   Max.  &nbsp;:2.500

    R&gt; quit
    Connection closed by foreign host.

#### [Rserve][137]

Note the way of launching Rserve is like the way we launch C program when R was embedded in C. See [Call R from C/C++][138] or [Example from Bioconductor workshop][139].

See my [Rserve][12] page.

#### (Commercial) [StatconnDcom][140]

#### [R.NET][141]

#### RJava

#### RCaller

#### RApache

#### littler

<http: dirk.eddelbuettel.com="" code="" littler.html="">

[Difference between Rscript and littler][142]

#### RInside: Embed R in C++

See [RInside][143]

(_From RInside documentation_) The RInside package makes it easier to embed R in your C++ applications. There is no code you would execute directly from the R environment. Rather, you write C++ programs that embed R which is illustrated by some the included examples.

The included examples are armadillo, eigen, mpi, qt, standard, threads and wt.

To run 'make' when we don't have a global R, we should modify the file <makefile>. Also if we just want to create one executable file, we can do, for example, 'make rinside_sample1'.

To run any executable program, we need to specify **LD_LIBRARY_PATH** variable, something like

    export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/home/brb/Downloads/R-3.0.2/lib

The real build process looks like (check <makefile> for completeness)

    g++ -I/home/brb/Downloads/R-3.0.2/include
        -I/home/brb/Downloads/R-3.0.2/library/Rcpp/include
        -I/home/brb/Downloads/R-3.0.2/library/RInside/include -g -O2 -Wall
        -I/usr/local/include
        rinside_sample0.cpp
        -L/home/brb/Downloads/R-3.0.2/lib -lR  -lRblas -lRlapack
        -L/home/brb/Downloads/R-3.0.2/library/Rcpp/lib -lRcpp
        -Wl,-rpath,/home/brb/Downloads/R-3.0.2/library/Rcpp/lib
        -L/home/brb/Downloads/R-3.0.2/library/RInside/lib -lRInside
        -Wl,-rpath,/home/brb/Downloads/R-3.0.2/library/RInside/lib
        -o rinside_sample0

Hello World example of embedding R in C++.

    #include <rinside.h>                    // for the embedded R via RInside

    int main(int argc, char *argv[]) {

        RInside R(argc, argv);              // create an embedded R instance

        R["txt"] = "Hello, world!n";	// assign a char* (string) to 'txt'

        R.parseEvalQ("cat(txt)");           // eval the init string, ignoring any returns

        exit(0);
    }

The above can be compared to the Hello world example in Qt.

    #include <qapplication.h>
    #include <qpushbutton.h>

    int main( int argc, char **argv )
    {
        QApplication app( argc, argv );

        QPushButton hello( "Hello world!", 0 );
        hello.resize( 100, 30 );

        app.setMainWidget( &amp;hello );
        hello.show();

        return app.exec();
    }

#### [RFortran][144]

RFortran is an open source project with the following aim:

_To provide an easy to use Fortran software library that enables Fortran programs to transfer data and commands to and from R._

It works only on Windows platform with Microsoft Visual Studio installed:(

### Call R from other languages

#### JRI

<http: www.rforge.net="" jri="">

#### ryp2

<http: rpy.sourceforge.net="" rpy2.html="">

### Create a standalone Rmath library

R has many math and statistical functions. We can easily use these functions in our C/C++/Fortran. The definite guide of doing this is on Chapter 9 "The standalone Rmath library" of [R-admin manual][145].

Here is my experience based on R 3.0.2 on Windows OS.

#### Create a static library <librmath.a> and a dynamic library <rmath.dll>

Suppose we have downloaded R source code and build R from its source. See [Build_R_from_its_source][146]. Then the following 2 lines will generate files <librmath.a> and <rmath.dll> under C:RR-3.0.2srcnmathstandalone directory.

    cd C:RR-3.0.2srcnmathstandalone
    make -f Makefile.win

#### Use Rmath library in our code

    set CPLUS_INCLUDE_PATH=C:RR-3.0.2srcinclude
    set LIBRARY_PATH=C:RR-3.0.2srcnmathstandalone
    # It is not LD_LIBRARY_PATH in above.

    # Created <rmathex1.cpp> from the book "Statistical Computing in C++ and R" web site
    # http://math.la.asu.edu/~eubank/CandR/ch4Code.cpp
    # It is OK to save the cpp file under any directory.

    # Force to link against the static library <librmath.a>
    g++ RmathEx1.cpp -lRmath -lm -o RmathEx1.exe
    # OR
    g++ RmathEx1.cpp -Wl,-Bstatic -lRmath -lm -o RmathEx1.exe

    # Force to link against dynamic library <rmath.dll>
    g++ RmathEx1.cpp Rmath.dll -lm -o RmathEx1Dll.exe

Test the executable program. Note that the executable program _RmathEx1.exe_ can be transferred to and run in another computer without R installed. Isn't it cool!

    c:R&gt;RmathEx1
    Enter a argument for the normal cdf:
    1
    Enter a argument for the chi-squared cdf:
    1
    Prob(Z &lt;= 1) = 0.841345
    Prob(Chi^2 &lt;= 1)= 0.682689

Below is the cpp program <rmathex1.cpp>.

    //RmathEx1.cpp
    #define MATHLIB_STANDALONE
    #include <iostream>
    #include "Rmath.h"

    using std::cout; using std::cin; using std::endl;

    int main()
    {
      double x1, x2;
      cout &lt;&lt; "Enter a argument for the normal cdf:" &lt;&lt; endl;
      cin &gt;&gt; x1;
      cout &lt;&lt; "Enter a argument for the chi-squared cdf:" &lt;&lt; endl;
      cin &gt;&gt; x2;

      cout &lt;&lt; "Prob(Z &lt;= " &lt;&lt; x1 &lt;&lt; ") = " &lt;&lt;
        pnorm(x1, 0, 1, 1, 0)  &lt;&lt; endl;
      cout &lt;&lt; "Prob(Chi^2 &lt;= " &lt;&lt; x2 &lt;&lt; ")= " &lt;&lt;
        pchisq(x2, 1, 1, 0) &lt;&lt; endl;
      return 0;
    }

### Calling R.dll directly

See Chapter 8.2.2 of [R Extensions][147]. This is related to embedding R under Windows. The file <r.dll> on Windows is like <libr.so> on Linux.

### Create HTML 5 web and slides

See here

### Create presentation file (beamer)

1. Create Rmd file first in Rstudio by File -&gt; R markdown. Select Presentation &gt; choose pdf (beamer) as output format.
2. Edit the template created by RStudio.
3. Click 'Knit pdf' button (Ctrl+Shift+k) to create/display the pdf file.

An example of Rmd is

    ---
    title: "My Example"
    author: You Know Me
    date: Dec 32, 2014
    output: beamer_presentation
    ---

    ## R Markdown

    This is an R Markdown presentation. Markdown is a simple formatting syntax for authoring HTML, PDF, and MS Word documents.
    For more details on using R Markdown see <http: rmarkdown.rstudio.com="">.

    When you click the **Knit** button a document will be generated that includes both content as well as the output of any
    embedded R code chunks within the document.

    ## Slide with Bullets

    - Bullet 1
    - Bullet 2
    - Bullet 3. Mean is $frac{1}{n} sum_{i=1}^n x_i$.
    $$
    mu = frac{1}{n} sum_{i=1}^n x_i
    $$

    ## New slide

    ![picture of BDGE](/home/brb/Pictures/BDGEFinished.png)

    ## Slide with R Code and Output

    ```{r}
    summary(cars)
    ```

    ## Slide with Plot

    ```{r, echo=FALSE}
    plot(cars)
    ```

### Create HTML report

[ReportingTools][148] (Jason Hackney) from Bioconductor.

#### [htmlTable][149] package

The htmlTable package is intended for generating tables using HTML formatting. This format is compatible with Markdown when used for HTML-output. The most basic table can easily be created by just passing a matrix or a data.frame to the htmlTable-function.

#### [htmltab][150] package

This package is NOT used to CREATE html report but EXTRACT html table.

#### [ztable][151] package

Makes zebra-striped tables (tables with alternating row colors) in LaTeX and HTML formats easily from a data.frame, matrix, lm, aov, anova, glm or coxph objects.

### Create academic report

[reports][152] package in CRAN and in [github][153] repository. The youtube video gives an overview of the package.

### Create pdf and epub files

    # Idea:
    #        knitr        pdflatex
    #   rnw -------&gt; tex ----------&gt; pdf
    library(knitr)
    knit("example.rnw") # create example.tex file

* A very simple example &lt;002-minimal.Rnw&gt; from [yihui.name][154] works fine on linux.
* <knitr-minimal.rnw>. I have no problem to create pdf file on Windows but still cannot generate pdf on Linux from tex file. Some people suggested to run **sudo apt-get install texlive-fonts-recommended** to install missing fonts. It works!

To see a real example, check out [DESeq2][155] package (inst/doc subdirectory). In addition to DESeq2, I also need to install **DESeq, BiocStyle, airway, vsn, gplots**, and **pasilla** packages from Bioconductor. Note that, it is best to use sudo/admin account to install packages.

Or starts with markdown file. Download the example &lt;001-minimal.Rmd&gt; and remove the last line of getting png file from internet.

    # Idea:
    #        knitr        pandoc
    #   rmd -------&gt; md ----------&gt; pdf

    R -e "library(knitr); knit('001-minimal.Rmd')"
    pandoc 001-minimal.md -o 001-minimal.pdf

To create an epub file (not success yet on Windows OS, missing figures on Linux OS)

    # Idea:
    #        knitr        pandoc
    #   rnw -------&gt; tex ----------&gt; markdown or epub

    library(knitr)
    knit("DESeq2.Rnw") # create DESeq2.tex
    system("pandoc  -f latex -t markdown -o DESeq2.text DESeq2.tex")

    ## Windows OS, epub cannot be built
    pandoc:
    Error:
    "source" (line 41, column 7):
    unexpected "k"
    expecting "{document}"

    ## Linux OS, epub missing figures and R codes.
    ## First install texlive base and extra packages
    ## sudo apt-get install texlive-latex-base texlive-latex-extra
    pandoc: Could not find media `figure/SchwederSpjotvoll-1', skipping...
    pandoc: Could not find media `figure/sortedP-1', skipping...
    pandoc: Could not find media `figure/figHeatmap2c-1', skipping...
    pandoc: Could not find media `figure/figHeatmap2b-1', skipping...
    pandoc: Could not find media `figure/figHeatmap2a-1', skipping...
    pandoc: Could not find media `figure/plotCountsAdv-1', skipping...
    pandoc: Could not find media `figure/plotCounts-1', skipping...
    pandoc: Could not find media `figure/MA-1', skipping...
    pandoc: Could not find media `figure/MANoPrior-1', skipping...

The problems are at least

* figures need to be generated under the same directory as the source code
* figures cannot be in the format of pdf (DESeq2 generates both pdf and png files format)
* missing R codes

### Create Word report

#### knitr + pandoc

It is better to create rmd file in RStudio. Rstudio provides a template for rmd file and it also provides a quick reference to R markdown language.

    # Idea:
    #        knitr       pandoc
    #   rmd -------&gt; md --------&gt; docx
    library(knitr)
    knit2html("example.rmd") #Create md and html files

and then

    FILE &lt;- "example"
    system(paste0("pandoc -o ", FILE, ".docx ", FILE, ".md"))

Note. For example reason, if I play around the above 2 commands for several times, the knit2html() does not work well. However, if I click 'Knit HTML' button on the RStudio, it then works again.

Another way is

    library(pander)
    name = "demo"
    knit(paste0(name, ".Rmd"), encoding = "utf-8")
    Pandoc.brew(file = paste0(name, ".md"), output = paste0(-name, "docx"), convert = "docx")

Note that once we have used knitr command to create a md file, we can use pandoc shell command to convert it to different formats:

* A pdf file: pandoc -s report.md -t latex -o report.pdf
* A html file: pandoc -s report.md -o report.html (with the -c flag html files can be added easily)
* Openoffice: pandoc report.md -o report.odt
* Word docx: pandoc report.md -o report.docx

We can also create the epub file for reading on Kobo ereader. For example, download [this file][156] and save it as example.Rmd. I need to remove the line containing the link to <http: i.imgur.com="" rvnmr.jpg=""> since it creates an error when I run pandoc (not sure if it is the pandoc version I have is too old). Now we just run these 2 lines to get the epub file. Amazing!

    knit("example.Rmd")
    pandoc("example.md", format="epub")

PS. If we don't remove the link, we will get an error message (pandoc 1.10.1 on Windows 7)

    &gt; pandoc("Rmd_to_Epub.md", format="epub")
    executing pandoc   -f markdown -t epub -o Rmd_to_Epub.epub "Rmd_to_Epub.utf8md"
    pandoc.exe: ..http://i.imgur.com/RVNmr.jpg: openBinaryFile: invalid argument (Invalid argument)
    Error in (function (input, format, ext, cfg) &nbsp;: conversion failed
    In addition: Warning message:
    running command 'pandoc   -f markdown -t epub -o Rmd_to_Epub.epub "Rmd_to_Epub.utf8md"' had status 1

#### pander

Try pandoc[1] with a minimal reproducible example, you might give a try to my "[pander][157]" package [2] too:

    library(pander)
    Pandoc.brew(system.file('examples/minimal.brew', package='pander'),
                output = tempfile(), convert = 'docx')

Where the content of the "minimal.brew" file is something you might have got used to with Sweave - although it's using "brew" syntax instead. See the examples of pander [3] for more details. Please note that pandoc should be installed first, which is pretty easy on Windows.

1. <http: johnmacfarlane.net="" pandoc="">
2. <http: rapporter.github.com="" pander="">
3. <http: rapporter.github.com="" pander="" #examples="">

#### R2wd

Use [R2wd][158] package. However, only 32-bit R is allowed and sometimes it can not produce all 'table's.

    &gt; library(R2wd)
    &gt; wdGet()
    Loading required package: rcom
    Loading required package: rscproxy
    rcom requires a current version of statconnDCOM installed.
    To install statconnDCOM type
         installstatconnDCOM()

    This will download and install the current version of statconnDCOM

    You will need a working Internet connection
    because installation needs to download a file.
    Error in if (wdapp[["Documents"]][["Count"]] == 0) wdapp[["Documents"]]$Add()&nbsp;:
      argument is of length zero

The solution is to launch 32-bit R instead of 64-bit R since statconnDCOM does not support 64-bit R.

#### Convert from pdf to word

The best rendering of advanced tables is done by converting from pdf to Word. See <http: biostat.mc.vanderbilt.edu="" wiki="" main="" sweaveconvert="">

#### rtf

Use [rtf][159] package for Rich Text Format (RTF) Output.

#### xtable

Package xtable will produce html output. If you save the file and then open it with Word, you will get serviceable results. I've had better luck copying the output from xtable and pasting it into Excel.

#### [ReporteRs][160]

Microsoft Word, Microsoft Powerpoint and HTML documents generation from R. The source code is hosted on <https: github.com="" davidgohel="" reporters="">

### R Graphs Gallery

### COM client or server

#### Client

[RDCOMClient][161] where [excel.link][162] depends on it.

#### Server

[RDCOMServer][163]

### Use R under proxy

<http: support.rstudio.org="" help="" kb="" faq="" configuring-r-to-use-an-http-proxy="">

### What is the best place to save Rconsole on Windows platform

Put it in _C:/Users/USERNAME/Documents_ folder so no matter how R was upgraded/downgraded, it always find my preference.

### Web scraping

<http: www.slideshare.net="" schamber="" web-data-from-r#btnnext="">

#### [pubmed.mineR][164]

Text mining of PubMed Abstracts (<http: www.ncbi.nlm.nih.gov="" pubmed="">). The algorithms are designed for two formats (text and XML) from PubMed.

### Launch Rstudio

If multiple versions of R was detected, Rstudio can not be launched successfully. A java-like clock will be spinning without a stop. The trick is to click Ctrl key and click the Rstudio at the same time. After done that, it will show up a selection of R to choose from.

![RStudio.jpg][165]

### List files using regular expression

    list.files(pattern = "\.txt$")

where the dot (.) is a metacharacter. It is used to refer to any character.

    list.files(pattern = "^Something")

Using **Sys.glob()"' as**

    &gt; Sys.glob("~/Downloads/*.txt")
    [1] "/home/brb/Downloads/ip.txt"       "/home/brb/Downloads/valgrind.txt"

### Hidden tool: rsync in Rtools

    c:Rtoolsbin&gt;rsync -avz "/cygdrive/c/users/limingc/Downloads/a.exe" "/cygdrive/c/users/limingc/Documents/"
    sending incremental file list
    a.exe

    sent 323142 bytes  received 31 bytes  646346.00 bytes/sec
    total size is 1198416  speedup is 3.71

    c:Rtoolsbin&gt;

And rsync works best when we need to sync folder.

    c:Rtoolsbin&gt;rsync -avz "/cygdrive/c/users/limingc/Downloads/binary" "/cygdrive/c/users/limingc/Documents/"
    sending incremental file list
    binary/
    binary/Eula.txt
    binary/cherrytree.lnk
    binary/depends64.chm
    binary/depends64.dll
    binary/depends64.exe
    binary/mtputty.exe
    binary/procexp.chm
    binary/procexp.exe
    binary/pscp.exe
    binary/putty.exe
    binary/sqlite3.exe
    binary/wget.exe

    sent 4115294 bytes  received 244 bytes  1175868.00 bytes/sec
    total size is 8036311  speedup is 1.95

    c:Rtoolsbin&gt;rm c:userslimingcDocumentsbinaryprocexp.exe
    cygwin warning:
      MS-DOS style path detected: c:userslimingcDocumentsbinaryprocexp.exe
      Preferred POSIX equivalent is: /cygdrive/c/users/limingc/Documents/binary/procexp.exe
      CYGWIN environment variable option "nodosfilewarning" turns off this warning.
      Consult the user's guide for more details about POSIX paths:
        http://cygwin.com/cygwin-ug-net/using.html#using-pathnames

    c:Rtoolsbin&gt;rsync -avz "/cygdrive/c/users/limingc/Downloads/binary" "/cygdrive/c/users/limingc/Documents/"
    sending incremental file list
    binary/
    binary/procexp.exe

    sent 1767277 bytes  received 35 bytes  3534624.00 bytes/sec
    total size is 8036311  speedup is 4.55

    c:Rtoolsbin&gt;

Unforunately, if the destination is a network drive, I could get a permission denied (13) error. See also <http: superuser.com="" questions="" 69620="" rsync-file-permissions-on-windows="">

### Install rgdal package on ubuntu

    sudo apt-get install libgdal1-dev libproj-dev
    R
    &gt; install.packages("rgdal")

### Set up Emacs on Windows

Edit the file _C:Program FilesGNU Emacs 23.2site-lispsite-start.el_ with something like

    (setq-default inferior-R-program-name
                  "c:/program files/r/r-2.15.2/bin/i386/rterm.exe")

### Database

#### [RMySQL][166]

#### [RSQLite][167]

[<http: blog.sobbayi.com="" sql="">

#### MongoDB

### Github

#### R source

#### github

<https: github.com="" languages="" r="">

#### Send local repository to Github in R by using reports package

<http: www.youtube.com="" watch?v="WdOI_-aZV0Y">

#### My collection

#### How to download

Clone ~ Download.

    git clone https://gist.github.com/4484270.git

This will create a subdirectory called '4484270' with all cloned files there.

    library(devtools)
    source_gist("4484270")

or First download the json file from

    <https: api.github.com="" users="" myuserlogin="" gists="">

and then

    library(RJSONIO)
    x &lt;- fromJSON("~/Downloads/gists.json")
    setwd("~/Downloads/")
    gist.id &lt;- lapply(x, "[[", "id")
    lapply(gist.id, function(x){
      cmd &lt;- paste0("git clone https://gist.github.com/", x, ".git")
      system(cmd)
    })

### Connect R with Arduino

### Android App

### Amazing plots

#### 3D plot

Using [persp][168] function to create the following plot.

![3dpersp.png][169]

    ### Random pattern
     # Create matrix with random values with dimension of final grid
      rand &lt;- rnorm(441, mean=0.3, sd=0.1)
      mat.rand &lt;- matrix(rand, nrow=21)

    # Create another matrix for the colors. Start by making all cells green
      fill &lt;- matrix("green3", nr = 21, nc = 21)

    # Change colors in each cell based on corresponding mat.rand value
      fcol &lt;- fill
      fcol[] &lt;- terrain.colors(40)[cut(mat.rand,
         stats::quantile(mat.rand, seq(0,1, len = 41),
         na.rm=T), include.lowest = TRUE)]

    # Create concave surface using expontential function
      x &lt;- -10:10
      y &lt;- x^2
      y &lt;- as.matrix(y)
      y1 &lt;- y
      for(i in 1:20){tmp &lt;- cbind(y,y1); y1 &lt;- tmp[,1]; y &lt;- tmp;}
      mat &lt;- tmp[1:21, 1:21]

    # Plot it up!
      persp(1:21, 1:21, t(mat)/10, theta = 90, phi = 35,col=fcol,
         scale = FALSE, axes = FALSE, box = FALSE)

    ### Organized pattern
    # Same as before
      rand &lt;- rnorm(441, mean=0.3, sd=0.1)

    # Create concave surface using expontential function
      x &lt;- -10:10
      y &lt;- x^2
      y &lt;- as.matrix(y)
      for(i in 1:20){tmp &lt;- cbind(y,y); y1 &lt;- tmp[,1]; y &lt;- tmp;}
      mat &lt;- tmp[1:21, 1:21]

    ###Organize rand by y and put into matrix form
      o &lt;- order(rand,as.vector(mat))
      o.tmp &lt;- cbind(rand[o], rev(sort(as.vector(mat))))
      mat.org &lt;- matrix(o.tmp[,1], nrow=21)
      half.1 &lt;- mat.org[,seq(1,21,2)]
      half.2 &lt;- mat.org[,rev(seq(2,20,2))]
      full &lt;- cbind(half.1, half.2)
      full &lt;- t(full)

    # Again, create color matrix and populate using rand values
    zi &lt;- full[-1, -1] + full[-1, -21] + full[-21,-1] + full[-21, -21]
    fill &lt;- matrix("green3", nr = 20, nc = 20)
    fcol &lt;- fill
    fcol[] &lt;- terrain.colors(40)[cut(zi,
            stats::quantile(zi, seq(0,1, len = 41), na.rm=T),
            include.lowest = TRUE)]

    # Plot it up!
    persp(1:21, 1:21, t(mat)/10, theta = 90, phi = 35,col=t(fcol),
         scale = FALSE, axes = FALSE, box = FALSE)

#### Christmas tree

<http: wiekvoet.blogspot.com="" 2014="" 12="" merry-christmas.html="">

    # http://blogs.sas.com/content/iml/2012/12/14/a-fractal-christmas-tree/
    # Each row is a 2x2 linear transformation
    # Christmas tree
    L &lt;-  matrix(
        c(0.03,  0,     0  ,  0.1,
            0.85,  0.00,  0.00, 0.85,
            0.8,   0.00,  0.00, 0.8,
            0.2,  -0.08,  0.15, 0.22,
            -0.2,   0.08,  0.15, 0.22,
            0.25, -0.1,   0.12, 0.25,
            -0.2,   0.1,   0.12, 0.2),
        nrow=4)
    # ... and each row is a translation vector
    B &lt;- matrix(
        c(0, 0,
            0, 1.5,
            0, 1.5,
            0, 0.85,
            0, 0.85,
            0, 0.3,
            0, 0.4),
        nrow=2)

    prob = c(0.02, 0.6,.08, 0.07, 0.07, 0.07, 0.07)

    # Iterate the discrete stochastic map
    N = 1e5 #5  #   number of iterations
    x = matrix(NA,nrow=2,ncol=N)
    x[,1] = c(0,2)   # initial point
    k &lt;- sample(1:7,N,prob,replace=TRUE) # values 1-7

    for (i in 2:N)
      x[,i] = crossprod(matrix(L[,k[i]],nrow=2),x[,i-1]) + B[,k[i]] # iterate

    # Plot the iteration history
    png('card.png')
    par(bg='darkblue',mar=rep(0,4))
    plot(x=x[1,],y=x[2,],
        col=grep('green',colors(),value=TRUE),
        axes=FALSE,
        cex=.1,
        xlab='',
        ylab='' )#,pch='.')

    bals &lt;- sample(N,20)
    points(x=x[1,bals],y=x[2,bals]-.1,
        col=c('red','blue','yellow','orange'),
        cex=2,
        pch=19
    )
    text(x=-.7,y=8,
        labels='Merry',
        adj=c(.5,.5),
        srt=45,
        vfont=c('script','plain'),
        cex=3,
        col='gold'
    )
    text(x=0.7,y=8,
        labels='Christmas',
        adj=c(.5,.5),
        srt=-45,
        vfont=c('script','plain'),
        cex=3,
        col='gold'
    )

![XMastree.png][170]

## Tricks

### Getting help

### Change default R repository

Edit global Rprofile file. On *NIX platforms, it's located in /usr/lib/R/library/base/R/Rprofile although local .Rprofile settings take precedence.

For example, I can specify the R mirror I like by creating a single line &lt;.Rprofile&gt; file under my home directory.

    options(repos = "http://cran.rstudio.com/")

### R package managment

#### Package related functions from package 'utils'

1. inst - a data frame with columns as the matrix returned by **installed.packages** plus "Status", a factor with levels c("ok", "upgrade"). Note: the manual does not mention "unavailable" case (but I do get it) in R 3.2.0?
2. avail - a data frame with columns as the matrix returned by **available.packages** plus "Status", a factor with levels c("installed", "not installed", "unavailable"). Note: I don't get the "unavailable" case in R 3.2.0?

    &gt; x &lt;- packageStatus()
    &gt; names(x)
    [1] "inst"  "avail"
    &gt; dim(x[['inst']])
    [1] 225  17
    &gt; x[['inst']][1:3, ]
                  Package                            LibPath Version Priority               Depends Imports
    acepack       acepack C:/Program Files/R/R-3.1.2/library 1.3-3.3     <na>                  <na>    <na>
    adabag         adabag C:/Program Files/R/R-3.1.2/library     4.0     <na> rpart, mlbench, caret    <na>
    affxparser affxparser C:/Program Files/R/R-3.1.2/library  1.38.0     <na>          R (&gt;= 2.6.0)    <na>
               LinkingTo                                                        Suggests Enhances
    acepack         <na>                                                            <na>     <na>
    adabag          <na>                                                            <na>     <na>
    affxparser      <na> R.oo (&gt;= 1.18.0), R.utils (&gt;= 1.32.4),nAffymetrixDataTestFiles     <na>
                          License License_is_FOSS License_restricts_use OS_type MD5sum NeedsCompilation Built
    acepack    MIT + file LICENSE            <na>                  <na>    <na>   <na>              yes 3.1.2
    adabag             GPL (&gt;= 2)            <na>                  <na>    <na>   <na>               no 3.1.2
    affxparser        LGPL (&gt;= 2)            <na>                  <na>    <na>   <na>             <na> 3.1.1
                    Status
    acepack             ok
    adabag              ok
    affxparser unavailable
    &gt; dim(x[['avail']])
    [1] 6538   18
    &gt; x[['avail']][1:3, ]
                    Package Version Priority                        Depends        Imports LinkingTo
    A3                   A3   0.9.2     <na> R (&gt;= 2.15.0), xtable, pbapply           <na>      <na>
    ABCExtremes ABCExtremes     1.0     <na>      SpatialExtremes, combinat           <na>      <na>
    ABCanalysis ABCanalysis   1.0.1     <na>                    R (&gt;= 2.10) Hmisc, plotrix      <na>
                           Suggests Enhances    License License_is_FOSS License_restricts_use OS_type Archs
    A3          randomForest, e1071     <na> GPL (&gt;= 2)            <na>                  <na>    <na>  <na>
    ABCExtremes                <na>     <na>      GPL-2            <na>                  <na>    <na>  <na>
    ABCanalysis                <na>     <na>      GPL-3            <na>                  <na>    <na>  <na>
                MD5sum NeedsCompilation File                                      Repository        Status
    A3            <na>             <na> <na> http://cran.rstudio.com/bin/windows/contrib/3.1 not installed
    ABCExtremes   <na>             <na> <na> http://cran.rstudio.com/bin/windows/contrib/3.1 not installed
    ABCanalysis   <na>             <na> <na> http://cran.rstudio.com/bin/windows/contrib/3.1 not installed

#### install.packages()

By default, install.packages() will check versions and install uninstalled packages shown in 'Depends', 'Imports', and 'LinkingTo' fields. See [R-exts][171] manual.

If we want to install packages listed in 'Suggests' field, we should specify it explicitly by using _dependencies_ argument:

    install.packages(XXXX, dependencies = c("Depends", "Imports", "Suggests", "LinkingTo"))
    # OR
    install.packages(XXXX, dependencies = TRUE)

For example, if I use a plain install.packages() command to install [downloader][172] package

    install.packages("downloader")

it will only install 'digest' and 'downloader' packages. If I use

    install.packages("downloader", dependencies=TRUE)

it will also install 'testhat' package.

The **install.packages** function source code can be found in R -&gt; src -&gt; library -&gt; utils -&gt; R -&gt; [packages2.R][173] file from [Github][174] repository (put 'install.packages' in the search box).

#### Query an R package installed locally

    packageDescription("MASS")
    packageVersion("MASS")

#### Query an R package (from CRAN) basic information

    packageStatus() # Summarize information about installed packages

    available.packages() # List Available Packages at CRAN-like Repositories

The **available.packages()** command is useful for understanding package dependency. Use **setRepositories()** or 'RGUI -&gt; Packages -&gt; select repositories' to select repositories and **options()$repos** to check or change the repositories.

Also the **packageStatus()** is another useful function for query how many packages are in the repositories, how many have been installed, and individual package status (installed or not, needs to be upgraded or not).

    &gt; options()$repos

    &gt; packageStatus()
    Number of installed packages:

                                          ok upgrade unavailable
      C:/Program Files/R/R-3.0.1/library 110       0           1

    Number of available packages (each package counted only once):

                                                                                        installed not installed
      http://watson.nci.nih.gov/cran_mirror/bin/windows/contrib/3.0                            76          4563
      http://www.stats.ox.ac.uk/pub/RWin/bin/windows/contrib/3.0                                0             5
      http://www.bioconductor.org/packages/2.12/bioc/bin/windows/contrib/3.0                   16           625
      http://www.bioconductor.org/packages/2.12/data/annotation/bin/windows/contrib/3.0         4           686
    &gt; tmp &lt;- available.packages()
    &gt; str(tmp)
     chr [1:5975, 1:17] "A3" "ABCExtremes" "ABCp2" "ACCLMA" "ACD" "ACNE" "ADGofTest" "ADM3" "AER" ...
     - attr(*, "dimnames")=List of 2
      ..$&nbsp;: chr [1:5975] "A3" "ABCExtremes" "ABCp2" "ACCLMA" ...
      ..$&nbsp;: chr [1:17] "Package" "Version" "Priority" "Depends" ...
    &gt; tmp[1:3,]
                Package       Version Priority Depends                     Imports LinkingTo Suggests
    A3          "A3"          "0.9.2" NA       "xtable, pbapply"           NA      NA        "randomForest, e1071"
    ABCExtremes "ABCExtremes" "1.0"   NA       "SpatialExtremes, combinat" NA      NA        NA
    ABCp2       "ABCp2"       "1.1"   NA       "MASS"                      NA      NA        NA
                Enhances License      License_is_FOSS License_restricts_use OS_type Archs MD5sum NeedsCompilation File
    A3          NA       "GPL (&gt;= 2)" NA              NA                    NA      NA    NA     NA               NA
    ABCExtremes NA       "GPL-2"      NA              NA                    NA      NA    NA     NA               NA
    ABCp2       NA       "GPL-2"      NA              NA                    NA      NA    NA     NA               NA
                Repository
    A3          "http://watson.nci.nih.gov/cran_mirror/bin/windows/contrib/3.0"
    ABCExtremes "http://watson.nci.nih.gov/cran_mirror/bin/windows/contrib/3.0"
    ABCp2       "http://watson.nci.nih.gov/cran_mirror/bin/windows/contrib/3.0"

And the following commands find which package depends on Rcpp and also which are from bioconductor repository.

    &gt; pkgName &lt;- "Rcpp"
    &gt; rownames(tmp)[grep(pkgName, tmp[,"Depends"])]
    &gt; tmp[grep("Rcpp", tmp[,"Depends"]), "Depends"]

    &gt; ind &lt;- intersect(grep(pkgName, tmp[,"Depends"]), grep("bioconductor", tmp[, "Repository"]))
    &gt; rownames(grep)[ind]
    NULL
    &gt; rownames(tmp)[ind]
     [1] "ddgraph"            "DESeq2"             "GeneNetworkBuilder" "GOSemSim"           "GRENITS"
     [6] "mosaics"            "mzR"                "pcaMethods"         "Rdisop"             "Risa"
    [11] "rTANDEM"

#### Install personal R packages after upgrade R

Scenario: We already have installed many R packages under R 3.1.X in the user's directory. Now we upgrade R to a new version (3.2.X). We like to have these packages available in R 3.2.X.

For Windows OS, refer to [R for Windows FAQ][175]

The follow method works on Linux and Windows.

Make sure only one instance of R is running

    # Step 1. update R's built-in packages and install them on my personal directory
    update.packages(ask=FALSE, checkBuilt = TRUE, repos="http://cran.rstudio.com")

    # Step 2. update Bioconductor packages
    .libPaths() # The first one is my personal directory
    # [1] "/home/brb/R/x86_64-pc-linux-gnu-library/3.2"
    # [2] "/usr/local/lib/R/site-library"
    # [3] "/usr/lib/R/site-library"
    # [4] "/usr/lib/R/library"

    Sys.getenv("R_LIBS_USER") # equivalent to .libPaths()[1]
    ul &lt;- unlist(strsplit(Sys.getenv("R_LIBS_USER"), "/"))
    src &lt;- file.path(paste(ul[1:(length(ul)-1)], collapse="/"), "3.1")
    des &lt;- file.path(paste(ul[1:(length(ul)-1)], collapse="/"), "3.2")
    pkg &lt;- dir(src, full.names = TRUE)
    if (!file.exists(des)) dir.create(des)  # If 3.2 subdirectory does not exist yet!
    file.copy(pkg, des, overwrite=FALSE, recursive = TRUE)
    source("http://www.bioconductor.org/biocLite.R")
    biocLite(ask = FALSE)

From Robert Kabacoff (R in Action)

* If you have a customized **Rprofile.site file** (see appendix B), save a copy outside of R.
* Launch your current version of R and issue the following statements

    oldip &lt;- installed.packages()[,1]
    save(oldip, file="path/installedPackages.Rdata")

where _path_ is a directory outside of R.

* Download and install the newer version of R.
* If you saved a customized version of the Rprofile.site file in step 1, copy it into the new installation.
* Launch the new version of R, and issue the following statements

    load("path/installedPackages.Rdata")
    newip &lt;- installed.packages()[,1]
    for(i in setdiff(oldip, newip))
      install.packages(i)

where path is the location specified in step 2.

* Delete the old installation (optional).

This approach will install only packages that are available from the CRAN. It won't find packages obtained from other locations. In fact, the process will display a list of packages that can't be installed For example for packages obtained from Bioconductor, use the following method to update packages

    source(http://bioconductor.org/biocLite.R)
    biocLite(PKGNAME)

#### List installed packages and versions

    ip &lt;- as.data.frame(installed.packages()[,c(1,3:4)])
    rownames(ip) &lt;- NULL
    unique(ip$Priority)
    # [1] <na>        base        recommended
    # Levels: base recommended
    ip &lt;- ip[is.na(ip$Priority),1:2,drop=FALSE]
    print(ip, row.names=FALSE)

#### Query the names of outdated packages

    psi &lt;- packageStatus()$inst
    subset(psi, Status == "upgrade", drop = FALSE)
    #                     Package                                  LibPath     Version    Priority                Depends
    # RcppArmadillo RcppArmadillo C:/Users/brb/Documents/R/win-library/3.2 0.5.100.1.0        <na>                   <na>
    # Matrix               Matrix       C:/Program Files/R/R-3.2.0/library       1.2-0 recommended R (&gt;= 2.15.2), methods
    #                                             Imports LinkingTo                 Suggests
    # RcppArmadillo                      Rcpp (&gt;= 0.11.0)      Rcpp RUnit, Matrix, pkgKitten
    # Matrix        graphics, grid, stats, utils, lattice      <na>               expm, MASS
    #                                            Enhances    License License_is_FOSS License_restricts_use OS_type MD5sum
    # RcppArmadillo                                  <na> GPL (&gt;= 2)            <na>                  <na>    <na>   <na>
    # Matrix        MatrixModels, graph, SparseM, sfsmisc GPL (&gt;= 2)            <na>                  <na>    <na>   <na>
    #               NeedsCompilation Built  Status
    # RcppArmadillo              yes 3.2.0 upgrade
    # Matrix                     yes 3.2.0 upgrade

The above output does not show the package version from the latest packages on CRAN. So the following snippet does that.

    psi &lt;- packageStatus()$inst
    pl &lt;- unname(psi$Package[psi$Status == "upgrade"])  # List package names

    out &lt;- cbind(subset(psi, Status == "upgrade")[, c("Package", "Version")], ap[match(pl, ap$Package), "Version"])
    colnames(out)[2:3] &lt;- c("OldVersion", "NewVersion")
    rownames(out) &lt;- NULL
    out
    #         Package  OldVersion  NewVersion
    # 1 RcppArmadillo 0.5.100.1.0 0.5.200.1.0
    # 2        Matrix       1.2-0       1.2-1

To consider also the packages from Bioconductor, we have the following code. Note that "3.1" means the Bioconductor version and "3.2" is the R version. See [Bioconductor release versions][176] page.

    psic &lt;- packageStatus(repos = c(contrib.url(getOption("repos")),
                                    "http://bioconductor.org/packages/3.1/bioc/bin/windows/contrib/3.2",
                                    "http://www.bioconductor.org/packages/3.1/data/annotation/bin/windows/contrib/3.2"))$inst
    subset(psic, Status == "upgrade", drop = FALSE)
    pl &lt;- unname(psic$Package[psic$Status == "upgrade"])

    # ap &lt;- as.data.frame(available.packages()[, c(1,2,3)], stringsAsFactors = FALSE)
    ap   &lt;- as.data.frame(available.packages(c(contrib.url(getOption("repos")),
                                    "http://bioconductor.org/packages/3.1/bioc/bin/windows/contrib/3.2",
                                    "http://www.bioconductor.org/packages/3.1/data/annotation/bin/windows/contrib/3.2"))[, c(1:3)],
                          stringAsFactors = FALSE)

    out &lt;- cbind(subset(psic, Status == "upgrade")[, c("Package", "Version")], ap[match(pl, ap$Package), "Version"])
    colnames(out)[2:3] &lt;- c("OldVersion", "NewVersion")
    rownames(out) &lt;- NULL
    out
    #         Package  OldVersion  NewVersion
    # 1         limma      3.24.5      3.24.9
    # 2 RcppArmadillo 0.5.100.1.0 0.5.200.1.0
    # 3        Matrix       1.2-0       1.2-1

#### Fishing for packages in CRAN

#### Query top downloaded packages

#### Warning: cannot remove prior installation of package

<http: stackoverflow.com="" questions="" 15932152="" unloading-and-removing-a-loaded-package-withouth-restarting-r="">

Instance 1.

    # Install the latest hgu133plus2cdf package
    # Remove/Uninstall hgu133plus2.db package
    # Put/Install an old version of IRanges (eg version 1.18.2 while currently it is version 1.18.3)
    # Test on R 3.0.1
    library(hgu133plus2cdf) # hgu133pluscdf does not depend or import IRanges
    source("http://bioconductor.org/biocLite.R")
    biocLite("hgu133plus2.db", ask=FALSE) # hgu133plus2.db imports IRanges
    # Warning:cannot remove prior installation of package 'IRanges'
    # Open Windows Explorer and check IRanges folder. Only see libs subfolder.

Note:

* In the above example, all packages were installed under C:Program FilesRR-3.0.1library.
* In another instance where I cannot reproduce the problem, new R packages were installed under C:UsersxxxDocumentsRwin-library3.0. The different thing is IRanges package CAN be updated but if I use packageVersion("IRanges") command in R, it still shows the old version.
* The above were tested on a desktop.

Instance 2.

    # On a fresh R 3.2.0, I install Bioconductor's depPkgTools &amp; lumi packages. Then I close R, re-open it,
    # and install depPkgTools package again.
    &gt; source("http://bioconductor.org/biocLite.R")
    Bioconductor version 3.1 (BiocInstaller 1.18.2),&nbsp;?biocLite for help
    &gt; biocLite("pkgDepTools")
    BioC_mirror: http://bioconductor.org
    Using Bioconductor version 3.1 (BiocInstaller 1.18.2), R version 3.2.0.
    Installing package(s) 'pkgDepTools'
    trying URL 'http://bioconductor.org/packages/3.1/bioc/bin/windows/contrib/3.2/pkgDepTools_1.34.0.zip'
    Content type 'application/zip' length 390579 bytes (381 KB)
    downloaded 381 KB

    package 'pkgDepTools' successfully unpacked and MD5 sums checked
    Warning: cannot remove prior installation of package 'pkgDepTools'

    The downloaded binary packages are in
            C:UsersbrbAppDataLocalTempRtmpYd2l7idownloaded_packages
    &gt; library(pkgDepTools)
    Error in library(pkgDepTools)&nbsp;: there is no package called 'pkgDepTools'

The pkgDepTools library folder appears in C:UsersbrbDocumentsRwin-library3.2, but it is empty. The weird thing is if I try the above steps again, I cannot reproduce the problem.

#### Warning: Unable to move temporary installation

The problem seems to happen only on virtual machines (Virtualbox).

* **Warning: unable to move temporary installation `C:UsersbrbDocumentsRwin-library3.0fileed8270978f5quadprog` to `C:UsersbrbDocumentsRwin-library3.0quadprog`** when I try to run 'install.packages("forecast").
* **Warning: unable to move temporary installation 'C:UsersbrbDocumentsRwin-library3.2file5e0104b5b49plyr' to 'C:UsersbrbDocumentsRwin-library3.2plyr' ** when I try to run 'biocLite("lumi")'. The other dependency packages look fine although I am not sure if any unknown problem can happen (it does, see below).

Here is a note of my trouble shooting.

1. If I try to ignore the warning and load the lumi package. I will get an error.
2. If I try to run biocLite("lumi") again, it will only download &amp; install lumi without checking missing 'plyr' package. Therefore, when I try to load the lumi package, it will give me an error again.
3. Even I install the plyr package manually, library(lumi) gives another error - missing mclust package.

```
    &gt; biocLite("lumi")
    trying URL 'http://bioconductor.org/packages/3.1/bioc/bin/windows/contrib/3.2/BiocInstaller_1.18.2.zip'
    Content type 'application/zip' length 114097 bytes (111 KB)
    downloaded 111 KB
    ...
    package 'lumi' successfully unpacked and MD5 sums checked

    The downloaded binary packages are in
            C:UsersbrbAppDataLocalTempRtmpyUjsJDdownloaded_packages
    Old packages: 'BiocParallel', 'Biostrings', 'caret', 'DESeq2', 'gdata', 'GenomicFeatures', 'gplots', 'Hmisc', 'Rcpp', 'RcppArmadillo', 'rgl',
      'stringr'
    Update all/some/none? [a/s/n]: a
    also installing the dependencies 'Rsamtools', 'GenomicAlignments', 'plyr', 'rtracklayer', 'gridExtra', 'stringi', 'magrittr'

    trying URL 'http://bioconductor.org/packages/3.1/bioc/bin/windows/contrib/3.2/Rsamtools_1.20.1.zip'
    Content type 'application/zip' length 8138197 bytes (7.8 MB)
    downloaded 7.8 MB
    ...
    package 'Rsamtools' successfully unpacked and MD5 sums checked
    package 'GenomicAlignments' successfully unpacked and MD5 sums checked
    package 'plyr' successfully unpacked and MD5 sums checked
    Warning: unable to move temporary installation 'C:UsersbrbDocumentsRwin-library3.2file5e0104b5b49plyr'
             to 'C:UsersbrbDocumentsRwin-library3.2plyr'
    package 'rtracklayer' successfully unpacked and MD5 sums checked
    package 'gridExtra' successfully unpacked and MD5 sums checked
    package 'stringi' successfully unpacked and MD5 sums checked
    package 'magrittr' successfully unpacked and MD5 sums checked
    package 'BiocParallel' successfully unpacked and MD5 sums checked
    package 'Biostrings' successfully unpacked and MD5 sums checked
    Warning: cannot remove prior installation of package 'Biostrings'
    package 'caret' successfully unpacked and MD5 sums checked
    package 'DESeq2' successfully unpacked and MD5 sums checked
    package 'gdata' successfully unpacked and MD5 sums checked
    package 'GenomicFeatures' successfully unpacked and MD5 sums checked
    package 'gplots' successfully unpacked and MD5 sums checked
    package 'Hmisc' successfully unpacked and MD5 sums checked
    package 'Rcpp' successfully unpacked and MD5 sums checked
    package 'RcppArmadillo' successfully unpacked and MD5 sums checked
    package 'rgl' successfully unpacked and MD5 sums checked
    package 'stringr' successfully unpacked and MD5 sums checked

    The downloaded binary packages are in
            C:UsersbrbAppDataLocalTempRtmpyUjsJDdownloaded_packages
    &gt; library(lumi)
    Error in loadNamespace(i, c(lib.loc, .libPaths()), versionCheck = vI[[i]])&nbsp;:
      there is no package called 'plyr'
    Error: package or namespace load failed for 'lumi'
    &gt; search()
     [1] ".GlobalEnv"            "package:BiocInstaller" "package:Biobase"       "package:BiocGenerics"  "package:parallel"      "package:stats"
     [7] "package:graphics"      "package:grDevices"     "package:utils"         "package:datasets"      "package:methods"       "Autoloads"
    [13] "package:base"
    &gt; biocLite("lumi")
    BioC_mirror: http://bioconductor.org
    Using Bioconductor version 3.1 (BiocInstaller 1.18.2), R version 3.2.0.
    Installing package(s) 'lumi'
    trying URL 'http://bioconductor.org/packages/3.1/bioc/bin/windows/contrib/3.2/lumi_2.20.1.zip'
    Content type 'application/zip' length 18185326 bytes (17.3 MB)
    downloaded 17.3 MB

    package 'lumi' successfully unpacked and MD5 sums checked

    The downloaded binary packages are in
            C:UsersbrbAppDataLocalTempRtmpyUjsJDdownloaded_packages
    &gt; search()
     [1] ".GlobalEnv"            "package:BiocInstaller" "package:Biobase"       "package:BiocGenerics"  "package:parallel"      "package:stats"
     [7] "package:graphics"      "package:grDevices"     "package:utils"         "package:datasets"      "package:methods"       "Autoloads"
    [13] "package:base"
    &gt; library(lumi)
    Error in loadNamespace(i, c(lib.loc, .libPaths()), versionCheck = vI[[i]])&nbsp;:
      there is no package called 'plyr'
    Error: package or namespace load failed for 'lumi'
    &gt; biocLite("plyr")
    BioC_mirror: http://bioconductor.org
    Using Bioconductor version 3.1 (BiocInstaller 1.18.2), R version 3.2.0.
    Installing package(s) 'plyr'
    trying URL 'http://cran.rstudio.com/bin/windows/contrib/3.2/plyr_1.8.2.zip'
    Content type 'application/zip' length 1128621 bytes (1.1 MB)
    downloaded 1.1 MB

    package 'plyr' successfully unpacked and MD5 sums checked

    The downloaded binary packages are in
            C:UsersbrbAppDataLocalTempRtmpyUjsJDdownloaded_packages

    &gt; library(lumi)
    Error in loadNamespace(j &lt;- i[[1L]], c(lib.loc, .libPaths()), versionCheck = vI[[j]])&nbsp;:
      there is no package called 'mclust'
    Error: package or namespace load failed for 'lumi'

    &gt;&nbsp;?biocLite
    Warning messages:
    1: In read.dcf(file.path(p, "DESCRIPTION"), c("Package", "Version"))&nbsp;:
      cannot open compressed file 'C:/Users/brb/Documents/R/win-library/3.2/Biostrings/DESCRIPTION', probable reason 'No such file or directory'
    2: In find.package(if (is.null(package)) loadedNamespaces() else package, &nbsp;:
      there is no package called 'Biostrings'
    &gt; library(lumi)
    Error in loadNamespace(j &lt;- i[[1L]], c(lib.loc, .libPaths()), versionCheck = vI[[j]])&nbsp;:
      there is no package called 'mclust'
    In addition: Warning messages:
    1: In read.dcf(file.path(p, "DESCRIPTION"), c("Package", "Version"))&nbsp;:
      cannot open compressed file 'C:/Users/brb/Documents/R/win-library/3.2/Biostrings/DESCRIPTION', probable reason 'No such file or directory'
    2: In find.package(if (is.null(package)) loadedNamespaces() else package, &nbsp;:
      there is no package called 'Biostrings'
    Error: package or namespace load failed for 'lumi'
```

[Other people also have the similar problem][177]. The possible cause is the virus scanner locks the file and R cannot move them.

Some possible solutions:

1. Delete ALL folders under R/library (e.g. C:/Progra~1/R/R-3.2.0/library) folder and install the main package again using install.packages() or biocLite().
2. For specific package like 'lumi' from Bioconductor, we can [find out all dependency packages][178] and then install them one by one.
3. Find out and install the top level package which misses dependency packages.
    1. This is based on the fact that install.packages() or biocLite() **sometimes** just checks &amp; installs the 'Depends' and 'Imports' packages and **won't install all packages recursively**
    2. we can do a small experiment by removing a package which is not directly dependent/imported by another package; e.g. 'iterators' is not dependent/imported by 'glment' directly but indirectly. So if we run **remove.packages("iterators"); install.packages("glmnet")**, then the 'iterator' package is still missing.
    3. A real example is if the missing packages are 'Biostrings', 'limma', 'mclust' (these are packages that 'minfi' directly depends/imports although they should be installed when I run biocLite("lumi") command), then I should just run the command **remove.packages("minfi"); biocLite("minfi")**. If we just run biocLite("lumi") or biocLite("methylumi"), the missing packages won't be installed.

#### Error in download.file(url, destfile, method, mode = "wb", ...)

HTTP status was '404 Not Found'

Tested on an existing R-3.2.0 session. Note that VariantAnnotation 1.14.4 was just uploaded to Bioc.

    &gt; biocLite("COSMIC.67")
    BioC_mirror: http://bioconductor.org
    Using Bioconductor version 3.1 (BiocInstaller 1.18.3), R version 3.2.0.
    Installing package(s) 'COSMIC.67'
    also installing the dependency 'VariantAnnotation'

    trying URL 'http://bioconductor.org/packages/3.1/bioc/bin/windows/contrib/3.2/VariantAnnotation_1.14.3.zip'
    Error in download.file(url, destfile, method, mode = "wb", ...)&nbsp;:
      cannot open URL 'http://bioconductor.org/packages/3.1/bioc/bin/windows/contrib/3.2/VariantAnnotation_1.14.3.zip'
    In addition: Warning message:
    In download.file(url, destfile, method, mode = "wb", ...)&nbsp;:
      cannot open: HTTP status was '404 Not Found'
    Warning in download.packages(pkgs, destdir = tmpd, available = available, &nbsp;:
      download of package 'VariantAnnotation' failed
    installing the source package 'COSMIC.67'

    trying URL 'http://bioconductor.org/packages/3.1/data/experiment/src/contrib/COSMIC.67_1.4.0.tar.gz'
    Content type 'application/x-gzip' length 40999037 bytes (39.1 MB)

However, when I tested on a new R-3.2.0 (just installed in VM), the COSMIC package installation is successful. That VariantAnnotation version 1.14.4 was installed (this version was just updated today from Bioconductor).

The cause of the error is the [**available.package()][179]** function will read the rds file first from cache in a tempdir (C:UsersXXXXAppDataLocalTempRtmpYYYYYY). See lines 51-55 of <packages.r>.

     dest &lt;- file.path(tempdir(),
                       paste0("repos_", URLencode(repos, TRUE), ".rds"))
     if(file.exists(dest)) {
        res0 &lt;- readRDS(dest)
     } else {
        ...
     }

Since my R was opened 1 week ago, the rds file it reads today contains old information. Note that Bioconductor does not hold the source code or binary code for the old version of packages. This explains why biocLite() function broke. When I restart R, the original problem is gone.

#### Unload a package

See an example below.

    require(splines)
    detach(package:splines, unload=TRUE)

#### [METACRAN][180] \- Search and browse all CRAN/R packages

### R package dependecies

#### Bioconductor's [pkgDepTools][181] package

The is an example of querying the dependencies of the notorious 'lumi' package which often broke the installation script. I am using R 3.2.0 and Bioconductor 3.1.

The **getInstallOrder** function is useful to get a list of all (recursive) dependency packages.

    source("http://bioconductor.org/biocLite.R")
    if (!require(pkgDepTools)) {
      biocLite("pkgDepTools", ask = FALSE)
      library(pkgDepTools)
    }
    MkPlot &lt;- FALSE

    library(BiocInstaller)
    biocUrl &lt;- biocinstallRepos()["BioCsoft"]
    biocDeps &lt;- makeDepGraph(biocUrl, type="source", dosize=FALSE) # pkgDepTools defines its makeDepGraph()

    PKG &lt;- "lumi"
    if (MkPlot) {
      if (!require(Biobase))  {
        biocLite("Biobase", ask = FALSE)
        library(Biobase)
      }
      if (!require(Rgraphviz))  {
        biocLite("Rgraphviz", ask = FALSE)
        library(Rgraphviz)
      }
      categoryNodes &lt;- c(PKG, names(acc(biocDeps, PKG)[[1]]))
      categoryGraph &lt;- subGraph(categoryNodes, biocDeps)
      nn &lt;- makeNodeAttrs(categoryGraph, shape="ellipse")
      plot(categoryGraph, nodeAttrs=nn)   # Complete but plot is too complicated &amp; font is too small.
    }

    system.time(allDeps &lt;- makeDepGraph(biocinstallRepos(), type="source",
                               keep.builtin=TRUE, dosize=FALSE)) # takes a little while
    #    user  system elapsed
    # 175.737  10.994 186.875
    # Warning messages:
    # 1: In .local(from, to, graph)&nbsp;: edges replaced: 'SNPRelate|gdsfmt'
    # 2: In .local(from, to, graph)&nbsp;:
    #   edges replaced: 'RCurl|methods', 'NA|bitops'

    # When needed.only=TRUE, only those dependencies not currently installed are included in the list.
    x1 &lt;- sort(getInstallOrder(PKG, allDeps, needed.only=TRUE)$packages); x1
     [1] "affy"                              "affyio"
     [3] "annotate"                          "AnnotationDbi"
     [5] "base64"                            "beanplot"
     [7] "Biobase"                           "BiocParallel"
     [9] "biomaRt"                           "Biostrings"
    [11] "bitops"                            "bumphunter"
    [13] "colorspace"                        "DBI"
    [15] "dichromat"                         "digest"
    [17] "doRNG"                             "FDb.InfiniumMethylation.hg19"
    [19] "foreach"                           "futile.logger"
    [21] "futile.options"                    "genefilter"
    [23] "GenomeInfoDb"                      "GenomicAlignments"
    [25] "GenomicFeatures"                   "GenomicRanges"
    [27] "GEOquery"                          "ggplot2"
    [29] "gtable"                            "illuminaio"
    [31] "IRanges"                           "iterators"
    [33] "labeling"                          "lambda.r"
    [35] "limma"                             "locfit"
    [37] "lumi"                              "magrittr"
    [39] "matrixStats"                       "mclust"
    [41] "methylumi"                         "minfi"
    [43] "multtest"                          "munsell"
    [45] "nleqslv"                           "nor1mix"
    [47] "org.Hs.eg.db"                      "pkgmaker"
    [49] "plyr"                              "preprocessCore"
    [51] "proto"                             "quadprog"
    [53] "RColorBrewer"                      "Rcpp"
    [55] "RCurl"                             "registry"
    [57] "reshape"                           "reshape2"
    [59] "rngtools"                          "Rsamtools"
    [61] "RSQLite"                           "rtracklayer"
    [63] "S4Vectors"                         "scales"
    [65] "siggenes"                          "snow"
    [67] "stringi"                           "stringr"
    [69] "TxDb.Hsapiens.UCSC.hg19.knownGene" "XML"
    [71] "xtable"                            "XVector"
    [73] "zlibbioc"

    # When needed.only=FALSE the complete list of dependencies is given regardless of the set of currently installed packages.
    x2 &lt;- sort(getInstallOrder(PKG, allDeps, needed.only=FALSE)$packages); x2
     [1] "affy"                              "affyio"                            "annotate"
     [4] "AnnotationDbi"                     "base64"                            "beanplot"
     [7] "Biobase"                           "BiocGenerics"                      "BiocInstaller"
    [10] "BiocParallel"                      "biomaRt"                           "Biostrings"
    [13] "bitops"                            "bumphunter"                        "codetools"
    [16] "colorspace"                        "DBI"                               "dichromat"
    [19] "digest"                            "doRNG"                             "FDb.InfiniumMethylation.hg19"
    [22] "foreach"                           "futile.logger"                     "futile.options"
    [25] "genefilter"                        "GenomeInfoDb"                      "GenomicAlignments"
    [28] "GenomicFeatures"                   "GenomicRanges"                     "GEOquery"
    [31] "ggplot2"                           "graphics"                          "grDevices"
    [34] "grid"                              "gtable"                            "illuminaio"
    [37] "IRanges"                           "iterators"                         "KernSmooth"
    [40] "labeling"                          "lambda.r"                          "lattice"
    [43] "limma"                             "locfit"                            "lumi"
    [46] "magrittr"                          "MASS"                              "Matrix"
    [49] "matrixStats"                       "mclust"                            "methods"
    [52] "methylumi"                         "mgcv"                              "minfi"
    [55] "multtest"                          "munsell"                           "nleqslv"
    [58] "nlme"                              "nor1mix"                           "org.Hs.eg.db"
    [61] "parallel"                          "pkgmaker"                          "plyr"
    [64] "preprocessCore"                    "proto"                             "quadprog"
    [67] "RColorBrewer"                      "Rcpp"                              "RCurl"
    [70] "registry"                          "reshape"                           "reshape2"
    [73] "rngtools"                          "Rsamtools"                         "RSQLite"
    [76] "rtracklayer"                       "S4Vectors"                         "scales"
    [79] "siggenes"                          "snow"                              "splines"
    [82] "stats"                             "stats4"                            "stringi"
    [85] "stringr"                           "survival"                          "tools"
    [88] "TxDb.Hsapiens.UCSC.hg19.knownGene" "utils"                             "XML"
    [91] "xtable"                            "XVector"                           "zlibbioc"

    &gt; sort(setdiff(x2, x1)) # Not all R's base packages are included; e.g. 'base', 'boot', ...
     [1] "BiocGenerics"  "BiocInstaller" "codetools"     "graphics"      "grDevices"
     [6] "grid"          "KernSmooth"    "lattice"       "MASS"          "Matrix"
    [11] "methods"       "mgcv"          "nlme"          "parallel"      "splines"
    [16] "stats"         "stats4"        "survival"      "tools"         "utils"

![Lumi rgraphviz.svg][182]

#### [miniCRAN package][183]

Before we go into R, we need to install some packages from Ubuntu terminal. See [here][184].

    # Consider glmnet package (today is 4/29/2015)
    # Version:	2.0-2
    # Depends:	Matrix (≥ 1.0-6), utils, foreach
    # Suggests:	survival, knitr, lars
    if (!require("miniCRAN"))  {
      install.packages("miniCRAN", dependencies = TRUE, repos="http://cran.rstudio.com") # include 'igraph' in Suggests.
      library(miniCRAN)
    }
    if (!"igraph"&nbsp;%in% installed.packages()[,1]) install.packages("igraph")

    tags &lt;- "glmnet"
    pkgDep(tags, suggests=TRUE, enhances=TRUE) # same as pkgDep(tags)
    #  [1] "glmnet"    "Matrix"    "foreach"   "codetools" "iterators" "lattice"   "evaluate"  "digest"
    #  [9] "formatR"   "highr"     "markdown"  "stringr"   "yaml"      "mime"      "survival"  "knitr"
    # [17] "lars"

    dg &lt;- makeDepGraph(tags, suggests=TRUE, enhances=TRUE) # miniCRAN defines its makeDepGraph()
    plot(dg, legendPosition = c(-1, 1), vertex.size=20)

![MiniCRAN dep.svg][185] ![PkgDepTools dep.svg][186] ![Glmnet dep.svg][187]

We can also display the dependence for a package from the [Bioconductor][188] repository.

    tags &lt;- "DESeq2"
    # Depends	S4Vectors, IRanges, GenomicRanges, Rcpp (&gt;= 0.10.1), RcppArmadillo (&gt;= 0.3.4.4)
    # Imports	BiocGenerics(&gt;= 0.7.5), Biobase, BiocParallel, genefilter, methods, locfit, geneplotter, ggplot2, Hmisc
    # Suggests	RUnit, gplots, knitr, RColorBrewer, BiocStyle, airway,npasilla (&gt;= 0.2.10), DESeq, vsn
    # LinkingTo     Rcpp, RcppArmadillo
    index &lt;- function(url, type="source", filters=NULL, head=5, cols=c("Package", "Version")){
      contribUrl &lt;- contrib.url(url, type=type)
      available.packages(contribUrl, type=type, filters=filters)
    }

    bioc &lt;- local({
      env &lt;- new.env()
      on.exit(rm(env))
      evalq(source("http://bioconductor.org/biocLite.R", local=TRUE), env)
      biocinstallRepos() # return URLs
    })

    bioc
    #                                               BioCsoft
    #            "http://bioconductor.org/packages/3.0/bioc"
    #                                                BioCann
    # "http://bioconductor.org/packages/3.0/data/annotation"
    #                                                BioCexp
    # "http://bioconductor.org/packages/3.0/data/experiment"
    #                                              BioCextra
    #           "http://bioconductor.org/packages/3.0/extra"
    #                                                   CRAN
    #                                "http://cran.fhcrc.org"
    #                                              CRANextra
    #                   "http://www.stats.ox.ac.uk/pub/RWin"
    str(index(bioc["BioCsoft"])) # similar to cranJuly2014 object

    system.time(dg &lt;- makeDepGraph(tags, suggests=TRUE, enhances=TRUE, availPkgs = index(bioc["BioCsoft"]))) # Very quick!
    plot(dg, legendPosition = c(-1, 1), vertex.size=20)

![Deseq2 dep.svg][189] ![Lumi dep.svg][190]

The dependencies of [GenomicFeature][191] and [GenomicAlignments][192] are more complicated. So we turn the 'suggests' option to FALSE.

    tags &lt;- "GenomicAlignments"
    dg &lt;- makeDepGraph(tags, suggests=FALSE, enhances=FALSE, availPkgs = index(bioc["BioCsoft"]))
    plot(dg, legendPosition = c(-1, 1), vertex.size=20)

![Genomicfeature dep dep.svg][193] ![Genomicalignments dep.svg][194]

#### [MRAN][195] (CRAN only)

#### Reverse dependence

### Create a new R package, namespace, documentation

#### R package depends vs imports

In the namespace era Depends is never really needed. All modern packages have no technical need for Depends anymore. Loosely speaking the only purpose of Depends today is to expose other package's functions to the user without re-exporting them.

load = functions exported in myPkg are available to interested parties as myPkg::foo or via direct imports - essentially this means the package can now be used

attach = the namespace (and thus all exported functions) is attached to the search path - the only effect is that you have now added the exported functions to the global pool of functions - sort of like dumping them in the workspace (for all practical purposes, not technically)

import a function into a package = make sure that this function works in my package regardless of the search path (so I can write fn1 instead of pkg1::fn1 and still know it will come from pkg1 and not someone's workspace or other package that chose the same name)

* * *

The distinction is between "loading" and "attaching" a package. Loading it (which would be done if you had MASS::loglm, or imported it) guarantees that the package is initialized and in memory, but doesn't make it visible to the user without the explicit MASS:: prefix. Attaching it first loads it, then modifies the user's search list so the user can see it.

Loading is less intrusive, so it's preferred over attaching. Both library() and require() would attach it.

#### Create R package with devtools and roxygen2

A useful [post][196] by Jacob Montgomery. Do watch the [youtube video][197] there.

The process requires 3 components: RStudio software, devtools and [roxygen2][198] (creating documentation from R code) packages.

#### Minimal R package for submission

<https: stat.ethz.ch="" pipermail="" r-devel="" 2013-august="" 067257.html=""> and [CRAN Repository Policy][199].

### Detect number of running R instances in Windows

    C:Program FilesR&gt;tasklist /FI "IMAGENAME eq Rscript.exe"
    INFO: No tasks are running which match the specified criteria.

    C:Program FilesR&gt;tasklist /FI "IMAGENAME eq Rgui.exe"

    Image Name                     PID Session Name        Session#    Mem Usage
    ========================= ======== ================ =========== ============
    Rgui.exe                      1096 Console                    1     44,712 K

    C:Program FilesR&gt;tasklist /FI "IMAGENAME eq Rserve.exe"

    Image Name                     PID Session Name        Session#    Mem Usage
    ========================= ======== ================ =========== ============
    Rserve.exe                    6108 Console                    1    381,796 K

In R, we can use

    &gt; system('tasklist /FI "IMAGENAME eq Rgui.exe" ', intern = TRUE)
    [1] ""
    [2] "Image Name                     PID Session Name        Session#    Mem Usage"
    [3] "========================= ======== ================ =========== ============"
    [4] "Rgui.exe                      1096 Console                    1     44,804 K"

    &gt; length(system('tasklist /FI "IMAGENAME eq Rgui.exe" ', intern = TRUE))-3

### Editor

<http: en.wikipedia.org="" wiki="" r_(programming_language)#editors_and_ides="">

* Emacs + ESS. The ESS is useful in the case I want to tidy R code (the tidy_source() function in the formatR package sometimes gives errors; eg when I tested it on an R file like <getcomparisonresults.r> from BRB-ArrayTools v4.4 stable).
* [Rstudio][200] \- editor/R terminal/R graphics/file browser/package manager. The new version (0.98) also provides a new feature for debugging step-by-step.
* [geany][201] \- I like the feature that it shows defined functions on the side panel even for R code. RStudio can also do this (see the bottom of the code panel).
* [Rgedit][202] which includes a feature of splitting screen into two panes and run R in the bottom panel. See [here][203].
* Komodo IDE with browser preview <http: www.youtube.com="" watch?v="wv89OOw9roI"> at 4:06 and <http: docs.activestate.com="" komodo="" 4.4="" editor.html="">

### GUI for Data Analysis

#### Rcmdr

<http: cran.r-project.org="" web="" packages="" rcmdr="" index.html="">

#### Deducer

<http: cran.r-project.org="" web="" packages="" deducer="" index.html="">

### Scope

See [Assignments within functions][204] in the **An Introduction to R** manual.

**Note that any ordinary assignments done within the function are local and temporary and are lost after exit from the function.**

Example 1.

    &gt; ttt &lt;- data.frame(type=letters[1:5], JpnTest=rep("999", 5), stringsAsFactors = F)
    &gt; ttt
      type JpnTest
    1    a     999
    2    b     999
    3    c     999
    4    d     999
    5    e     999
    &gt; jpntest &lt;- function() { ttt$JpnTest[1] ="N5"; print(ttt)}
    &gt; jpntest()
      type JpnTest
    1    a      N5
    2    b     999
    3    c     999
    4    d     999
    5    e     999
    &gt; ttt
      type JpnTest
    1    a     999
    2    b     999
    3    c     999
    4    d     999
    5    e     999

Example 2. [How can we set global variables inside a function?][205] The answer is to use the "&lt;&lt;-" operator or **assign(, , envir = .GlobalEnv)** function.

Other resource: [Advanced R][206] by Hadley Wickham.

### Vectorization

<http: www.noamross.net="" blog="" 2014="" 4="" 16="" vectorization-in-r--why.html="">

#### Mean of duplicated rows

    &gt; attach(mtcars)
    &gt; head(mtcars)
                       mpg cyl disp  hp drat    wt  qsec vs am gear carb
    Mazda RX4         21.0   6  160 110 3.90 2.620 16.46  0  1    4    4
    Mazda RX4 Wag     21.0   6  160 110 3.90 2.875 17.02  0  1    4    4
    Datsun 710        22.8   4  108  93 3.85 2.320 18.61  1  1    4    1
    Hornet 4 Drive    21.4   6  258 110 3.08 3.215 19.44  1  0    3    1
    Hornet Sportabout 18.7   8  360 175 3.15 3.440 17.02  0  0    3    2
    Valiant           18.1   6  225 105 2.76 3.460 20.22  1  0    3    1
    &gt; aggdata &lt;-aggregate(mtcars, by=list(cyl,vs),
    +   FUN=mean, na.rm=TRUE)
    &gt; print(aggdata)
      Group.1 Group.2      mpg cyl   disp       hp     drat       wt     qsec vs
    1       4       0 26.00000   4 120.30  91.0000 4.430000 2.140000 16.70000  0
    2       6       0 20.56667   6 155.00 131.6667 3.806667 2.755000 16.32667  0
    3       8       0 15.10000   8 353.10 209.2143 3.229286 3.999214 16.77214  0
    4       4       1 26.73000   4 103.62  81.8000 4.035000 2.300300 19.38100  1
    5       6       1 19.12500   6 204.55 115.2500 3.420000 3.388750 19.21500  1
             am     gear     carb
    1 1.0000000 5.000000 2.000000
    2 1.0000000 4.333333 4.666667
    3 0.1428571 3.285714 3.500000
    4 0.7000000 4.000000 1.500000
    5 0.0000000 3.500000 2.500000
    &gt; detach(mtcars)

### Apply family

Vectorize, aggregate, apply, by, eapply, lapply, mapply, rapply, replicate, scale, sapply, split, tapply, and vapply. Check out [this][207].

Examples of using lapply + split on a data frame. See [rollingyours.wordpress.com][208].

However, apply is just a wrap of a loop. The performance is not better than a for loop. See

The package 'pbapply' creates a text-mode progress bar - it works on any platforms. On Windows platform, check out [this post][209]. It uses winProgressBar() and setWinProgressBar() functions.

[This][210] discusses why **vapply** is safer and faster than sapply.

### plyr and dplyr packages

[Paper][211] in J. Stat Software.

[A quick introduction to plyr][212] with a summary of apply functions in R and compare them with functions in plyr package.

1. plyr has a common syntax -- easier to remember
2. plyr requires less code since it takes care of the input and output format
3. plyr can easily be run in parallel -- faster

Tutorials

Examples of using dplyr:

#### llply()

llply is equivalent to lapply except that it will preserve labels and can display a progress bar. This is handy if we want to do a crazy thing.

    LLID2GOIDs &lt;- lapply(rLLID, function(x) get("org.Hs.egGO")[[x]])

where rLLID is a list of entrez ID. For example,

    get("org.Hs.egGO")[["6772"]]

returns a list of 49 GOs.

#### ddply()

<http: lamages.blogspot.com="" 2012="" 06="" transforming-subsets-of-data-in-r-with.html="">

#### ldply()

[An R Script to Automatically download PubMed Citation Counts By Year of Publication][213]

### [tidyr][214]

An evolution of reshape2. It's designed specifically for data tidying (not general reshaping or aggregating) and works well with dplyr data pipelines.

### mclapply() from paralle package is a mult-core version of lapply()

Note that Windows OS can not take advantage of it.

Another choice for Windows OS is to use parLapply() function in parallel package.

    ncores &lt;- as.integer( Sys.getenv('NUMBER_OF_PROCESSORS') )
    cl &lt;- makeCluster(getOption("cl.cores", ncores))
    LLID2GOIDs2 &lt;- parLapply(cl, rLLID, function(x) {
                                        library(org.Hs.eg.db); get("org.Hs.egGO")[[x]]}
                            )
    stopCluster(cl)

It does work. Cut the computing time from 100 sec to 29 sec on 4 cores.

### Regular Expression

Not specific to R

#### Syntax

The following table is from [endmemo.com][215].

```
| ----- |
|  Syntax  |  Description  |
|  \d  |  Digit, 0,1,2 ... 9  |
|  \D  |  Not Digit  |
|  \s  |  Space  |
|  \S  |  Not Space  |
|  \w  |  Word  |
|  \W  |  Not Word  |
|   |  Tab  |
|   |  New line  |
|  ^  |  Beginning of the string  |
|  $  |  End of the string  |
|   |  Escape special characters, e.g. \ is "", + is "+"  |
|   |  d)n/ matches "en" and "dn"  |
|  •  |  Any character, except n or line terminator  |
|  [ab]  |  a or b  |
|  [^ab]  |  Any character except a and b  |
|  [0-9]  |  All Digit  |
|  [A-Z]  |  All uppercase A to Z letters  |
|  [a-z]  |  All lowercase a to z letters  |
|  [A-z]  |  All Uppercase and lowercase a to z letters  |
|  i+  |  i at least one time  |
|  i*  |  i zero or more times  |
|  i?  |  i zero or 1 time  |
|  i{n}  |  i occurs n times in sequence  |
|  i{n1,n2}  |  i occurs n1 - n2 times in sequence  |
|  i{n1,n2}?  |  non greedy match, see above example  |
|  i{n,}  |  i occures &gt;= n times  |
|  [:alnum:]  |  Alphanumeric characters: [:alpha:] and [:digit:]  |
|  [:alpha:]  |  Alphabetic characters: [:lower:] and [:upper:]  |
|  [:blank:]  |  Blank characters: e.g. space, tab  |
|  [:cntrl:]  |  Control characters  |
|  [:digit:]  |  Digits: 0 1 2 3 4 5 6 7 8 9  |
|  [:graph:]  |  Graphical characters: [:alnum:] and [:punct:]  |
|  [:lower:]  |  Lower-case letters in the current locale  |
|  [:print:]  |  Printable characters: [:alnum:], [:punct:] and space  |
|  [:punct:]  |  } ~  |
|  [:space:]  |  Space characters: tab, newline, vertical tab, form feed, carriage return, space  |
|  [:upper:]  |  Upper-case letters in the current locale  |
|  [:xdigit:]  |  Hexadecimal digits: 0 1 2 3 4 5 6 7 8 9 A B C D E F a b c d e f  |
```

#### [grep()][216]

#### [sub() and gsub()][216]

The sub function changes only the first occurrence of the regular expression, while the gsub function performs the substitution on all occurrences within the string.

#### [regexpr() and gregexpr()][216]

The output from these functions is a vector of starting positions of the regular expressions which were found; if no match occurred, a value of -1 is returned.

The regexpr function will only provide information about the first match in its input string(s), while the gregexpr function returns information about all matches found.

The following example is coming from the book 'Data Manipulation with R' by [Phil Spector][217], Chapter 7, Character Manipulation.

    tst = c('one x7 two b1', 'three c5 four b9', 'five six seven', 'a8 eight nine')
    wh = regexpr('[a-z][0-9]', tst)
    wh
    # [1] 5 7 -1 1
    # attr(,"match.length")
    # [1] 2 2 -1 2

    wh1 = gregexpr('[a-z][0-9]',tst) # return a list just like strsplit()
    wh1

    # [[1]]
    # [1]  5 12
    # attr(,"match.length")
    # [1] 2 2
    # attr(,"useBytes")
    # [1] TRUE
    #
    # [[2]]
    # [1]  7 15
    # attr(,"match.length")
    # [1] 2 2
    # attr(,"useBytes")
    # [1] TRUE
    #
    # [[3]]
    # [1] -1
    # attr(,"match.length")
    # [1] -1
    # attr(,"useBytes")
    # [1] TRUE
    #
    # [[4]]
    # [1] 1
    # attr(,"match.length")
    # [1] 2
    # attr(,"useBytes")
    # [1] TRUE

#### Examples

* grep("\.zip$", pkgs) or grep("\.tar.gz$", pkgs) will search for the string ending with zip or tar.gz
* grep("9.11", string) will search for the string containing '9', any character (to split 9 &amp; 11) and '11'.
* pipe metacharacter; it is translated to 'or'. flood|fire will match strings containing floor or fire.
* [^?.]$ will match anyone not ending with the question mark or period.
* ^[Gg]ood|[Bb]ad will match strings starting with Good/good and anywhere containing Bad/bad.
* ^([Gg]ood|[Bb]ad) will look for strings beginning with Good/good/Bad/bad.
* &nbsp;? character; it means optional. [Gg]eorge( [Ww].)? [Bb]ush will match strings like 'george bush', 'George W. Bush' or 'george bushes'. Note that we escape the metacharacter dot by '.' so it becomes a literal period.
* star and plus sign. star means any number including none and plus means at least one. For example, (.*) matches 'abc(222 )' and '()'.
* [0-9]+ (.*) [0-9]+ will match one number and following by any number of characters and a number; e.g. 'afda1080 p' and '4 by 5 size'.
* {} refers to as interval quantifiers; specify the minimum and maximum number of match of an expression.

### Clipboard

    source("clipboard")
    read.table("clipboard")

### read/manipulate binary data

* x &lt;\- readBin(fn, raw(), file.info(fn)$size)
* rawToChar(x[1:16])
* See Biostrings C API

### String Manipulation

* [ebook][218] by Gaston Sanchez.
* Chapter 7 of the book 'Data Manipulation with R' by Phil Spector.
* Chapter 7 of the book 'R Cookbook' by Paul Teetor.
* Chapter 2 of the book 'Using R for Data Management, Statistical Analysis and Graphics' by Horton and Kleinman.
* <http: www.endmemo.com="" program="" r="" deparse.php="">. **It includes lots of examples for each R function it lists.**

### read/download/source a file from internet

#### Simple text file http

    retail &lt;- read.csv("http://robjhyndman.com/data/ausretail.csv",header=FALSE)

#### Zip file and url() function

    con = gzcon(url('http://www.systematicportfolio.com/sit.gz', 'rb'))
    source(con)
    close(con)

Here url() function is like file(), gzfile(), bzfile(), xzfile(), unz(), pipe(), fifo(), socketConnection(). They are used to create connections. By default, the connection is not opened (except for 'socketConnection'), but may be opened by setting a non-empty value of argument 'open'. See&nbsp;?url.

Another example of using url() is

    load(url("http:/www.example.com/example.RData"))

#### [downloader][172] package

This package provides a wrapper for the download.file function, making it possible to download files over https on Windows, Mac OS X, and other Unix-like platforms. The RCurl package provides this functionality (and much more) but can be difficult to install because it must be compiled with external dependencies. This package has no external dependencies, so it is much easier to install.

#### Google drive file based on https using [RCurl][219] package

    require(RCurl)
    myCsv &lt;- getURL("https://docs.google.com/spreadsheet/pub?hl=en_US&amp;hl=en_US&amp;key=0AkuuKBh0jM2TdGppUFFxcEdoUklCQlJhM2kweGpoUUE&amp;single=true&amp;gid=0&amp;output=csv")
    read.csv(textConnection(myCsv))

#### Github files https using RCurl package

    x = getURL("https://gist.github.com/arraytools/6671098/raw/c4cb0ca6fe78054da8dbe253a05f7046270d5693/GeneIDs.txt",
                ssl.verifypeer = FALSE)
    read.table(text=x)

### Create publication tables using **tables** package

See p13 for example in <http: www.ianwatson.com.au="" stata="" tabout_tutorial.pdf="">

R's [tables][220] packages is the best solution. For example,

    &gt; library(tables)
    &gt; tabular( (Species + 1) ~ (n=1) + Format(digits=2)*
    +          (Sepal.Length + Sepal.Width)*(mean + sd), data=iris )

                    Sepal.Length      Sepal.Width
     Species    n   mean         sd   mean        sd
     setosa      50 5.01         0.35 3.43        0.38
     versicolor  50 5.94         0.52 2.77        0.31
     virginica   50 6.59         0.64 2.97        0.32
     All        150 5.84         0.83 3.06        0.44
    &gt; str(iris)
    'data.frame':   150 obs. of  5 variables:
     $ Sepal.Length: num  5.1 4.9 4.7 4.6 5 5.4 4.6 5 4.4 4.9 ...
     $ Sepal.Width&nbsp;: num  3.5 3 3.2 3.1 3.6 3.9 3.4 3.4 2.9 3.1 ...
     $ Petal.Length: num  1.4 1.4 1.3 1.5 1.4 1.7 1.4 1.5 1.4 1.5 ...
     $ Petal.Width&nbsp;: num  0.2 0.2 0.2 0.2 0.2 0.4 0.3 0.2 0.2 0.1 ...
     $ Species    &nbsp;: Factor w/ 3 levels "setosa","versicolor",..: 1 1 1 1 1 1 1 1 1 1 ...

and

    # This example shows some of the less common options
    &gt; Sex &lt;- factor(sample(c("Male", "Female"), 100, rep=TRUE))
    &gt; Status &lt;- factor(sample(c("low", "medium", "high"), 100, rep=TRUE))
    &gt; z &lt;- rnorm(100)+5
    &gt; fmt &lt;- function(x) {
      s &lt;- format(x, digits=2)
      even &lt;- ((1:length(s))&nbsp;%% 2) == 0
      s[even] &lt;- sprintf("(%s)", s[even])
      s
    }
    &gt; tabular( Justify(c)*Heading()*z*Sex*Heading(Statistic)*Format(fmt())*(mean+sd) ~ Status )
                      Status
     Sex    Statistic high   low    medium
     Female mean       4.88   4.96   5.17
            sd        (1.20) (0.82) (1.35)
     Male   mean       4.45   4.31   5.05
            sd        (1.01) (0.93) (0.75)

See also a collection of R packages related to reproducible research in <http: cran.r-project.org="" web="" views="" reproducibleresearch.html="">

### Create flat tables in R console using ftable()

    &gt; ftable(Titanic, row.vars = 1:3)
                       Survived  No Yes
    Class Sex    Age
    1st   Male   Child            0   5
                 Adult          118  57
          Female Child            0   1
                 Adult            4 140
    2nd   Male   Child            0  11
                 Adult          154  14
          Female Child            0  13
                 Adult           13  80
    3rd   Male   Child           35  13
                 Adult          387  75
          Female Child           17  14
                 Adult           89  76
    Crew  Male   Child            0   0
                 Adult          670 192
          Female Child            0   0
                 Adult            3  20
    &gt; ftable(Titanic, row.vars = 1:2, col.vars = "Survived")
                 Survived  No Yes
    Class Sex
    1st   Male            118  62
          Female            4 141
    2nd   Male            154  25
          Female           13  93
    3rd   Male            422  88
          Female          106  90
    Crew  Male            670 192
          Female            3  20
    &gt; ftable(Titanic, row.vars = 2:1, col.vars = "Survived")
                 Survived  No Yes
    Sex    Class
    Male   1st            118  62
           2nd            154  25
           3rd            422  88
           Crew           670 192
    Female 1st              4 141
           2nd             13  93
           3rd            106  90
           Crew             3  20
    &gt; str(Titanic)
     table [1:4, 1:2, 1:2, 1:2] 0 0 35 0 0 0 17 0 118 154 ...
     - attr(*, "dimnames")=List of 4
      ..$ Class  &nbsp;: chr [1:4] "1st" "2nd" "3rd" "Crew"
      ..$ Sex    &nbsp;: chr [1:2] "Male" "Female"
      ..$ Age    &nbsp;: chr [1:2] "Child" "Adult"
      ..$ Survived: chr [1:2] "No" "Yes"
    &gt; x &lt;- ftable(mtcars[c("cyl", "vs", "am", "gear")])
    &gt; x
              gear  3  4  5
    cyl vs am
    4   0  0        0  0  0
           1        0  0  1
        1  0        1  2  0
           1        0  6  1
    6   0  0        0  0  0
           1        0  2  1
        1  0        2  2  0
           1        0  0  0
    8   0  0       12  0  0
           1        0  0  2
        1  0        0  0  0
           1        0  0  0
    &gt; ftable(x, row.vars = c(2, 4))
            cyl  4     6     8
            am   0  1  0  1  0  1
    vs gear
    0  3         0  0  0  0 12  0
       4         0  0  0  2  0  0
       5         0  1  0  1  0  2
    1  3         1  0  2  0  0  0
       4         2  6  2  0  0  0
       5         0  1  0  0  0  0
    &gt;
    &gt; ## Start with expressions, use table()'s "dnn" to change labels
    &gt; ftable(mtcars$cyl, mtcars$vs, mtcars$am, mtcars$gear, row.vars = c(2, 4),
             dnn = c("Cylinders", "V/S", "Transmission", "Gears"))

              Cylinders     4     6     8
              Transmission  0  1  0  1  0  1
    V/S Gears
    0   3                   0  0  0  0 12  0
        4                   0  0  0  2  0  0
        5                   0  1  0  1  0  2
    1   3                   1  0  2  0  0  0
        4                   2  6  2  0  0  0
        5                   0  1  0  0  0  0

### tracemem, data type, copy

[How to avoid copying a long vector][221]

### Tell if the current R is running in 32-bit or 64-bit mode

    8 * .Machine$sizeof.pointer

where **sizeof.pointer** returns the number of *bytes* in a C SEXP type and '8' means number of bits per byte.

### 32- and 64-bit

See [R-admin.html][222].

* For speed you may want to use a 32-bit build, but to handle large datasets a 64-bit build.
* Even on 64-bit builds of R there are limits on the size of R objects, some of which stem from the use of 32-bit integers (especially in FORTRAN code). For example, the dimensionas of an array are limited to 2^31 -1.
* Since R 2.15.0, it is possible to select '64-bit Files' from the standard installer even on a 32-bit version of Windows (2012/3/30).

### Handling length 2^31 and more in R 3.0.0

From R News for 3.0.0 release:

_There is a subtle change in behaviour for numeric index values 2^31 and larger. These never used to be legitimate and so were treated as NA, sometimes with a warning. They are now legal for long vectors so there is no longer a warning, and x[2^31] &lt;\- y will now extend the vector on a 64-bit platform and give an error on a 32-bit one. _

In R 2.15.2, if I try to assign a vector of length 2^31, I will get an error

    &gt; x &lt;- seq(1, 2^31)
    Error in from:to&nbsp;: result would be too long a vector

However, for R 3.0.0 (tested on my 64-bit Ubuntu with 16GB RAM. The R was compiled by myself):

    &gt; system.time(x &lt;- seq(1,2^31))
       user  system elapsed
      8.604  11.060 120.815
    &gt; length(x)
    [1] 2147483648
    &gt; length(x)/2^20
    [1] 2048
    &gt; gc()
                 used    (Mb) gc trigger    (Mb)   max used    (Mb)
    Ncells     183823     9.9     407500    21.8     350000    18.7
    Vcells 2147764406 16386.2 2368247221 18068.3 2148247383 16389.9
    &gt;

Note:

1. 2^31 length is about 2 Giga length. It takes about 16 GB (2^31*8/2^20 MB) memory.
2. On Windows, it is almost impossible to work with 2^31 length of data if the memory is less than 16 GB because virtual disk on Windows does not work well. For example, when I tested on my 12 GB Windows 7, the whole Windows system freezes for several minutes before I force to power off the machine.
3. My slide in <http: goo.gl="" g7sgx=""> shows the screenshots of running the above command on my Ubuntu and RHEL machines. As you can see the linux is pretty good at handling large (&gt; system RAM) data. That said, as long as your linux system is 64-bit, you can possibly work on large data without too much pain.
4. For large dataset, it makes sense to use database or specially crafted packages like [bigmemory][223] or [ff][224].

### NA in index

* Question: what is seq(1, 3)[c(1, 2, NA)]?

Answer: It will reserve the element with NA in indexing and return the value NA for it.

* Question: What is TRUE &amp; NA?

Answer: NA

* Question: What is FALSE &amp; NA?

Answer: FALSE

* Question: c("A", "B", NA)&nbsp;!= ""&nbsp;?

Answer: TRUE TRUE NA

* Question: which(c("A", "B", NA)&nbsp;!= "")&nbsp;?

Answer: 1 2

* Question: c(1, 2, NA)&nbsp;!= "" &amp;&nbsp;!is.na(c(1, 2, NA))&nbsp;?

Answer: TRUE TRUE FALSE

* Question: c("A", "B", NA)&nbsp;!= "" &amp;&nbsp;!is.na(c("A", "B", NA))&nbsp;?

Answer: TRUE TRUE FALSE

**Conclusion**: In order to exclude empty or NA for numerical or character data type, we can use **which()** or a convenience function **keep.complete(x) &lt;\- function(x) x&nbsp;!= "" &amp;&nbsp;!is.na(x)**. This will guarantee return logical values and not contain NAs.

Don't just use x&nbsp;!= "" OR&nbsp;!is.na(x).

### Data frame

### matrix vs data.frame

    ip1 &lt;- installed.packages()[,c(1,3:4)] # class(ip1) = 'matrix'
    unique(ip1$Priority)
    # Error in ip1$Priority&nbsp;: $ operator is invalid for atomic vectors
    unique(ip1[, "Priority"])   # OK

    ip2 &lt;- as.data.frame(installed.packages()[,c(1,3:4)], stringsAsFactors = FALSE) # matrix -&gt; data.frame
    unique(ip2$Priority)     # OK

### Print a vector by suppressing names

Use **unname**.

### Creating publication quality graphs in R

### Write unix format files on Windows and vice versa

<https: stat.ethz.ch="" pipermail="" r-devel="" 2012-april="" 063931.html="">

### with() and within() functions

within() is similar to with() except it is used to create new columns and merge them with the original data sets. See [youtube video][225].

    closePr &lt;- with(mariokart, totalPr - shipPr)
    head(closePr, 20)

    mk &lt;- within(mariokart, {
                 closePr &lt;- totalPr - shipPr
         })
    head(mk) # new column closePr

    mk &lt;- mariokart
    aggregate(. ~ wheels + cond, mk, mean)
    # create mean according to each level of (wheels, cond)

    aggregate(totalPr ~ wheels + cond, mk, mean)

    tapply(mk$totalPr, mk[, c("wheels", "cond")], mean)

### Scatterplot with rugs

    require(stats)  # both 'density' and its default method
    with(faithful, {
        plot(density(eruptions, bw = 0.15))
        rug(eruptions)
        rug(jitter(eruptions, amount = 0.01), side = 3, col = "light blue")
    })

![RugFunction.png][226]

### Draw a single plot with two different y-axes

### Draw Color Palette

### Read only specific columns

Use 'colClasses' option in read.table, read.delim, .... For example, the following example reads only the 3rd column of the text file and also changes its data type from a data frame to a vector. Note that we have include double quotes around NULL.

    x &lt;- read.table("var_annot.vcf", colClasses = c(rep("NULL", 2), "character", rep("NULL", 7)),
                    skip=62, header=T, stringsAsFactors = FALSE)[, 1]

To know the number of columns, we might want to read the first row first.

    library(magrittr)
    scan("var_annot.vcf", sep="t", what="character", skip=62, nlines=1, quiet=TRUE)&nbsp;%&gt;% length()

### Read excel files

My experience is to save the Excel file as csv file (before it can be read into R) if it is possible.

### Serialization

If we want to pass an R object to C (use recv() function), we can use writeBin() to output the stream size and then use serialize() function to output the stream to a file. See the [post][227] on R mailing list.

    &gt; a &lt;- list(1,2,3)
    &gt; a_serial &lt;- serialize(a, NULL)
    &gt; a_length &lt;- length(a_serial)
    &gt; a_length
    [1] 70
    &gt; writeBin(as.integer(a_length), connection, endian="big")
    &gt; serialize(a, connection)

In C++ process, I receive one int variable first to get the length, and then read <length> bytes from the connection.

### socketConnection

See&nbsp;?socketconnection.

#### Simple example

from the socketConnection's manual.

Open one R session

    con1 &lt;- socketConnection(port = 22131, server = TRUE) # wait until a connection from some client
    writeLines(LETTERS, con1)
    close(con1)

Open another R session (client)

    con2 &lt;- socketConnection(Sys.info()["nodename"], port = 22131)
    # as non-blocking, may need to loop for input
    readLines(con2)
    while(isIncomplete(con2)) {
       Sys.sleep(1)
       z &lt;- readLines(con2)
       if(length(z)) print(z)
    }
    close(con2)

#### Use nc in client

The client does not have to be the R. We can use telnet, nc, etc. See the post [here][228]. For example, on the client machine, we can issue

    nc localhost 22131   [ENTER]

Then the client will wait and show anything written from the server machine. The connection from nc will be terminated once close(con1) is given.

If I use the command

    nc -v -w 2 localhost -z 22130-22135

then the connection will be established for a short time which means the cursor on the server machine will be returned. If we issue the above nc command again on the client machine it will show the connection to the port 22131 is refused. PS. "-w" switch denotes the number of seconds of the timeout for connects and final net reads.

Some post I don't have a chance to read. <http: digitheadslabnotebook.blogspot.com="" 2010="" 09="" how-to-send-http-put-request-from-r.html="">

#### Use curl command in client

On the server,

    con1 &lt;- socketConnection(port = 8080, server = TRUE)

On the client,

    curl --trace-ascii debugdump.txt http://localhost:8080/

Then go to the server,

    while(nchar(x &lt;- readLines(con1, 1)) &gt; 0) cat(x, "n")

    close(con1) # return cursor in the client machine

#### Use telnet command in client

On the server,

    con1 &lt;- socketConnection(port = 8080, server = TRUE)

On the client,

    sudo apt-get install telnet
    telnet localhost 8080
    abcdefg
    hijklmn
    qestst

Go to the server,

    readLines(con1, 1)
    readLines(con1, 1)
    readLines(con1, 1)
    close(con1) # return cursor in the client machine

Some [tutorial][229] about using telnet on http request. And [this][230] is a summary of using telnet.

### Subsetting

[Subset assignment of R Language Definition][231] and [Manipulation of functions][232].

The result of the command **x[3:5] &lt;\- 13:15** is as if the following had been executed

    `*tmp*` &lt;- x
    x &lt;- "[&lt;-"(`*tmp*`, 3:5, value=13:15)
    rm(`*tmp*`)

### S3 and S4

To get the source code of S4 methods, we can use showMethod(), getMethod() and showMethod(). For example

    library(qrqc)
    showMethods("gcPlot")
    getMethod("gcPlot", "FASTQSummary") # get an error
    showMethod("gcPlot", "FASTQSummary") # good.

    library(IRanges)
    ir &lt;- IRanges(start=c(10, 20, 30), width=5)
    ir

    class(ir)
    ## [1] "IRanges"
    ## attr(,"package")
    ## [1] "IRanges"

    getClassDef(class(ir))
    ## Class "IRanges" [package "IRanges"]
    ##
    ## Slots:
    ##
    ## Name:            start           width           NAMES     elementType
    ## Class:         integer         integer characterORNULL       character
    ##
    ## Name:  elementMetadata        metadata
    ## Class: DataTableORNULL            list
    ##
    ## Extends:
    ## Class "Ranges", directly
    ## Class "IntegerList", by class "Ranges", distance 2
    ## Class "RangesORmissing", by class "Ranges", distance 2
    ## Class "AtomicList", by class "Ranges", distance 3
    ## Class "List", by class "Ranges", distance 4
    ## Class "Vector", by class "Ranges", distance 5
    ## Class "Annotated", by class "Ranges", distance 6
    ##
    ## Known Subclasses: "NormalIRanges"

### findInterval()

Related functions are cuts() and split(). See also

### do.call, rbind, lapply

Lots of examples. See for example [this one][233] for creating a data frame from a vector.

    x &lt;- readLines(textConnection("---CLUSTER 1 ---
     3
     4
     5
     6
     ---CLUSTER 2 ---
     9
     10
     8
     11"))

     # create a list of where the 'clusters' are
     clust &lt;- c(grep("CLUSTER", x), length(x) + 1L)

     # get size of each cluster
     clustSize &lt;- diff(clust) - 1L

     # get cluster number
     clustNum &lt;- gsub("[^0-9]+", "", x[grep("CLUSTER", x)])

     result &lt;- do.call(rbind, lapply(seq(length(clustNum)), function(.cl){
         cbind(Object = x[seq(clust[.cl] + 1L, length = clustSize[.cl])]
             , Cluster = .cl
             )
         }))

     result

         Object Cluster
    [1,] "3"    "1"
    [2,] "4"    "1"
    [3,] "5"    "1"
    [4,] "6"    "1"
    [5,] "9"    "2"
    [6,] "10"   "2"
    [7,] "8"    "2"
    [8,] "11"   "2"

### How to get examples from help file

See [this post][234]. Method 1:

    example(acf, give.lines=TRUE)

Method 2:

    Rd &lt;- utils:::.getHelpFile(?acf)
    tools::Rd2ex(Rd)

### "[" and "[[" with the sapply() function

Suppose we want to extract string from the id like "ABC-123-XYZ" before the first hyphen.

    sapply(strsplit("ABC-123-XYZ", "-"), "[", 1)

is the same as

    sapply(strsplit("ABC-123-XYZ", "-"), function(x) x[1])

### Dealing with date

    d1 = date()
    class(d1) # "character"
    d2 = Sys.Date()
    class(d2) # "Date"

    format(d2, "%a&nbsp;%b&nbsp;%d")

    library(lubridate); ymd("20140108") # "2014-01-08 UTC"
    mdy("08/04/2013") # "2013-08-04 UTC"
    dmy("03-04-2013") # "2013-04-03 UTC"
    ymd_hms("2011-08-03 10:15:03") # "2011-08-03 10:15:03 UTC"
    ymd_hms("2011-08-03 10:15:03", tz="Pacific/Auckland")
    # "2011-08-03 10:15:03 NZST"
    ?Sys.timezone
    x = dmy(c("1jan2013", "2jan2013", "31mar2013", "30jul2013"))
    wday(x[1]) # 3
    wday(x[1], label=TRUE) # Tues

### [Nonstandard evaluation][235]

* substitute(expr, env) - capture expression. substitute() is often paired with deparse() to create informative labels for data sets and plots.
* quote(expr) - similar to substitute() but do nothing??
* eval(expr, envir), evalq(expr, envir) - eval evaluates its first argument in the current scope before passing it to the evaluator: evalq avoids this.
* deparse(expr) - turns unevaluated expressions into character strings. For example,

    &gt; deparse(args(lm))
    [1] "function (formula, data, subset, weights, na.action, method = "qr", "
    [2] "    model = TRUE, x = FALSE, y = FALSE, qr = TRUE, singular.ok = TRUE, "
    [3] "    contrasts = NULL, offset, ...) "
    [4] "NULL"

    &gt; deparse(args(lm), width=20)
    [1] "function (formula, data, "        "    subset, weights, "
    [3] "    na.action, method = "qr", " "    model = TRUE, x = FALSE, "
    [5] "    y = FALSE, qr = TRUE, "       "    singular.ok = TRUE, "
    [7] "    contrasts = NULL, "           "    offset, ...) "
    [9] "NULL"

### Lazy evaluation in R functions arguments

**R function arguments are lazy — they're only evaluated if they're actually used**.

* Example 1. By default, R function arguments are lazy.

```
    f &lt;- function(x) {
      999
    }
    f(stop("This is an error!"))
    #&gt; [1] 999
```

* Example 2. If you want to ensure that an argument is evaluated you can use **force()**.

```
    add &lt;- function(x) {
      force(x)
      function(y) x + y
    }
    adders2 &lt;- lapply(1:10, add)
    adders2[[1]](10)
    #&gt; [1] 11
    adders2[[10]](10)
    #&gt; [1] 20
```

* Example 3. Default arguments are evaluated inside the function.

```
    f &lt;- function(x = ls()) {
      a &lt;- 1
      x
    }

    # ls() evaluated inside f:
    f()
    # [1] "a" "x"

    # ls() evaluated in global environment:
    f(ls())
    # [1] "add"    "adders" "f"
```

* Example 4. Laziness is useful in if statements — the second statement below will be evaluated only if the first is true.

```
    x &lt;- NULL
    if (!is.null(x) &amp;&amp; x &gt; 0) {

    }
```

### Backtick sign, infix/prefix/postfix operators

The backtick sign ` (not the single quote) refers to functions or variables that have otherwise reserved or illegal names; e.g. '&amp;&amp;', '+', '(', 'for', 'if', etc. See some examples in [this note][206].

[**infix][236]** operator.

```
    1 + 2    # infix
    + 1 2    # prefix
    1 2 +    # postfix
```

### List data type

#### [Calling a function given a list of arguments][206]

```
    &gt; args &lt;- list(c(1:10, NA, NA), na.rm = TRUE)
    &gt; do.call(mean, args)
    [1] 5.5
    &gt; mean(c(1:10, NA, NA), na.rm = TRUE)
    [1] 5.5
```

### Error handling and exceptions

```
    out &lt;- try({
      a &lt;- 1
      b &lt;- "x"
      a + b
    })

    elements &lt;- list(1:10, c(-1, 10), c(T, F), letters)
    results &lt;- lapply(elements, log)
    is.error &lt;- function(x) inherits(x, "try-error")
    succeeded &lt;-&nbsp;!sapply(results, is.error)
```

* tryCatch(): With tryCatch() you map conditions to handlers (like switch()), named functions that are called with the condition as an input. Note that try() is a simplified version of tryCatch().

```
    tryCatch(expr, ..., finally)

    show_condition &lt;- function(code) {
      tryCatch(code,
        error = function(c) "error",
        warning = function(c) "warning",
        message = function(c) "message"
      )
    }
    show_condition(stop("!"))
    #&gt; [1] "error"
    show_condition(warning("?!"))
    #&gt; [1] "warning"
    show_condition(message("?"))
    #&gt; [1] "message"
    show_condition(10)
    #&gt; [1] 10
```

### Using list type

#### Avoid if-else or switch

```
?plot.stepfun.

    y0 &lt;- c(1,2,4,3)
    sfun0  &lt;- stepfun(1:3, y0, f = 0)
    sfun.2 &lt;- stepfun(1:3, y0, f = .2)
    sfun1  &lt;- stepfun(1:3, y0, right = TRUE)

    for(i in 1:3)
      lines(list(sfun0, sfun.2, stepfun(1:3, y0, f = 1))[[i]], col = i)
    legend(2.5, 1.9, paste("f =", c(0, 0.2, 1)), col = 1:3, lty = 1, y.intersp = 1)

```

### Open a new Window device

```
X11() or dev.new()
```

### par()

```
?par
```

#### mgp

The margin line (in 'mex' units) for the axis title, axis labels and axis line. Note that 'mgp[1]' affects 'title' whereas 'mgp[2:3]' affect 'axis'. The default is 'c(3, 1, 0)'.

If we like to make the axis labels closer to an axis, we can use mgp=c(2.3, 1, 0) for example.

#### lty

1=solid (default), 2=dashed, 3=dotted, 4=dotdash, 5=longdash, 6=twodash

#### oma

The following trick is useful when we want to draw multiple plots with a common title.

    par(mfrow=c(1,2),oma = c(0, 0, 2, 0))  # oma=c(0, 0, 0, 0) by default
    plot(1:10,  main="Plot 1")
    plot(1:100,  main="Plot 2")
    mtext("Title for Two Plots", outer = TRUE, cex = 1.5) # outer=FALSE by default

## Resource

### Books

### Blogs

### Bug Tracking System

<https: bugs.r-project.org="" bugzilla3=""> and [Search existing bug reports][237]. Remember to select 'All' in the Status drop-down list.

[1]: http://cran.r-project.org/doc/manuals/r-release/R-admin.html#The-Windows-toolset
[2]: http://sourceforge.net/projects/mingw-w64/
[3]: https://stat.ethz.ch/pipermail/r-devel/2013-September/067410.html
[4]: /mediawiki/index.php/R#Create_a_standalone_Rmath_library "R"
[5]: http://cran.r-project.org/doc/manuals/r-release/R-admin.html
[6]: http://cran.r-project.org/doc/manuals/r-patched/R-admin.html#g_t64_002dbit-Windows-builds
[7]: /mediawiki/index.php/NAS "NAS"
[8]: /mediawiki/index.php/Raspberry "Raspberry"
[9]: /mediawiki/index.php/Beaglebone "Beaglebone"
[10]: /mediawiki/index.php/Udoo "Udoo"
[11]: http://cran.r-project.org/doc/manuals/R-admin.html#Installing-R-under-Unix_002dalikes
[12]: /mediawiki/index.php/Rserve "Rserve"
[13]: http://bugs.debian.org/cgi-bin/bugreport.cgi?bug=679180
[14]: /mediawiki/index.php/File:Build_R_log.txt "File:Build R log.txt"
[15]: /mediawiki/index.php/Beaglebone#Build_R_on_BBB "Beaglebone"
[16]: https://github.com/wch/r-source/commit/f1d91a0b34dbaa6ac807f3852742e3d646fbe95e
[17]: https://gist.github.com/arraytools/684a316f09a350a9850f
[18]: http://sites.psu.edu/theubunturblog/2012/08/09/installing-the-development-version-of-r-on-ubuntu-alongside-the-current-version-of-r/
[19]: http://cran.r-project.org/doc/manuals/R-admin.html#Getting-the-source-files
[20]: http://www.oracle.com/technetwork/java/javase/downloads/index.html
[21]: http://cran.r-project.org/web/views/WebTechnologies.html
[22]: https://github.com/yihui/knitr-examples/blob/master/009-slides.Rmd
[23]: http://yihui.name/knitr/demo/showcase/
[24]: http://en.wikipedia.org/wiki/Markdown
[25]: http://johnmacfarlane.net/pandoc/try/
[26]: http://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol
[27]: http://blog.abhijeetr.com/2010/04/very-simple-http-server-writen-in-c.html
[28]: http://en.wikipedia.org/wiki/List_of_Hello_world_program_examples#H
[29]: /mediawiki/index.php/C#Socket_Programming_Examples_using_C.2FC.2B.2B.2FQt "C"
[30]: http://www.rstudio.com/shiny/
[31]: http://webcache.googleusercontent.com/mediawiki/images/thumb/2/22/ShinyHello.png/100px-ShinyHello.png
[32]: http://webcache.googleusercontent.com/mediawiki/images/thumb/3/32/Shinympg.png/100px-Shinympg.png
[33]: http://webcache.googleusercontent.com/mediawiki/images/thumb/2/21/ShinyReactivity.png/100px-ShinyReactivity.png
[34]: http://webcache.googleusercontent.com/mediawiki/images/thumb/a/a3/ShinyTabsets.png/100px-ShinyTabsets.png
[35]: http://webcache.googleusercontent.com/mediawiki/images/thumb/c/cb/ShinyUpload.png/100px-ShinyUpload.png
[36]: http://cran.r-project.org/web/packages/websockets/index.html
[37]: http://rstudio.github.io/shiny/tutorial/#run-and-debug
[38]: https://github.com/rstudio/shiny-server
[39]: http://www.rstudio.com/shiny/server/
[40]: http://rstudio.github.io/shinydashboard/
[41]: http://www.shinyapps.io/
[42]: http://cran.r-project.org/web/packages/httpuv/index.html
[43]: http://rapache.net/
[44]: http://cran.r-project.org/web/packages/gWidgetsWWW/index.html
[45]: http://cran.r-project.org/web/packages/Rook/index.html
[46]: https://docs.google.com/present/view?id=0AUTe_sntp1JtZGdnbjVicTlfMzFuZDQ5dmJxNw
[47]: https://github.com/rstats/RookTutorial
[48]: http://www.rinfinance.com/agenda/2011/JeffHorner.pdf
[49]: http://jeffreyhorner.tumblr.com/post/4723187316/introducing-rook
[50]: http://jeffreybreen.wordpress.com/2011/04/25/4-lines-of-r-to-get-you-started-using-the-rook-web-server-interface/
[51]: http://webcache.googleusercontent.com/mediawiki/images/thumb/1/1b/Rook.png/100px-Rook.png
[52]: http://webcache.googleusercontent.com/mediawiki/images/thumb/2/2c/Rook2.png/100px-Rook2.png
[53]: http://webcache.googleusercontent.com/mediawiki/images/thumb/6/6d/Rookapprnorm.png/100px-Rookapprnorm.png
[54]: http://webcache.googleusercontent.com/mediawiki/images/thumb/2/25/Rookappsummary.png/100px-Rookappsummary.png
[55]: https://code.google.com/p/sumo/
[56]: http://www.stat.ucla.edu/~jeroen/stockplot
[57]: http://www.rforge.net/FastRWeb/
[58]: http://sysbio.mrc-bsu.cam.ac.uk/Rwui/tutorial/Instructions.html
[59]: http://cran.r-project.org/web/packages/CGIwithR/index.html
[60]: http://cran.r-project.org/web/packages/WebDevelopR/
[61]: http://www.rstudio.com/ide/docs/advanced/manipulate
[62]: https://github.com/rstudio/rstudio/tree/master/src/cpp/r/R/packages/manipulate
[63]: http://reference.wolfram.com/mathematica/tutorial/IntroductionToManipulate.html
[64]: https://github.com/att/rcloud
[65]: http://user2014.stat.ucla.edu/abstracts/talks/193_Harner.pdf
[66]: http://blog.rstudio.org/2014/11/24/rvest-easy-web-scraping-with-r/
[67]: http://www.ncbi.nlm.nih.gov/geo/
[68]: /mediawiki/index.php/GEO#R_packages "GEO"
[69]: http://cran.r-project.org/web/packages/sendplot/index.html
[70]: http://cran.r-project.org/web/packages/RIGHT/index.html
[71]: http://righthelp.github.io/tutorial/interactivity
[72]: http://cran.r-project.org/web/packages/d3Network/index.html
[73]: http://www.htmlwidgets.org/
[74]: http://taichi.selfip.net:81/RmirrorMap/Rmirror.html
[75]: http://taichi.selfip.net:81/Rsummary/R_reposit.html
[76]: http://taichi.selfip.net:81/Rsummary/cran.html
[77]: http://taichi.selfip.net:81/Rsummary/bioc.html
[78]: http://taichi.selfip.net:81/Rsummary/annotation.html
[79]: http://taichi.selfip.net:81/Rsummary/experiment.html
[80]: http://www.r-pkg.org/pkglist
[81]: http://shop.oreilly.com/product/0636920021421.do
[82]: http://www.stat.berkeley.edu/scf/paciorek-distribComp.pdf
[83]: http://danielmarcelino.com/parallel-processing/Parallel
[84]: http://webcache.googleusercontent.com/mediawiki/images/thumb/4/4e/WindowsSecurityAlert.png/100px-WindowsSecurityAlert.png
[85]: http://www.inside-r.org/r-doc/parallel/clusterApply
[86]: http://cran.r-project.org/web/packages/snow/index.html
[87]: http://cran.r-project.org/web/packages/multicore/index.html
[88]: http://cran.r-project.org/web/packages/foreach/index.html
[89]: http://cran.r-project.org/web/packages/Rmpi/index.html
[90]: http://www.youtube.com/watch?v=UQ8yKQcPTg0
[91]: /mediawiki/index.php/Qt "Qt"
[92]: http://webcache.googleusercontent.com/mediawiki/images/thumb/9/99/Qtdensity.png/100px-Qtdensity.png
[93]: http://www.webtoolkit.eu/wt
[94]: http://dirk.eddelbuettel.com/blog/2011/11/30/
[95]: http://stackoverflow.com/questions/13137770/fatal-error-unable-to-open-the-base-package
[96]: http://cran.r-project.org/web/views/HighPerformanceComputing.html
[97]: https://github.com/RevolutionAnalytics/RHadoop/wiki
[98]: http://cran.r-project.org/web/packages/XML/index.html
[99]: https://github.com/hadley/httr
[100]: http://cran.r-project.org/web/packages/curl/
[101]: http://cran.r-project.org/web/packages/gWidgets/index.html
[102]: http://cran.r-project.org/web/packages/GenOrd/index.html
[103]: http://statistical-research.com/simulating-random-multivariate-correlated-data-categorical-variables/?utm_source=rss&amp;utm_medium=rss&amp;utm_campaign=simulating-random-multivariate-correlated-data-categorical-variables
[104]: http://cran.r-project.org/web/packages/rjson/index.html
[105]: http://cran.r-project.org/web/packages/RJSONIO/index.html
[106]: http://webcache.googleusercontent.com/mediawiki/images/thumb/9/9d/GoogleVis.png/200px-GoogleVis.png
[107]: http://jeffreyhorner.tumblr.com/page/3
[108]: http://cran.r-project.org/web/packages/googleVis/index.html
[109]: /mediawiki/index.php/R#RJSONIO "R"
[110]: http://cran.r-project.org/web/packages/Rcpp/index.html
[111]: http://lists.r-forge.r-project.org/pipermail/rcpp-devel/
[112]: http://stackoverflow.com/questions/19034564/can-the-bh-r-package-link-to-boost-math-and-numeric
[113]: http://bash.cyberciti.biz/guide/Command_substitution
[114]: http://cran.r-project.org/web/packages/RcppParallel/index.html
[115]: http://cran.r-project.org/web/packages/caret/index.html
[116]: http://cran.r-project.org/web/packages/xlsx/index.html
[117]: http://cran.r-project.org/web/packages/openxlsx/index.html
[118]: https://github.com/hadley/readxl
[119]: http://cran.r-project.org/web/packages/ggplot2/index.html
[120]: https://github.com/cttobin/ggthemr
[121]: http://rpubs.com/danmirman/Rgroup-part1
[122]: https://github.com/smbache/magrittr
[123]: http://cran.r-project.org/web/packages/jpeg/index.html
[124]: http://cran.r-project.org/web/packages/Cairo/index.html
[125]: http://tolstoy.newcastle.edu.au/R/e6/help/09/05/15613.html
[126]: http://cran.r-project.org/web/packages/iterators/
[127]: http://rpubs.com/gaston/colortools
[128]: https://github.com/kevinushey/rex
[129]: http://cran.r-project.org/web/packages/formatR/index.html
[130]: http://stackoverflow.com/questions/3017877/tool-to-auto-format-r-code
[131]: http://cran.r-project.org/web/packages/biorxivr/index.html
[132]: http://cran.r-project.org/web/packages/aRxiv/index.html
[133]: http://cran.r-project.org/doc/manuals/r-devel/R-exts.html
[134]: https://stat.ethz.ch/pipermail/r-devel/2013-August/067107.html
[135]: https://stat.ethz.ch/pipermail/r-devel/2013-August/067109.html
[136]: http://epub.ub.uni-muenchen.de/2085/1/tr012.pdf
[137]: http://www.rforge.net/Rserve/doc.html
[138]: /mediawiki/index.php/R#Call_R_from_C.2FC.2B.2B "R"
[139]: /mediawiki/index.php/R#An_Example_from_Bioconductor_Workshop "R"
[140]: http://www.statconn.com/
[141]: http://rdotnet.codeplex.com/
[142]: http://stackoverflow.com/questions/3205302/difference-between-rscript-and-littler
[143]: /mediawiki/index.php/R#RInside "R"
[144]: http://www.rfortran.org/
[145]: http://cran.r-project.org/doc/manuals/R-admin.html#The-standalone-Rmath-library
[146]: /mediawiki/index.php/R#Build_R_from_its_source "R"
[147]: http://cran.r-project.org/doc/manuals/R-exts.html#Calling-R_002edll-directly%7CWriting
[148]: http://www.bioconductor.org/packages/release/bioc/html/ReportingTools.html
[149]: http://cran.r-project.org/web/packages/htmlTable/index.html
[150]: https://github.com/crubba/htmltab
[151]: http://cran.r-project.org/web/packages/ztable/index.html
[152]: http://cran.r-project.org/web/packages/reports/index.html
[153]: https://github.com/trinker/reports
[154]: http://yihui.name/knitr/demo/minimal/
[155]: http://www.bioconductor.org/packages/release/bioc/html/DESeq2.html
[156]: https://gist.github.com/jeromyanglim/2716336
[157]: http://cran.r-project.org/web/packages/pander/
[158]: http://cran.r-project.org/web/packages/R2wd/
[159]: http://cran.r-project.org/web/packages/rtf/
[160]: http://cran.r-project.org/web/packages/ReporteRs/index.html
[161]: http://www.omegahat.org/RDCOMClient/
[162]: http://cran.r-project.org/web/packages/excel.link/index.html
[163]: http://www.omegahat.org/RDCOMServer/
[164]: http://cran.r-project.org/web/packages/pubmed.mineR/index.html
[165]: http://webcache.googleusercontent.com/mediawiki/images/thumb/4/46/RStudio.jpg/100px-RStudio.jpg
[166]: /mediawiki/index.php/MySQL#Use_through_R "MySQL"
[167]: http://cran.r-project.org/web/packages/RSQLite/index.html
[168]: https://chitchatr.wordpress.com/2010/06/28/fun-with-persp-function/
[169]: http://webcache.googleusercontent.com/mediawiki/images/thumb/4/44/3dpersp.png/200px-3dpersp.png
[170]: http://webcache.googleusercontent.com/mediawiki/images/thumb/2/27/XMastree.png/150px-XMastree.png
[171]: http://cran.r-project.org/doc/manuals/r-release/R-exts.html
[172]: http://cran.r-project.org/web/packages/downloader/index.html
[173]: https://github.com/wch/r-source/blob/trunk/src/library/utils/R/packages2.R
[174]: https://github.com/wch/r-source
[175]: http://cran.r-project.org/bin/windows/base/rw-FAQ.html#What_0027s-the-best-way-to-upgrade_003f
[176]: http://bioconductor.org/about/release-announcements/#release-versions
[177]: http://r.789695.n4.nabble.com/unable-to-move-temporary-installation-td4521714.html
[178]: /mediawiki/index.php/R#Bioconductor.27s_pkgDepTools_package "R"
[179]: https://github.com/wch/r-source/blob/trunk/src/library/utils/R/packages.R
[180]: http://www.r-pkg.org/
[181]: http://www.bioconductor.org/packages/release/bioc/html/pkgDepTools.html
[182]: http://webcache.googleusercontent.com/mediawiki/images/thumb/f/f2/Lumi_rgraphviz.svg/200px-Lumi_rgraphviz.svg.png
[183]: http://cran.r-project.org/web/packages/miniCRAN/
[184]: /mediawiki/index.php/R#Ubuntu.2FDebian_2 "R"
[185]: http://webcache.googleusercontent.com/mediawiki/images/thumb/d/df/MiniCRAN_dep.svg/300px-MiniCRAN_dep.svg.png
[186]: http://webcache.googleusercontent.com/mediawiki/images/thumb/2/2a/PkgDepTools_dep.svg/300px-PkgDepTools_dep.svg.png
[187]: http://webcache.googleusercontent.com/mediawiki/images/thumb/9/90/Glmnet_dep.svg/300px-Glmnet_dep.svg.png
[188]: http://cran.r-project.org/web/packages/miniCRAN/vignettes/miniCRAN-non-CRAN-repos.html
[189]: http://webcache.googleusercontent.com/mediawiki/images/thumb/d/df/Deseq2_dep.svg/300px-Deseq2_dep.svg.png
[190]: http://webcache.googleusercontent.com/mediawiki/images/thumb/1/16/Lumi_dep.svg/300px-Lumi_dep.svg.png
[191]: http://www.bioconductor.org/packages/release/bioc/html/GenomicFeatures.html
[192]: http://www.bioconductor.org/packages/release/bioc/html/GenomicAlignments.html
[193]: http://webcache.googleusercontent.com/mediawiki/images/thumb/3/38/Genomicfeature_dep_dep.svg/300px-Genomicfeature_dep_dep.svg.png
[194]: http://webcache.googleusercontent.com/mediawiki/images/thumb/2/29/Genomicalignments_dep.svg/300px-Genomicalignments_dep.svg.png
[195]: http://mran.revolutionanalytics.com/
[196]: http://thepoliticalmethodologist.com/2014/08/14/building-and-maintaining-r-packages-with-devtools-and-roxygen2/
[197]: https://www.youtube.com/watch?v=9PyQlbAEujY#t=19
[198]: http://cran.r-project.org/web/packages/roxygen2/index.html
[199]: http://cran.r-project.org/web/packages/policies.html
[200]: http://www.rstudio.com/
[201]: http://www.geany.org/
[202]: http://rgedit.sourceforge.net/
[203]: http://www.stattler.com/article/using-gedit-or-rgedit-r
[204]: http://cran.r-project.org/doc/manuals/R-intro.html#Assignment-within-functions
[205]: http://stackoverflow.com/questions/1236620/global-variables-in-r
[206]: http://adv-r.had.co.nz/Functions.html
[207]: http://people.stern.nyu.edu/ylin/r_apply_family.html
[208]: http://rollingyours.wordpress.com/category/r-programming-apply-lapply-tapply/
[209]: http://www.theanalystatlarge.com/for-loop-tracking-windows-progress-bar/
[210]: http://stackoverflow.com/questions/12339650/why-is-vapply-safer-than-sapply
[211]: http://www.jstatsoft.org/v40/i01/paper
[212]: http://seananderson.ca/courses/12-plyr/plyr_2012.pdf
[213]: http://rpsychologist.com/an-r-script-to-automatically-look-at-pubmed-citation-counts-by-year-of-publication/
[214]: http://cran.r-project.org/web/packages/tidyr/index.html
[215]: http://www.endmemo.com/program/R/grep.php
[216]: https://stat.ethz.ch/R-manual/R-devel/library/base/html/grep.html
[217]: http://www.stat.berkeley.edu/~spector/
[218]: http://gastonsanchez.com/blog/resources/how-to/2013/09/22/Handling-and-Processing-Strings-in-R.html
[219]: http://www.omegahat.org/RCurl/FAQ.html
[220]: http://cran.r-project.org/web/packages/tables/index.html
[221]: http://stackoverflow.com/questions/18359940/r-programming-vector-a1-2-avoid-copying-the-whole-vector/18361181#18361181
[222]: http://cran.r-project.org/doc/manuals/R-admin.html#Choosing-between-32_002d-and-64_002dbit-builds
[223]: http://cran.r-project.org/web/packages/bigmemory/
[224]: http://cran.r-project.org/web/packages/ff/
[225]: http://www.youtube.com/watch?v=pZ6Bnxg9E8w&amp;list=PLOU2XLYxmsIK9qQfztXeybpHvru-TrqAP
[226]: http://webcache.googleusercontent.com/mediawiki/images/thumb/9/98/RugFunction.png/200px-RugFunction.png
[227]: https://stat.ethz.ch/pipermail/r-devel/attachments/20130628/56473803/attachment.pl
[228]: https://stat.ethz.ch/pipermail/r-sig-hpc/2009-April/000144.html
[229]: http://blog.gahooa.com/2009/01/23/basics-of-telnet-and-http/
[230]: http://unixhelp.ed.ac.uk/tables/telnet_commands.html
[231]: http://lib.stat.cmu.edu/R/CRAN/doc/manuals/R-lang.html#Subset-assignment
[232]: http://lib.stat.cmu.edu/R/CRAN/doc/manuals/R-lang.html#Manipulation-of-functions
[233]: https://stat.ethz.ch/pipermail/r-help/attachments/20140423/62d8d103/attachment.pl
[234]: https://stat.ethz.ch/pipermail/r-help/2014-April/369342.html
[235]: http://adv-r.had.co.nz/Computing-on-the-language.html
[236]: http://en.wikipedia.org/wiki/Infix_notation
[237]: https://bugs.r-project.org/bugzilla3/query.cgi
  </https:></http:></length></https:></http:></http:></http:></http:></http:></http:></http:></http:></http:></http:></getcomparisonresults.r></http:></https:></packages.r></http:></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></na></http:></https:></http:></https:></http:></http:></http:></http:></http:></https:></http:></http:></http:></http:></http:></knitr-minimal.rnw></http:></libr.so></r.dll></iostream></rmathex1.cpp></rmath.dll></librmath.a></rmathex1.cpp></rmath.dll></librmath.a></rmath.dll></librmath.a></http:></http:></qpushbutton.h></qapplication.h></rinside.h></makefile></makefile></http:></http:></rdefines.h></rembedded.h></embed.c></index.html></src></http:></http:></na></na></na></na></brca.xls></boost></boost></rcpp.h></http:></rcpp.h></http:></http:></http:></http:></http:></http:></https:></http:></https:></ftp:></http:></http:></http:></http:></https:></http:></http:></http:></http:></http:></http:></http:></http:></http:></https:></https:></http:></http:></http:></http:></http:></http:></index.html></http:></http:></dendrogram></https:></https:></main></http:></http:></https:></http:></http:></http:></http:></rcommand.bat></rcommand.bat></rcommand.bat></http:>