## Demo 1 - Running a Windows Container ##

```
docker

docker run -it mcr.microsoft.com/windows/servercore:1809 cmd

-- in the container
dir
md demo
cd demo
copy con myfile.txt
Hello this is in my docker container
ctrl+c

dir
-- now you see the text file

-- exit the container
ctrl+p ctrl+q

--on the host
docker ps
-- you now see a running contianer with a name that is generated.

on the c:\ run command 
dir
you don't see the demo folder

docker stop name-of-the-container
docker ps
docker ps -a

docker run -it mcr.microsoft.com/windows/servercore:1809 cmd
--in the container
dir
-- no more demo folder
```

```
docker ps
docker ps -a 
-- you now see two images that have exited
docker search microsoft
--you see all public images microsoft created as base images

docker run -ti mcr.microsoft.com/windows/servercore:1809
CTRL PQ
docker attach <name of container>
docker exec <name of container> cmd
docker kill <name of container>
docker inspect <name of container>
docker tag <name of container> <tagname>
docker pull microsoft/aspnet
docker images 
```

Explore some options with running with a name --name as terminal, as background process etc..

## Demo 2 - Running, Kernel Sharing, Isolation ##

https://docs.microsoft.com/en-us/virtualization/windowscontainers/manage-containers/hyperv-container

```
docker run -d --isolation process mcr.microsoft.com/windows/servercore:1809 ping localhost -t

docker top

docker run --isolation hyperv -d mmcr.microsoft.com/windows/servercore:1809 ping localhost -t

docker top

get-process -Name ping
get-process -Name vmwp

```

## Demo 3 - Build an Image ##
```
docker run -it -name mycontainer mcr.microsoft.com/windows/servercore:1809 cmd
md builddemo
cd buidldemo
copy con myfile.txt
this is a text file
ctrl+c
exit

docker ps -a
docker commit -t mycontainerimage mycontainer 
docker images 
docker run -it -name mycontainer mcr.microsoft.com/windows/servercore:1809 cmd
dir
exit

docker run -it mycontainerimage cmd
dir 
you see the demo folder again

```
FROM mcr.microsoft.com/windows/servercore:1809
WORKDIR builddemo
RUN echo "hello world" >> myfile.txt
```

docker build -t vslive/demo .


```
## Demo 4 - Push an Image ##

Show Docker Hub
Show ACR

Create a Azure Container registry

```
docker tag mycontainer myacr/mycontainer
docker push myacr/mycontainer

docker search myacr
```

## Demo 5 - Host IIS and browse to website ##

docker search microsoft
docker pull microsoft/iis

docker run -d -p 80:80 microsoft/iis
docker inspect <containerID>

start IE, browse to IP adress of container

docker ps
docker images


## Demo 6 - Shared Volumes ##
docker run -ti -v %cd%:c:\data\ mcr.microsoft.com/windows/servercore:1809


https://docs.microsoft.com/en-us/virtualization/windowscontainers/about/