[Source](http://wikemacs.org/wiki/Installing_Emacs_on_OS_X) 

```
$ brew update
```

```
error: Your local changes to the following files would be overwritten by merge:
	r.rb
Please, commit your changes or stash them before you can merge.
Aborting
Error: Failed to update tap: homebrew/science
Updated Homebrew from 6763ef1f to 77a79ee5.
==> New Formulae
blockhash		   fpp			      kafkacat
cppformat		   get-flash-videos	      kotlin-compiler
curlpp			   hebcal		      librdkafka
efl			   highlighting-kate	      moe
elementary		   homebrew/versions/go13     natalie
embulk			   homebrew/versions/mono3    rexster
evas-generic-loaders	   jpegrescan		      tasksh
exact-image		   jvgrep		      xonsh
==> Updated Formulae
ack					galen
afl-fuzz				gawk
alpine					gdal
android-platform-tools			gdal-grass
android-sdk				gearman
ansible					ghostscript
ant					git
apache-spark				git-integration
apktool					git-lfs
apr					gitbucket
apr-util				glib
arangodb				gnome-icon-theme
ascii					gnustep-make
asciidoc				gnutls
audiofile				gpgme
aws-elasticbeanstalk			gpsim
awscli					gptfdisk
base64					gradle
bear					groonga
beecrypt				groonga-normalizer-mysql
binwalk					gsettings-desktop-schemas
boost					gtk-doc
boost-bcp				guile
boost-python				hadoop
boot2docker				hbase
bullet					heroku-toolbelt
capstone				hh
carthage				hive
cask					homebank
chibi-scheme				homebrew/dupes/less
clang-format				homebrew/dupes/nano
clipsafe				homebrew/dupes/tcl-tk
codequery				homebrew/versions/bash-completion2
commonmark				homebrew/versions/elasticsearch-0.20
couchdb					homebrew/versions/elasticsearch090
cpp-netlib				homebrew/versions/elasticsearch11
creduce					homebrew/versions/elasticsearch12
cryptol					homebrew/versions/elasticsearch13
ctags					homebrew/versions/elasticsearch14
curl					homebrew/versions/gnutls34
dcd					homebrew/versions/llvm36
debianutils				homebrew/versions/lua53
deheader				homebrew/versions/mongodb24
dfu-util				homebrew/versions/open-mpi16
dmd					homebrew/versions/openjpeg20
dns2tcp					homebrew/versions/openjpeg21
dnscrypt-wrapper			homebrew/versions/perl514
dnsrend					homebrew/versions/perl516
docker					homebrew/versions/perl518
dpkg					homebrew/versions/play12
dtc					homebrew/versions/play13
dwdiff					homebrew/versions/subversion16
ejabberd				homebrew/versions/subversion17
elasticsearch				htop-osx
emscripten				http_load
exim					icu4c
fio					ilmbase
flactag					influxdb
fleetctl				iojs
fontforge				iprint
freeling				jbig2dec
freeradius-server			jbigkit
freetds					jed
fsharp					jenkins
fuse4x					juju
fuse4x-kext				jython
keepassc				nodebrew
keybase					nordugrid-arc
knot					npth
kubernetes-cli				nsq
lcrack					nss
le					num-utils
ledger					nut
libarchive				nuttcp
libcdr					nvm
libdsk					ocrad
libmspub				omniorb
libsass					open-mpi
libsodium				open-vcdiff
libswiften				openexr
libtasn1				openslide
libuv					openvpn
libvirt					ortp
libvisio				osquery
libwebsockets				osxfuse
libyubikey				patchutils
llvm					pazpar2
lmdb					pcre
logcheck				pcre2
logrotate				pdal
lua					pdfgrep
lua51					pdksh
macvim					percona-toolkit
magit					pgbadger
mapnik					pgbouncer
mariadb					pgpool-ii
maven					pianod
mdf2iso					pit
mecab-ko				pius
megatools				pixz
memcached				pkg-config
memtester				plowshare
mercurial				pmccabe
mesos					points2grid
metaproxy				postgis
midnight-commander			postgresql
minimodem				pound
minised					primesieve
minizinc				proguard
mitie					proof-general
mitmproxy				protobuf
mkcue					proxychains-ng
mksh					psftools
mkvalidator				psqlodbc
mkvtoolnix				pstoedit
modman					pulseaudio
monetdb					pup
mongodb					pure-ftpd
mongoose				purescript
monit					pushpin
mosquitto				pyenv
mpd					python
msgpack					python3
mu					qd
muparser				qt
mytop					qt5
nave					quantlib
ncdc					quazip
ncdu					quex
neko					quilt
nghttp2					rcssserver
nmap					re2
redis					suricata
rethinkdb				syncthing
riak					szl
robodoc					tachyon
rocksdb					tag
rpm					tbb
rrdtool					tcpflow
rt-audio				the_platinum_searcher
ruby-build				the_silver_searcher
rust					tika
safe-rm					tmux
saltstack				tomcat
sassc					tomcat-native
saxon					upx
sbcl					urlview
sblim-sfcc				utimer
sgrep					vcdimager
sha2					vim
shadowsocks-libev			vimpager
shellcheck				wellington
signing-party				whitedb
snappy					wine
spawn-fcgi				xml-security-c
sqlite					xmlrpc-c
sqlitebrowser				xmlto
squid					yaz
src					youtube-dl
sshfs					yubico-piv-tool
sshtrix					zebra
sslmate					zorba
stunnel
==> Deleted Formulae
git-latexdiff		   pure
homebrew/dupes/tidy	   shell.fm
```

```
$ brew install emacs --with-cocoa
```

```
==> Installing emacs dependency: xz
==> Downloading https://homebrew.bintray.com/bottles/xz-5.2.1.yosemite.bottle.ta
######################################################################## 100.0%
==> Pouring xz-5.2.1.yosemite.bottle.tar.gz
🍺  /usr/local/Cellar/xz/5.2.1: 59 files, 1.7M
==> Installing emacs
==> Downloading http://ftpmirror.gnu.org/emacs/emacs-24.5.tar.xz
######################################################################## 100.0%
==> ./configure --prefix=/usr/local/Cellar/emacs/24.5 --enable-locallisppath=/us
==> make
==> make install
==> Caveats
A command line wrapper for the cocoa app was installed to:
  /usr/local/Cellar/emacs/24.5/bin/emacs

To have launchd start emacs at login:
    ln -sfv /usr/local/opt/emacs/*.plist ~/Library/LaunchAgents
Then to load emacs now:
    launchctl load ~/Library/LaunchAgents/homebrew.mxcl.emacs.plist
.app bundles were installed.
Run `brew linkapps emacs` to symlink these to /Applications.
==> Summary
🍺  /usr/local/Cellar/emacs/24.5: 3923 files, 118M, built in 3.5 minutes
```
```
Vikas:bin Vikas$ brew linkapps emacs
Linking /usr/local/opt/emacs/Emacs.app to /Applications.
```

```
$ ln -s /usr/local/Cellar/emacs/24.5/Emacs.app /Applications
$ cp -r /usr/local/Cellar/emacs/24.5/Emacs.app /Applications/

```


```
$ ls -la /Applications | grep emacs
lrwxr-xr-x   1 Vikas  admin    38 May 10 21:32 Emacs.app -> /usr/local/Cellar/emacs/24.5/Emacs.app
```



> Further reading 
> - [Emacs on OSX](http://batsov.com/articles/2012/10/14/emacs-on-osx/)