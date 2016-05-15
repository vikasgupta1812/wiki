Logstash troubleshooting 
=====

```sh
ps --cols 1000 -o cputime,etime,rss,args, -u logstash
```
Watch memory usage 

```sh
watch -n 5 free -m
```

[http://www.pixelbeat.org/scripts/](http://www.pixelbeat.org/scripts/)

```
 Private  +   Shared  =  RAM used	Program

  4.0 KiB +  34.0 KiB =  38.0 KiB	rc
  4.0 KiB +  37.0 KiB =  41.0 KiB	elasticsearch
  4.0 KiB +  61.0 KiB =  65.0 KiB	dbus-daemon
 20.0 KiB +  47.5 KiB =  67.5 KiB	atd
  4.0 KiB +  68.5 KiB =  72.5 KiB	xinetd
 48.0 KiB +  45.5 KiB =  93.5 KiB	init
 44.0 KiB +  58.0 KiB = 102.0 KiB	thd
 64.0 KiB +  88.0 KiB = 152.0 KiB	ifplugd (2)
104.0 KiB +  53.0 KiB = 157.0 KiB	redis-server
104.0 KiB +  68.5 KiB = 172.5 KiB	cron
  8.0 KiB + 193.0 KiB = 201.0 KiB	udevd (3)
236.0 KiB +  48.5 KiB = 284.5 KiB	dhcpcd5
260.0 KiB +  53.5 KiB = 313.5 KiB	mount.ntfs
140.0 KiB + 199.5 KiB = 339.5 KiB	avahi-daemon (2)
384.0 KiB + 102.5 KiB = 486.5 KiB	ntpd
672.0 KiB +  66.5 KiB = 738.5 KiB	startpar
812.0 KiB +  99.0 KiB = 911.0 KiB	rsyslogd
888.0 KiB + 139.0 KiB =   1.0 MiB	nmbd
752.0 KiB + 356.5 KiB =   1.1 MiB	smbd (2)
816.0 KiB + 325.5 KiB =   1.1 MiB	sudo (3)
756.0 KiB + 486.0 KiB =   1.2 MiB	supervisord
  1.9 MiB +  84.5 KiB =   2.0 MiB	bash
848.0 KiB +   2.1 MiB =   3.0 MiB	sshd (5)
 58.4 MiB + 117.5 KiB =  58.5 MiB	node
354.0 MiB +   3.7 MiB = 357.7 MiB	java (2)

                        429.7 MiB

```
[https://help.ubuntu.com/community/Byobu]()


```
  PID USER      PRI  NI  VIRT   RES   SHR S CPU% MEM%   TIME+  Command
13354 pi         20   0  4576  3208  1960 R  5.0  0.3  0:00.73 htop
 3423 root       20   0 1226M  292M  4528 S  0.0 31.6  1h30:35 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3446 root       20   0 1226M  292M  4528 S  0.0 31.6  1:43.00 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3475 root       20   0 1226M  292M  4528 S  0.0 31.6  3:32.83 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3492 root       20   0 1226M  292M  4528 S  0.0 31.6  3:33.25 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3493 root       20   0 1226M  292M  4528 S  0.0 31.6  3:37.65 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3495 root       20   0 1226M  292M  4528 S  0.0 31.6  3:37.23 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
    1 root       20   0  2244  1164  1136 S  0.0  0.1  0:14.25 init [2]
  183 root       20   0  3244  1296  1296 S  0.0  0.1  0:00.45 udevd --daemon
 1613 root       20   0  1820   884   864 S  0.0  0.1  0:31.25 /usr/sbin/ifplugd -i lo -q -f -u0 -d10 -w -I
 1618 root       20   0  1820  1036   992 S  0.0  0.1  2:36.21 /usr/sbin/ifplugd -i eth0 -q -f -u0 -d10 -w -I
 1963 root       20   0  1828   924   920 S  0.0  0.1  0:00.01 /bin/sh /etc/init.d/rc 2
 1972 root       20   0  2480  2164  1540 S  0.0  0.2  2:00.24 startpar -p 4 -t 20 -T 3 -M start -P N -R 2
 2045 nobody     20   0  2220  1172  1156 S  0.0  0.1  0:03.91 /usr/sbin/thd --daemon --triggers /etc/triggerhappy/triggers.d/ --socket /var/run/thd.socket --pidfile /var/run/thd.pid --user nobody /dev/in
 2099 root       20   0 32588  2196  1904 S  0.0  0.2  1:53.51 /usr/sbin/rsyslogd -c5
 2100 root       20   0 32588  2196  1904 S  0.0  0.2  0:20.80 /usr/sbin/rsyslogd -c5
 2101 root       20   0 32588  2196  1904 S  0.0  0.2  0:00.01 /usr/sbin/rsyslogd -c5
 2092 root       20   0 32588  2196  1904 S  0.0  0.2  2:14.34 /usr/sbin/rsyslogd -c5
 2154 daemon     20   0  2368    12     0 S  0.0  0.0  0:00.01 /usr/sbin/atd
 2253 root       20   0  4100  1400  1328 S  0.0  0.1  0:01.03 /usr/sbin/cron
 2303 root       20   0  3240  1112  1108 S  0.0  0.1  0:00.01 udevd --daemon
 2304 root       20   0  3240  1112  1108 S  0.0  0.1  0:00.01 udevd --daemon
 2321 ntp        20   0  6060  2028  1824 S  0.0  0.2  0:42.53 /usr/sbin/ntpd -p /var/run/ntpd.pid -g -u 104:107
 2334 messagebu  20   0  3504   588   584 S  0.0  0.1  0:00.00 /usr/bin/dbus-daemon --system
 2340 root       20   0 10696  2248  2144 S  0.0  0.2  0:13.81 /usr/sbin/nmbd -D
 2405 root       20   0 19792  3072  2820 S  0.0  0.3  0:03.64 /usr/sbin/smbd -D
 2428 avahi      20   0  3804  1488  1308 S  0.0  0.2  1:15.84 avahi-daemon: running [pi.local]
 2430 avahi      20   0  3704    48     0 S  0.0  0.0  0:00.00 avahi-daemon: chroot helper
 2437 redis      20   0 27520  1204  1144 S  0.0  0.1  0:00.00 /usr/bin/redis-server /etc/redis/redis.conf
 2438 redis      20   0 27520  1204  1144 S  0.0  0.1  0:00.00 /usr/bin/redis-server /etc/redis/redis.conf
 2436 redis      20   0 27520  1204  1144 S  0.0  0.1  4:24.36 /usr/bin/redis-server /etc/redis/redis.conf
 2494 root       20   0 20308   516   452 S  0.0  0.1  0:00.00 /usr/sbin/smbd -D
 2520 root       20   0  6828  1960  1864 S  0.0  0.2  0:42.14 /usr/sbin/sshd
 2606 root       20   0  3312  1356  1356 S  0.0  0.1  0:00.01 /usr/sbin/xinetd -pidfile /var/run/xinetd.pid -stayalive -inetd_compat
 2883 root       20   0 14380  2616  1932 S  0.0  0.3  2:48.25 /usr/bin/python /usr/bin/supervisord -c /etc/supervisor/supervisord.conf
 3022 root       20   0  2236  1412  1268 S  0.0  0.1  0:02.31 /sbin/dhcpcd
 3412 root       20   0  1828   228   228 S  0.0  0.0  0:00.00 /bin/sh /etc/init.d/elasticsearch start
 3430 root       20   0 1226M  292M  4528 S  0.0 31.6  0:15.25 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3431 root       20   0 1226M  292M  4528 S  0.0 31.6  0:42.81 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3432 root       20   0 1226M  292M  4528 S  0.0 31.6  0:42.74 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3433 root       20   0 1226M  292M  4528 S  0.0 31.6  0:42.64 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3434 root       20   0 1226M  292M  4528 S  0.0 31.6  0:43.10 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3435 root       20   0 1226M  292M  4528 S  0.0 31.6  2:25.59 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3436 root       20   0 1226M  292M  4528 S  0.0 31.6  0:56.93 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3437 root       20   0 1226M  292M  4528 S  0.0 31.6  0:00.99 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3438 root       20   0 1226M  292M  4528 S  0.0 31.6  0:00.82 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3439 root       20   0 1226M  292M  4528 S  0.0 31.6  0:00.00 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3440 root       20   0 1226M  292M  4528 S  0.0 31.6  0:00.00 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3430 root       20   0 1226M  292M  4528 S  0.0 31.6  0:15.25 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3431 root       20   0 1226M  292M  4528 S  0.0 31.6  0:42.81 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3432 root       20   0 1226M  292M  4528 S  0.0 31.6  0:42.74 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3433 root       20   0 1226M  292M  4528 S  0.0 31.6  0:42.64 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3434 root       20   0 1226M  292M  4528 S  0.0 31.6  0:43.10 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3435 root       20   0 1226M  292M  4528 S  0.0 31.6  2:25.59 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3436 root       20   0 1226M  292M  4528 S  0.0 31.6  0:56.96 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3437 root       20   0 1226M  292M  4528 S  0.0 31.6  0:00.99 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3438 root       20   0 1226M  292M  4528 S  0.0 31.6  0:00.82 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3439 root       20   0 1226M  292M  4528 S  0.0 31.6  0:00.00 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3440 root       20   0 1226M  292M  4528 S  0.0 31.6  0:00.00 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3441 root       20   0 1226M  292M  4528 S  0.0 31.6  4:52.49 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3442 root       20   0 1226M  292M  4528 S  0.0 31.6  0:00.00 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3448 root       20   0 1226M  292M  4528 S  0.0 31.6  0:01.92 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3449 root       20   0 1226M  292M  4528 S  0.0 31.6  0:01.74 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3450 root       20   0 1226M  292M  4528 S  0.0 31.6  0:32.85 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3451 root       20   0 1226M  292M  4528 S  0.0 31.6  0:33.03 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3453 root       20   0 1226M  292M  4528 S  0.0 31.6  0:31.67 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3454 root       20   0 1226M  292M  4528 S  0.0 31.6  0:32.17 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3455 root       20   0 1226M  292M  4528 S  0.0 31.6  0:32.60 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3456 root       20   0 1226M  292M  4528 S  0.0 31.6  0:33.60 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3457 root       20   0 1226M  292M  4528 S  0.0 31.6  0:33.20 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3458 root       20   0 1226M  292M  4528 S  0.0 31.6  0:43.16 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3459 root       20   0 1226M  292M  4528 S  0.0 31.6  0:33.39 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3460 root       20   0 1226M  292M  4528 S  0.0 31.6  0:32.75 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3462 root       20   0 1226M  292M  4528 S  0.0 31.6  0:32.39 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3464 root       20   0 1226M  292M  4528 S  0.0 31.6  0:32.03 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3465 root       20   0 1226M  292M  4528 S  0.0 31.6  0:31.94 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3466 root       20   0 1226M  292M  4528 S  0.0 31.6  0:32.14 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3467 root       20   0 1226M  292M  4528 S  0.0 31.6  0:00.03 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3468 root       20   0 1226M  292M  4528 S  0.0 31.6  0:02.62 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3469 root       20   0 1226M  292M  4528 S  0.0 31.6  0:20.26 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3472 root       20   0 1226M  292M  4528 S  0.0 31.6  3:43.30 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3474 root       20   0 1226M  292M  4528 S  0.0 31.6  0:00.02 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3476 root       20   0 1226M  292M  4528 S  0.0 31.6  0:33.59 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3477 root       20   0 1226M  292M  4528 S  0.0 31.6  0:33.20 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3478 root       20   0 1226M  292M  4528 S  0.0 31.6  0:50.43 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3480 root       20   0 1226M  292M  4528 S  0.0 31.6  0:33.14 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3483 root       20   0 1226M  292M  4528 S  0.0 31.6  0:33.03 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3484 root       20   0 1226M  292M  4528 S  0.0 31.6  0:00.27 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3485 root       20   0 1226M  292M  4528 S  0.0 31.6  0:00.00 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
 3487 root       20   0 1226M  292M  4528 S  0.0 31.6  0:14.17 /usr/bin/java -Xms256m -Xmx1g -Xss256k -Djava.awt.headless=true -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=7
```


```
org.jruby.Main --1.9 /opt/logstash-1.5.3/lib/bootstrap/environment.rb logstash/runner.rb agent -f /etc/logstash/conf.d/logstash.conf -l /var/log/logstash/logs.txt
```
