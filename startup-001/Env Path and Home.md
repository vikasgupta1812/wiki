Basic Linux Commands


EC2 SUSE LINUX 

```SH
env
```


```
LESSKEY=/etc/lesskey.bin
NNTPSERVER=news
MANPATH=/usr/local/man:/usr/share/man
XDG_SESSION_ID=2
HOSTNAME=ip-172-30-0-194
XKEYSYMDB=/usr/X11R6/lib/X11/XKeysymDB
HOST=ip-172-30-0-194
TERM=xterm-256color
SHELL=/bin/bash
PROFILEREAD=true
HISTSIZE=1000
SSH_CLIENT=65.31.252.31 61971 22
MORE=-sl
SSH_TTY=/dev/pts/0
USER=ec2-user
XNLSPATH=/usr/X11R6/lib/X11/nls
HOSTTYPE=x86_64
FROM_HEADER=
PAGER=less
CSHEDIT=emacs
XDG_CONFIG_DIRS=/etc/xdg
LIBGL_DEBUG=quiet
MINICOM=-c on
MAIL=/var/mail/ec2-user
PATH=/home/ec2-user/bin:/usr/local/bin:/usr/bin:/bin:/usr/games
CPU=x86_64
SSH_SENDS_LOCALE=yes
INPUTRC=/etc/inputrc
PWD=/home/ec2-user
LANG=en_US.UTF-8
PYTHONSTARTUP=/etc/pythonstart
GPG_TTY=/dev/pts/0
SHLVL=1
HOME=/home/ec2-user
LESS_ADVANCED_PREPROCESSOR=no
OSTYPE=linux
G_FILENAME_ENCODING=@locale,UTF-8,ISO-8859-15,CP1252
LESS=-M -I -R
MACHTYPE=x86_64-suse-linux
LOGNAME=ec2-user
XDG_DATA_DIRS=/usr/share
SSH_CONNECTION=65.31.252.31 61971 172.30.0.194 22
LESSOPEN=lessopen.sh %s
XDG_RUNTIME_DIR=/run/user/1000
NO_AT_BRIDGE=1
LESSCLOSE=lessclose.sh %s %s
G_BROKEN_FILENAMES=1
COLORTERM=1
_=/usr/bin/env
```

```sh
ec2-user@ip-172-30-0-194:~> env | grep '^PATH'
```
```
PATH=/home/ec2-user/bin:/usr/local/bin:/usr/bin:/bin:/usr/games
```

default order 

```
/home/ec2-user/bin
/usr/local/bin
/usr/bin
/bin
/usr/games
```




