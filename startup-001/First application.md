```
Vikas:~ Vikas$ chmod 400 aws-startup.pem 
Vikas:~ Vikas$ ssh -i aws-startup.pem ubuntu@52.11.144.15
```

```
The authenticity of host '52.11.144.15 (52.11.144.15)' can't be established.
RSA key fingerprint is fb:53:bb:cf:1e:86:e3:b6:14:d2:7a:f5:f2:45:3a:61.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '52.11.144.15' (RSA) to the list of known hosts.
Welcome to Ubuntu 14.04.2 LTS (GNU/Linux 3.13.0-48-generic x86_64)

 * Documentation:  https://help.ubuntu.com/

  System information as of Wed Apr 29 04:13:09 UTC 2015

  System load: 0.16             Memory usage: 5%   Processes:       83
  Usage of /:  9.8% of 7.74GB   Swap usage:   0%   Users logged in: 0

  Graph this data and manage this system at:
    https://landscape.canonical.com/

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.



The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.
```

``` 
ubuntu@ip-172-30-0-181:~$ sudo apt-get install -y git-core
```
```
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following extra packages will be installed:
  git git-man liberror-perl
Suggested packages:
  git-daemon-run git-daemon-sysvinit git-doc git-el git-email git-gui gitk
  gitweb git-arch git-bzr git-cvs git-mediawiki git-svn
The following NEW packages will be installed:
  git git-core git-man liberror-perl
0 upgraded, 4 newly installed, 0 to remove and 0 not upgraded.
Need to get 3,347 kB of archives.
After this operation, 21.6 MB of additional disk space will be used.
Get:1 http://us-west-2.ec2.archive.ubuntu.com/ubuntu/ trusty/main liberror-perl all 0.17-1.1 [21.1 kB]
Get:2 http://us-west-2.ec2.archive.ubuntu.com/ubuntu/ trusty-updates/main git-man all 1:1.9.1-1ubuntu0.1 [698 kB]
Get:3 http://us-west-2.ec2.archive.ubuntu.com/ubuntu/ trusty-updates/main git amd64 1:1.9.1-1ubuntu0.1 [2,627 kB]
Get:4 http://us-west-2.ec2.archive.ubuntu.com/ubuntu/ trusty-updates/main git-core all 1:1.9.1-1ubuntu0.1 [1,458 B]
Fetched 3,347 kB in 0s (33.1 MB/s)
Selecting previously unselected package liberror-perl.
(Reading database ... 51120 files and directories currently installed.)
Preparing to unpack .../liberror-perl_0.17-1.1_all.deb ...
Unpacking liberror-perl (0.17-1.1) ...
Selecting previously unselected package git-man.
Preparing to unpack .../git-man_1%3a1.9.1-1ubuntu0.1_all.deb ...
Unpacking git-man (1:1.9.1-1ubuntu0.1) ...
Selecting previously unselected package git.
Preparing to unpack .../git_1%3a1.9.1-1ubuntu0.1_amd64.deb ...
Unpacking git (1:1.9.1-1ubuntu0.1) ...
Selecting previously unselected package git-core.
Preparing to unpack .../git-core_1%3a1.9.1-1ubuntu0.1_all.deb ...
Unpacking git-core (1:1.9.1-1ubuntu0.1) ...
Processing triggers for man-db (2.6.7.1-1ubuntu1) ...
Setting up liberror-perl (0.17-1.1) ...
Setting up git-man (1:1.9.1-1ubuntu0.1) ...
Setting up git (1:1.9.1-1ubuntu0.1) ...
Setting up git-core (1:1.9.1-1ubuntu0.1) ...
```


