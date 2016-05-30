# Installation Commands from Docker

```
root@docker-dev:~# docker search rstudio 
root@docker-dev:~# sudo docker run -d -P rocker/rstudio 
root@docker-dev:~# ifconfig eth0
```


#### r-base installation image 

```
root@dev:~/r-base# docker search -s 5 r-base 
root@dev:~/r-base# docker pull rocker/r-base
```

#### r-devel installed docker Images

```
root@dev:~/r-base# docker search r-devel 
root@dev:~/r-base# docker pull rocker/r-devel 
```

#### RStudio docker image installation

```
root@dev:~/r-base# docker search rstudio 
root@dev:~/r-base# docker pull rocker/rstudio 
```

#### 3.4. hadleyverse dokeo image installation

```
root@dev:~/r-base# docker search hadleyverse 
root@dev:~/r-base# docker pull rocker/hadleyverse 
```

`docker run` doker run the image through a command `-d` option means that the flag is run as a `daemon container`. `rocker/shiny` the port number for the container Offset `3838:3838` when the matching IP address to connect to the web server on Shiny 

The connection to the client interface. General `rocker/rstudio` ttuiwooryeomyeon the container `8787` using the port and `rocker/hadleyverse` If you launch a container 7777 uses the port.


```
#delete process with the command. 
docker kill 
```






How to use RStudio without having R installed.

Install:
```
docker pull rocker/rstudio
```
Run:

```
docker run --name=rstudio -d -p 127.0.0.1:8787:8787 rocker/rstudio
```

WebFrame:

```
firefox http://127.0.0.1:8787
```

adminuser/password=rstudio/rstudio

Uninstall:

```
docker stop rstudio
docker rm rstudio
docker rmi rocker/rstudio﻿
```



```
docker run 
			-dp 8787:8787 
			-v /c/Users/shawn graham/docker:/home/rstudio/ 
			-e ROOT=TRUE rocker/rstudio
```


-d, --detach=false         Run container in background and print container ID
-p, --publish=[]           Publish a container's port(s) to the host
-v, --volume=[]            Bind mount a volume
-e, --env=[]               Set environment variables



http://keeganhin.es/blog/docker.html

```
docker pull rocker/rstudio
docker run -d -p 8787:8787 rocker/rstudio
```
This deploys Rstudio server on port 8787 and with the default username/password of rstudio/rstudio and returns a lengthy ID key for the container we created. 

Since we're deploying Rstudio, we'll probably want to add some extra security with our own username and password.

```
docker run -d -p 8787:8787 -e USERNAME=someusername -e PASSWORD=somepassword rocker/rstudio 
```
And there we have, rather effortlessly, set up Rstudio Server so we can do our number crunching from a web brower.




```
docker run -dp 80:80 -it rocker/hadleyverse /bin/bash
docker run -dp 80:80 -it kaggle/rstats /bin/bash
docker run -dp 80:80 -it revolutionanalytics/rro-ubuntu /bin/bash

docker run -dp 443:443 -p 80:80 -it revolutionanalytics/rro-ubuntu /bin/bash
docker run -dp 80:80 -p 443:443 -it kaggle/rstats /bin/bash
```
```
source('http://r.research.att.com/benchmarks/R-benchmark-25.R')
```



```
/bin/sh /usr/lib/R/bin/BATCH train_gbm_cross_dept_v1.R

/usr/lib/R/bin/exec/R --slave --no-restore -e parallel:::.slaveRSOCK() --args MASTER=localhost PORT=11385 OUT=/dev/null TIMEOUT=2592000 METHODS=TRUE XDR=TRUE

/usr/lib/R/bin/exec/R -f train_gbm_cross_dept_v1.R --restore --save --no-readline

```
-----
How to run a command on an already existing docker container?

Your container will exit as the command you gave it will end. Use the following options to keep it live:

`-i` Keep STDIN open even if not attached.
`-t` Allocate a pseudo-TTY.
So your new run command is:

`docker run -it -d shykes/pybuilder bin/bash`
If you would like to attach to an already running container:

`docker exec -it CONTAINER_ID /bin/bash`
In these examples `/bin/bash` is used as the command.


#### Create docker script for Revolution R 

https://github.com/Kaggle/docker-rcran/blob/master/Dockerfile


```
$azure vm image list | grep Docker
data:    Name                                                                                                                              Category  OS     
data:    --------------------------------------------------------------------------------------------------------------------------------  --------  -------
data:    b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-15.04-Snappy-core-SSH-Docker-amd64-edge-201507081917-119-en-us-30GB                      Public    Linux
```