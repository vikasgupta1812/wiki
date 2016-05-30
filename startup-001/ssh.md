```
Vikas:~ Vikas$ ssh -i aws-startup.pem ubuntu@54.186.64.97 
uptime
```

```
 06:03:26 up 50 min,  0 users,  load average: 0.00, 0.01, 0.05
```

```
Vikas:~ Vikas$ ssh vikas@192.168.2.34 uptime
```
```
vikas@192.168.2.34's password: 
 01:03:42 up  2:36,  1 user,  load average: 0.02, 0.03, 0.05
```

```
Vikas:~ Vikas$ ssh vikas@192.168.2.34 w
```
```
 01:08:21 up  2:41,  1 user,  load average: 0.16, 0.05, 0.06
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
vikas    pts/13   192.168.2.4      22:39    2:21m  0.06s  0.06s -bash
```

```
Vikas:~ Vikas$ ssh -i aws-startup.pem ubuntu@54.186.64.97 w
```

```
 06:08:33 up 55 min,  0 users,  load average: 0.00, 0.01, 0.05
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
```