```
ubuntu@ip-172-30-0-181:~$ wget -qO- https://toolbelt.heroku.com/install-ubuntu.sh | sh
This script requires superuser access to install apt packages.
You will be prompted for your password by sudo.
--2015-04-29 04:23:34--  https://toolbelt.heroku.com/apt/release.key
Resolving toolbelt.heroku.com (toolbelt.heroku.com)... 23.23.230.173, 54.235.221.131, 23.23.157.116
Connecting to toolbelt.heroku.com (toolbelt.heroku.com)|23.23.230.173|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 1737 (1.7K) [application/octet-stream]
Saving to: ‘STDOUT’

100%[=====================================================>] 1,737       --.-K/s   in 0s      

2015-04-29 04:23:35 (448 MB/s) - written to stdout [1737/1737]

OK
Ign http://us-west-2.ec2.archive.ubuntu.com trusty InRelease
Ign http://us-west-2.ec2.archive.ubuntu.com trusty-updates InRelease           
Hit http://us-west-2.ec2.archive.ubuntu.com trusty Release.gpg                 
Get:1 http://us-west-2.ec2.archive.ubuntu.com trusty-updates Release.gpg [933 B]
Hit http://us-west-2.ec2.archive.ubuntu.com trusty Release                     
Get:2 http://us-west-2.ec2.archive.ubuntu.com trusty-updates Release [63.5 kB] 
Get:3 http://us-west-2.ec2.archive.ubuntu.com trusty/main Sources [1,064 kB]   
Get:4 http://us-west-2.ec2.archive.ubuntu.com trusty/universe Sources [6,399 kB]
Ign http://security.ubuntu.com trusty-security InRelease                       
Get:5 http://security.ubuntu.com trusty-security Release.gpg [933 B]           
Hit http://us-west-2.ec2.archive.ubuntu.com trusty/main amd64 Packages         
Ign http://toolbelt.heroku.com ./ InRelease                                    
Hit http://us-west-2.ec2.archive.ubuntu.com trusty/universe amd64 Packages     
Hit http://us-west-2.ec2.archive.ubuntu.com trusty/main Translation-en         
Hit http://us-west-2.ec2.archive.ubuntu.com trusty/universe Translation-en     
Get:6 http://us-west-2.ec2.archive.ubuntu.com trusty-updates/main Sources [195 kB]
Get:7 http://us-west-2.ec2.archive.ubuntu.com trusty-updates/universe Sources [113 kB]
Get:8 http://us-west-2.ec2.archive.ubuntu.com trusty-updates/main amd64 Packages [503 kB]
Get:9 http://us-west-2.ec2.archive.ubuntu.com trusty-updates/universe amd64 Packages [273 kB]
Get:10 http://security.ubuntu.com trusty-security Release [63.5 kB]            
Get:11 http://us-west-2.ec2.archive.ubuntu.com trusty-updates/main Translation-en [238 kB]
Get:12 http://us-west-2.ec2.archive.ubuntu.com trusty-updates/universe Translation-en [142 kB]
Ign http://us-west-2.ec2.archive.ubuntu.com trusty/main Translation-en_US      
Ign http://us-west-2.ec2.archive.ubuntu.com trusty/universe Translation-en_US  
Get:13 http://toolbelt.heroku.com ./ Release.gpg [473 B]                       
Get:14 http://security.ubuntu.com trusty-security/main Sources [79.4 kB]       
Get:15 http://security.ubuntu.com trusty-security/universe Sources [20.9 kB]   
Get:16 http://toolbelt.heroku.com ./ Release [1,609 B]                         
Get:17 http://security.ubuntu.com trusty-security/main amd64 Packages [260 kB] 
Get:18 http://toolbelt.heroku.com ./ Packages [1,064 B]                        
Get:19 http://security.ubuntu.com trusty-security/universe amd64 Packages [99.6 kB]
Get:20 http://security.ubuntu.com trusty-security/main Translation-en [132 kB] 
Get:21 http://security.ubuntu.com trusty-security/universe Translation-en [55.8 kB]
Ign http://toolbelt.heroku.com ./ Translation-en_US                            
Ign http://toolbelt.heroku.com ./ Translation-en                         
Fetched 9,707 kB in 3s (2,739 kB/s)                
Reading package lists... Done
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following extra packages will be installed:
  foreman heroku libjs-jquery libruby1.9.1 libruby2.0 ruby ruby1.9.1 ruby2.0
  rubygems-integration
Suggested packages:
  javascript-common ri ruby-dev ruby1.9.1-examples ri1.9.1 graphviz
  ruby1.9.1-dev ruby-switch bundler
The following NEW packages will be installed:
  foreman heroku heroku-toolbelt libjs-jquery libruby1.9.1 libruby2.0 ruby
  ruby1.9.1 ruby2.0 rubygems-integration
0 upgraded, 10 newly installed, 0 to remove and 35 not upgraded.
Need to get 7,381 kB of archives.
After this operation, 26.7 MB of additional disk space will be used.
Get:1 http://us-west-2.ec2.archive.ubuntu.com/ubuntu/ trusty/main libjs-jquery all 1.7.2+dfsg-2ubuntu1 [78.8 kB]
Get:2 http://us-west-2.ec2.archive.ubuntu.com/ubuntu/ trusty-updates/main libruby2.0 amd64 2.0.0.484-1ubuntu2.2 [2,805 kB]
Get:3 http://us-west-2.ec2.archive.ubuntu.com/ubuntu/ trusty-updates/main libruby1.9.1 amd64 1.9.3.484-2ubuntu1.2 [2,645 kB]
Get:4 http://us-west-2.ec2.archive.ubuntu.com/ubuntu/ trusty-updates/main ruby1.9.1 amd64 1.9.3.484-2ubuntu1.2 [35.6 kB]
Get:5 http://us-west-2.ec2.archive.ubuntu.com/ubuntu/ trusty/main ruby all 1:1.9.3.4 [5,334 B]
Get:6 http://us-west-2.ec2.archive.ubuntu.com/ubuntu/ trusty/main rubygems-integration all 1.5 [5,340 B]
Get:7 http://us-west-2.ec2.archive.ubuntu.com/ubuntu/ trusty-updates/main ruby2.0 amd64 2.0.0.484-1ubuntu2.2 [66.5 kB]
Get:8 http://toolbelt.heroku.com/ubuntu/ ./ foreman 0.75.0 [114 kB]       
Get:9 http://toolbelt.heroku.com/ubuntu/ ./ heroku 3.32.0 [1,624 kB]
Get:10 http://toolbelt.heroku.com/ubuntu/ ./ heroku-toolbelt 3.32.0 [668 B]    
Fetched 7,381 kB in 14s (518 kB/s)                                             
Selecting previously unselected package libjs-jquery.
(Reading database ... 51869 files and directories currently installed.)
Preparing to unpack .../libjs-jquery_1.7.2+dfsg-2ubuntu1_all.deb ...
Unpacking libjs-jquery (1.7.2+dfsg-2ubuntu1) ...
Selecting previously unselected package libruby2.0:amd64.
Preparing to unpack .../libruby2.0_2.0.0.484-1ubuntu2.2_amd64.deb ...
Unpacking libruby2.0:amd64 (2.0.0.484-1ubuntu2.2) ...
Selecting previously unselected package libruby1.9.1.
Preparing to unpack .../libruby1.9.1_1.9.3.484-2ubuntu1.2_amd64.deb ...
Unpacking libruby1.9.1 (1.9.3.484-2ubuntu1.2) ...
Selecting previously unselected package ruby1.9.1.
Preparing to unpack .../ruby1.9.1_1.9.3.484-2ubuntu1.2_amd64.deb ...
Unpacking ruby1.9.1 (1.9.3.484-2ubuntu1.2) ...
Selecting previously unselected package ruby.
Preparing to unpack .../ruby_1%3a1.9.3.4_all.deb ...
Unpacking ruby (1:1.9.3.4) ...
Selecting previously unselected package rubygems-integration.
Preparing to unpack .../rubygems-integration_1.5_all.deb ...
Unpacking rubygems-integration (1.5) ...
Selecting previously unselected package ruby2.0.
Preparing to unpack .../ruby2.0_2.0.0.484-1ubuntu2.2_amd64.deb ...
Unpacking ruby2.0 (2.0.0.484-1ubuntu2.2) ...
Selecting previously unselected package foreman.
Preparing to unpack .../foreman_0.75.0_all.deb ...
Unpacking foreman (0.75.0) ...
Selecting previously unselected package heroku.
Preparing to unpack .../archives/heroku_3.32.0_all.deb ...
Unpacking heroku (3.32.0) ...
Selecting previously unselected package heroku-toolbelt.
Preparing to unpack .../heroku-toolbelt_3.32.0_all.deb ...
Unpacking heroku-toolbelt (3.32.0) ...
Processing triggers for man-db (2.6.7.1-1ubuntu1) ...
Setting up libjs-jquery (1.7.2+dfsg-2ubuntu1) ...
Setting up ruby (1:1.9.3.4) ...
Setting up libruby1.9.1 (1.9.3.484-2ubuntu1.2) ...
Setting up rubygems-integration (1.5) ...
Setting up ruby2.0 (2.0.0.484-1ubuntu2.2) ...
Setting up foreman (0.75.0) ...
Setting up heroku (3.32.0) ...
Setting up heroku-toolbelt (3.32.0) ...
Setting up ruby1.9.1 (1.9.3.484-2ubuntu1.2) ...
Setting up libruby2.0:amd64 (2.0.0.484-1ubuntu2.2) ...
Processing triggers for libc-bin (2.19-0ubuntu6.6) ...
```

