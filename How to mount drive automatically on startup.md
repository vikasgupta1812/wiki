How to mount drive automatically on startup
=====


http://askubuntu.com/a/117898

Edit /etc/rc.local. This gets executed at startup after boot as root:

```
mount_at="/media/vikas/New Volume"
partition="/dev/sdc1"

if [ ! -d $mount_at ] #create mound directory if it doesn't exist
then
  mkdir $mount_at
fi

mount -t ntfs "/dev/sdc1" "/media/vikas/New Volume"
```


-----
http://askubuntu.com/a/910

A simple command (one which doesn't need to remain running) could use an Upstart job like:

```
start on startup
task
```


----
How to shutdown Ubuntu after (2 hours) of idle?

Shutdown after idle time 

http://askubuntu.com/a/461380