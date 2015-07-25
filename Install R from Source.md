Install R from source (ix86, x86_64 and arm platforms, Linux system)
====

Source: http://webcache.googleusercontent.com/search?q=cache:GoLoejt_LZAJ:taichimd.us/mediawiki/index.php/R+&cd=1&hl=en&ct=clnk&gl=us

### Debian system (focus on arm architecture with notes from x86 system)

#### Simplest configuration

On my debian system in Pogoplug (armv5), Raspberry Pi (armv6) OR Beaglebone Black & Udoo(armv7), I can compile R. See R's [admin manual](http://cran.r-project.org/doc/manuals/R-admin.html#Installing-R-under-Unix_002dalikes). If the OS needs x11, I just need to install 2 required packages.


install gfortran: 
```
apt-get install build-essential gfortran ##(gfortran is not part of build-essential)
```

install readline library: 
```
apt-get install libreadline5-dev (pogoplug), 
apt-get install libreadline6-dev (raspberry pi/BBB), 
apt-get install libreadline-dev (Ubuntu)
```
Note: if I need X11, I should install

```
libX11 and libX11-devel, libXt, 
libXt-devel (for fedora)
libx11-dev (for debian) or xorg-dev (for pogoplug/raspberry pi/BBB)
```
and optional `texinfo` (to fix 'WARNING: you cannot build info or HTML versions of the R manuals')

Note that it is also safe to install required tools via (please `run nano /etc/apt/sources.list` to include the repository of your favorite R mirror and also run `sudo apt-get update` first)

```
sudo apt-get build-dep r-base
```
The above command will install R dependence like `jdk`, `tcl`, `tex`, etc. The `apt-get build-dep` gave a more complete list than `apt-get install r-base-dev` for some reasons.

[Arm architecture] I also run `apt-get install readline-common`. I don't know if this is necessary. If `x11` is not needed or not available (eg Pogoplug), I can add `--with-x=no` option in `./configure` command. If R will be called from other applications such as `Rserve`, I can add `--enable-R-shlib` option in `./configure` command. Check out `./configure --help` to get a complete list of all options.

After running

```
wget http://cran.r-project.org/src/base/R-2/R-2.15.2.tar.gz
tar xzvf R-2.15.2.tar.gz
cd R-2.15.2
./configure --enable-R-shlib
```

(--enable-R-shlib option will create a shared R library libR.so in $RHOME/lib subdirectory. This allows R to be embedded in other applications. See Embedding R.) I got

```
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
```
After that, we can run `make` to create R binary. If the computer has multiple cores, we can run make in parallel by using the `-j` flag (for example, `-j 4` means to run 4 jobs simultaneously). We can also add time command in front of make to report the make time (useful for benchmark).

```
make   
# make -j 4
# time make
```
PS 1. On my raspberry pi machine, it shows R is now configured for armv6l-unknown-linux-gnueabihf and on Beaglebone black it shows R is now configured for armv7l-unknown-linux-gnueabihf.

PS 2. On my Beaglebone black, it took 2 hours to run 'make' and it only took 5 minutes to run 'make -j 12' on my Xeon W3690 @ 3.47Ghz (6 cores with hyperthread) based on R 3.1.2. The timing is obtained by using 'time' command as described above.

PS 3. On my x86 system, it shows

```
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
```
[arm] However, make gave errors for recommended packages like `KernSmooth`, `MASS`, `boot`, `class`, `cluster`, `codetools`, `foreign`, `lattice`, `mgcv`, `nlme`, `nnet`, `rpart`, `spatial`, and `survival`. The error stems from gcc: SHLIB_LIBADD: No such file or directory. Note that I can get this error message even I try `install.packages("MASS", type="source")`. A suggested fix is here; adding `perl = TRUE` in `sub()` call for two lines in `src/library/tools/R/install.R` file. However, I got another error shared object 'MASS.so' not found. See also `http://ftp.debian.org/debian/pool/main/r/r-base/`. To build R without recommended packages like `./configure --without-recommended`.

```
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
```

```sh
root@debian:/mnt/usb/R-2.15.2#
root@debian:/mnt/usb/R-2.15.2# bin/R
```

```
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
```
```
> library(MASS)
Error in library(MASS) : there is no package called 'MASS'
```
```
> library()
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
```
```
> Sys.info()["machine"]
   machine
"armv5tel"
```
```
> gc()
         used (Mb) gc trigger (Mb) max used (Mb)
Ncells 170369  4.6     350000  9.4   350000  9.4
Vcells 163228  1.3     905753  7.0   784148  6.0
See http://bugs.debian.org/cgi-bin/bugreport.cgi?bug=679180
```


#### Full configuration

```
  Interfaces supported:      X11, tcltk
  External libraries:        readline
  Additional capabilities:   PNG, JPEG, TIFF, NLS, cairo
  Options enabled:           shared R library, shared BLAS, R profiling, Java
Update: R 3.0.1 on Beaglebone Black (armv7a) + Ubuntu 13.04
```
See the page here.

Update: R 3.1.3 & R 3.2.0 on Raspberry Pi 2

It took 134m to run 'make -j 4' on RPi 2 using R 3.1.3.

But I got an error when I ran './configure; make -j 4' using R 3.2.0. The errors start from compiling <main/connections.c> file with 'undefined reference to ....'. The gcc version is 4.6.3.

Install all dependencies for building R

This is a comprehensive list. This list is even larger than r-base-dev.

```
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
```