```
ubuntu@ip-172-30-0-181:~$ which git
```
```
/usr/bin/git
```
```
ubuntu@ip-172-30-0-181:~$ which heroku
```
```
/usr/bin/heroku
```

```
ubuntu@ip-172-30-0-181:~$ heroku login
```
```
Enter your Heroku credentials.
Email: vikas.ymca@gmail.com
Password (typing will be hidden): 
Authentication successful.
```
```
ubuntu@ip-172-30-0-181:~$ ssh-keygen -t rsa
```

```
Generating public/private rsa key pair.
Enter file in which to save the key (/home/ubuntu/.ssh/id_rsa):       
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/ubuntu/.ssh/id_rsa.
Your public key has been saved in /home/ubuntu/.ssh/id_rsa.pub.
The key fingerprint is:
b2:ad:f4:4e:ca:87:7a:a8:82:6c:0b:c5:3b:56:57:41 ubuntu@ip-172-30-0-181
The key's randomart image is:
+--[ RSA 2048]----+
|       .E        |
|         .       |
|        .        |
| .     .         |
|  o . o S        |
| . o . +         |
|+ +  .o.o        |
|++ ..oo=.        |
|.oo..o+oo        |
+-----------------+
```

```
ubuntu@ip-172-30-0-181:~$ cd /home/ubuntu/.ssh/
ubuntu@ip-172-30-0-181:~/.ssh$ ls -la
```
```
total 20
drwx------ 2 ubuntu ubuntu 4096 Apr 29 04:25 .
drwxr-xr-x 5 ubuntu ubuntu 4096 Apr 29 04:24 ..
-rw------- 1 ubuntu ubuntu  393 Apr 29 04:13 authorized_keys
-rw------- 1 ubuntu ubuntu 1679 Apr 29 04:25 id_rsa
-rw-r--r-- 1 ubuntu ubuntu  404 Apr 29 04:25 id_rsa.pub
```
```
ubuntu@ip-172-30-0-181:~/.ssh$ cat id_rsa
```

