How to install R & Ipython on RPI2 


Install ZMQ3


```sh
$wget http://Download.zeromq.org/zeromq-3.2.4.tar.gz
$tar zxvf zeromq-3.2.4.tar.gz
#and in the new zeromq directory:
$sudo ./configure 
sudo make j2
sudo make j2 install
sudo ldconfig
```


```r
install.packages('rzmq', repos = c('http://irkernel.github.io/', getOption('repos')), type = 'source')
install.packages('repr', repos = c('http://irkernel.github.io/', getOption('repos')), type = 'source')
install.packages('IRkernel', repos = c('http://irkernel.github.io/', getOption('repos')), type = 'source')
install.packages('IRdisplay', repos = c('http://irkernel.github.io/', getOption('repos')), type = 'source')
```


Cython 16.0 

```sh
sudo apt-get install python-pip python-dev
wget http://pypi.python.org/packages/source/C/Cython/Cython-0.16.tar.gz
tar xvzf Cython-0.16.tar.gz
sudo python setup.py install
```

Cython 20

```sh
pip install cython==0.20.2
```

Cython 22

```sh
wget https://pypi.python.org/packages/source/C/Cython/Cython-0.22.1.tar.gz#md5=b920f3c81ae767477a342807bf5e68e8
tar xvzf Cython-0.22.1.tar.gz
sudo python setup.py install
```


Setup ipyton notebook remote server 
https://arundurvasula.wordpress.com/2014/04/01/remote-ipython-notebook-with-raspberry-pi/


How To Use Logstash and Kibana To Centralize Logs On Ubuntu 14.04
https://www.digitalocean.com/community/tutorials/how-to-use-logstash-and-kibana-to-centralize-and-visualize-logs-on-ubuntu-14-04

#### Cannot allocate memory 
Source - http://stackoverflow.com/a/27137346

I is all about the swap value.
```sh
#Know the swap value    
cat /proc/sys/vm/swappiness
1
# Access the swap configuration
sudo nano /etc/sysctl.conf

# Increase the swap usage to 10 (default is 1)
vm.swappiness=1
```


Install R Packages from source

```r
install.packages(package name, type="source")
```

Building R package getting error “ld: cannot find -lgfortran ”


Try `ldconfig -p | grep libgfortran` and it should show that `libgfortran.so.3` is found from the `/usr/lib/x86_64-linux-gnus` directory

Specifically, I was getting two errors:

```
/usr/bin/ld: cannot find -lgfortran
/usr/bin/ld: cannot find -lquadmath
```
Which were fixed by the lines:

```
sudo ln -s /usr/lib/x86_64-linux-gnu/libgfortran.so.3 /usr/lib/libgfortran.so
sudo ln -s /usr/lib/x86_64-linux-gnu/libquadmath.so.0 /usr/lib/libquadmath.so
```

Installed packages 

```
apt --installed list
capabilities("tcltk") 
gcc, make, gfortran, g++, liblzma-dev, tk, tk-dev, tcl, tcl-dev, libcairo2, libjpeg8, libreadline6
```


```
R is now configured for armv7l-unknown-linux-gnueabihf

  Source directory:          .
  Installation directory:    /usr/local

  C compiler:                gcc -std=gnu99  -g -O2
  Fortran 77 compiler:       gfortran  -g -O2

  C++ compiler:              g++  -g -O2
  C++ 11 compiler:           g++  -std=c++11 -g -O2
  Fortran 90/95 compiler:    gfortran -g -O2
  Obj-C compiler:	      

  Interfaces supported:      X11, tcltk
  External libraries:        readline, zlib, bzlib, lzma, PCRE, curl
  Additional capabilities:   PNG, JPEG, NLS, cairo, ICU
  Options enabled:           shared BLAS, R profiling

  Capabilities skipped:      TIFF
  Options not enabled:       memory profiling

  Recommended packages:      yes

configure: WARNING: you cannot build info or HTML versions of the R manuals
```


Source 
https://www.raspberrypi.org/forums/viewtopic.php?f=32&t=69529
