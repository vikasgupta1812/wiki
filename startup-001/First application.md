### Install first node application on Heroku
A barebones Node.js app using the Express framework. http://node-js-sample.herokuapp.com/

```
$ chmod 400 aws-startup.pem 
$ ssh -i aws-startup.pem ubuntu@52.11.144.15
```

``` 
ubuntu@ip-172-30-0-181:~$ sudo apt-get install -y git-core
ubuntu@ip-172-30-0-181:~$ wget -qO- https://toolbelt.heroku.com/
install-ubuntu.sh | sh
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
...
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