```
-----BEGIN RSA PRIVATE KEY-----
MIIEpAIBAAKCAQEAqNG8UsffC466y8tPGar/hsM/+xMfNnGHwjDg/r9XZ5Y8oppN
mxn1NpPJRoGGLan0pAmyEfrFMcUOn6Z2fvr7ko17lypAlP3rm607NpZc/oQzvkfg
6WADpCeIqbYQpnhFWHri8t73WnIsaXJZ2BVAYugNjV65UZwplmxzGK9RKyrEjgIJ
iAKxU8qxnHIHo5tCbZDCW6kevUyDywxJgypRsFq0E9hzZgcy2F5mcwZh/LAsQ78e
TPCdONbsElqAncSpETg+1ngOaMJV5QhDvNZDcFXVmEDP7Nv0xes0zGcVA0K1zwmI
wVfTaG3/4Ae4aFyqG2sgl59nsbJwx6xQnC1wwQIDAQABAoIBAQCdf37o5BuNFs3i
z3yuf8ABJCuOvBpEmsqDO0LNqAmNVLahJL/+UctZ7aq8Ip7h/0uDtp/w8joC4stv
2sd2VAVchq6lKwAxgGvNQ2KY3NNJiGEVxs1oLPF4toFjg74o8NARaiRNXgL62MXi
YpK7a6g0HjZ2i8btAnoyIl+Gyhk54RoPFkXw3YGAecKIhYWWoRxm3ECLIldOvgsq
SKDWBtsm8Ev8Aan+9ukZGO51AEruimLEkmLXQOkhVs2vBrUuncbgvsS0TiYSS0zI
PwuahVSKOWzd7Z9j4FPCoJjCGNHErI8CEVn5tthZH/HfSDFUpKlvoRa38yb3y2Rc
2Pmw9qshAoGBANw+X5TTTX/PgTASiifmtdTxQRTQq5KGCyNtY4qGzwR912vb2hVa
jWfDWgKx/edfHElxEw+5XxTUYWlCBHvnR+/Hug1kDgN7WO17qungiofUnZVmAdF2
AXMtQaCQSjFoAzQB+qfsSYSuXCCCiNme9l7JsTk+DMASSrOq0rg+LLO1AoGBAMQ6
GGx8fMbOvjWi047tuurNRrY+/BjgfLs67upAUAIvMM/EPfNkPoOLHWVpkGe6kjbR
aUKp9iLD30DAn/WiWiqmw3KxNxxaCo9cWAjAlSBF/MUtYxr2gdgph74Tl9Pdk6ZZ
xb+VTTQ5MYnjHlDpackrjBJWX+NXetjwNhMnmIhdAoGAKIn2j+9A4ZixP8b51RRb
PcHWZ91s50BzBmdZHiNoMXx0TW8fOjT7uDC1/a8DfDX+f+onRwqo3K2m7HfxWVkd
3Z3WuiZDihKHMNdFg10IQq44/0nSZdqhs7CN1t8YOPXbORRwLb6JXbm2TWmZhO0E
jjfzvgSU1jnHtEBqHu2azs0CgYEApUnacOebo0ta5Ys2cVrG7CnlunXrnHjcGEpY
HXH28yAVGa3QEUkLb3qrVFVLklSR/SMAa2sHLdmYIM8g7qPHF85JLD8ikPs3kfLT
JOwzsW/Cr8S/imLClPbGpNGUPp6SVLmh3PNCiQ70L5XkX3t95DqOTpP7SWDS1hHh
OtlLnvECgYB+6SXLl9Pdi21d06JtyN7KAYpi7XIOfYqCMhNLwfGpowZehXFx5PK9
KZauMMNv4ZOFeGUHyf+VY/xTrJ2dC4PkbUttYW44y6pFk0SwkwEoXiv74Ok4bKfC
bFIEO4P2JtN1FXc6dVs2O3Nwu1Sa1Dbzs+F41HbpzPQS9MUYA6HJAw==
-----END RSA PRIVATE KEY-----
```

```
ubuntu@ip-172-30-0-181:~$ heroku keys:add
```

```
Found an SSH public key at /home/ubuntu/.ssh/id_rsa.pub
Would you like to upload it to Heroku? [Yn] Y
Uploading SSH public key /home/ubuntu/.ssh/id_rsa.pub... done
```

```
ubuntu@ip-172-30-0-181:~$ git clone https://github.com/heroku/node-js-sample.git
```

```
Cloning into 'node-js-sample'...
remote: Counting objects: 388, done.
remote: Total 388 (delta 0), reused 0 (delta 0), pack-reused 388
Receiving objects: 100% (388/388), 213.57 KiB | 0 bytes/s, done.
Resolving deltas: 100% (46/46), done.
Checking connectivity... done.
```

```
ubuntu@ip-172-30-0-181:~$ cd node-js-sample/
ubuntu@ip-172-30-0-181:~/node-js-sample$ ls -la
```
```
total 44
drwxrwxr-x 4 ubuntu ubuntu 4096 Apr 29 04:26 .
drwxr-xr-x 6 ubuntu ubuntu 4096 Apr 29 04:26 ..
-rw-rw-r-- 1 ubuntu ubuntu  254 Apr 29 04:26 app.json
drwxrwxr-x 8 ubuntu ubuntu 4096 Apr 29 04:26 .git
-rw-rw-r-- 1 ubuntu ubuntu   13 Apr 29 04:26 .gitignore
-rw-rw-r-- 1 ubuntu ubuntu  339 Apr 29 04:26 index.js
-rw-rw-r-- 1 ubuntu ubuntu 8048 Apr 29 04:26 npm-shrinkwrap.json
-rw-rw-r-- 1 ubuntu ubuntu  575 Apr 29 04:26 package.json
drwxrwxr-x 2 ubuntu ubuntu 4096 Apr 29 04:26 public
-rw-rw-r-- 1 ubuntu ubuntu 1240 Apr 29 04:26 README.md
```

```
ubuntu@ip-172-30-0-181:~/node-js-sample$ heroku create
```
```
Creating mysterious-falls-7309... done, stack is cedar-14
https://mysterious-falls-7309.herokuapp.com/ | https://git.heroku.com/mysterious-falls-7309.git
Git remote heroku added
```

```
ubuntu@ip-172-30-0-181:~/node-js-sample$ git push heroku master
```
```
Counting objects: 388, done.
Compressing objects: 100% (313/313), done.
Writing objects: 100% (388/388), 213.57 KiB | 0 bytes/s, done.
Total 388 (delta 46), reused 388 (delta 46)
remote: Compressing source files... done.
remote: Building source:
remote: 
remote: -----> Node.js app detected
remote: 
remote: -----> Reading application state
remote:        package.json...
remote:        build directory...
remote:        cache directory...
remote:        environment variables...
remote: 
remote:        Node engine:         0.12.x
remote:        Npm engine:          unspecified
remote:        Start mechanism:     npm start
remote:        node_modules source: npm-shrinkwrap.json
remote:        node_modules cached: false
remote: 
remote:        NPM_CONFIG_PRODUCTION=true
remote:        NODE_MODULES_CACHE=true
remote: 
remote: -----> Installing binaries
remote:        Resolving node version 0.12.x via semver.io...
remote:        Downloading and installing node 0.12.2...
remote:        Using default npm version: 2.7.4
remote: 
remote: -----> Building dependencies
remote:        Installing node modules
remote:        express@4.12.3 node_modules/express
remote:        ├── merge-descriptors@1.0.0
remote:        ├── cookie-signature@1.0.6
remote:        ├── utils-merge@1.0.0
remote:        ├── methods@1.1.1
remote:        ├── cookie@0.1.2
remote:        ├── fresh@0.2.4
remote:        ├── escape-html@1.0.1
remote:        ├── range-parser@1.0.2
remote:        ├── content-type@1.0.1
remote:        ├── finalhandler@0.3.4
remote:        ├── vary@1.0.0
remote:        ├── parseurl@1.3.0
remote:        ├── serve-static@1.9.2
remote:        ├── content-disposition@0.5.0
remote:        ├── path-to-regexp@0.1.3
remote:        ├── depd@1.0.1
remote:        ├── qs@2.4.1
remote:        ├── on-finished@2.2.0 (ee-first@1.1.0)
remote:        ├── debug@2.1.3 (ms@0.7.0)
remote:        ├── proxy-addr@1.0.7 (forwarded@0.1.0, ipaddr.js@0.1.9)
remote:        ├── send@0.12.2 (destroy@1.0.3, ms@0.7.0, mime@1.3.4)
remote:        ├── etag@1.5.1 (crc@3.2.1)
remote:        ├── accepts@1.2.5 (negotiator@0.5.1, mime-types@2.0.10)
remote:        └── type-is@1.6.1 (media-typer@0.3.0, mime-types@2.0.10)
remote: 
remote: -----> Checking startup method
remote:        No Procfile; Adding 'web: npm start' to new Procfile
remote: 
remote: -----> Finalizing build
remote:        Creating runtime environment
remote:        Exporting binary paths
remote:        Cleaning npm artifacts
remote:        Cleaning previous cache
remote:        Caching results for future builds
remote: 
remote: -----> Build succeeded!
remote: 
remote:        node-js-sample@0.2.0 /tmp/build_cccc5e8e684d6b539d3057b06fe3bd8f
remote:        └── express@4.12.3
remote:        
remote: -----> Discovering process types
remote:        Procfile declares types -> web
remote: 
remote: -----> Compressing... done, 9.3MB
remote: -----> Launching... done, v3
remote:        https://mysterious-falls-7309.herokuapp.com/ deployed to Heroku
remote: 
remote: Verifying deploy... done.
To https://git.heroku.com/mysterious-falls-7309.git
 * [new branch]      master -> master
```
