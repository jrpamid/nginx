# NGINX
[Nginx Documentaion](https:/nginx.org/en/docs/)
>nginx [engine x] is an HTTP and reverse proxy server, a mail proxy server, and a generic TCP/UDP proxy server
Default Nginx Verion = 1.16.1
Current Stable Version = 1.22.1 (as of march 7,  2023)

Image naming convention used.
nginx-<os>:<nginx-version> for alpine and centos 
nginx-ubuntu:<ubuntu_codename>


</br>

## *Nginx On Alpine*
Dockerfile = build/alpine/Dockerfile
</br>
>This Container image definition file is an example of multi-stage builds. 
The Image is built, by compiling from the Nginx source with its dependencies over alpine as base os. 
The final image only contains the compiled executable and  required config files.
Nginx Version to be compiled and  built can be passed as a build arg.

Default Nginx version = 1.6.1
Default alpine version = 3.16.4
Current latest alpine = 3.17.2 (use latest tag for latest alpine)
>

</br>

| File | Location |
| ---- | -------- |
| Nginx executable | /opt/nginx/bin/nginx |
| Nginx Conf | /opt/nginx/conf/nginx.conf |
| Nginx Web Root | /opt/nginx/html |
| Nginx Logs | /opt/nginx/logs/{access.log, error.log} |

</br>

```` 	
# Image Usage
# ------------ #

# custom nginx version on custom alpine vrsion build via command line arguments
$sudo docker build --tag nginx-alpine:1.16.1 --build-arg nginx_ver=1.16.1 alpine_ver=3.16.4 .
$sudo docker build --tag nginx-alpine:1.22.1 --build-arg nginx_ver=1.22.1 alpine_ver=latest .

# Test and dump the configuration
$sudo docker run -itd nginx-alpine:1.22.1 -T

# Run a nginx container and test
$sudo docker run -itd --name my_nginx-test -p 8080:8080 nginx-alpine:1.16.1
$sudo docker exec my_nginx-test -T
$sudo docker logs my_nginx-test
$curl http:/localhost:8080 

# Run a nginx container and map custom nginx.conf file and static html pages
$ sudo docker run -itd --name my_wwww \
                       -p 8080:8080 \
					   -v /website/html:/opt/nginx/html \
					   -v /website/nginx.conf:/opt/nginx/conf/nginx.conf \
					   nginx-alpine:1.22.1

````

</br>

## *Nginx  on CentOS*

Dockerfile = build/centos/Dockerfile

Centos default version = 8
Default Nginx version = 1.6.1
Stable Nginx Version = 1.22.1 (as of March 7, 2023)

</br>

| File | Location |
| ---- | -------- |
| Nginx executable | /opt/nginx/bin/nginx |
| Nginx Conf | /opt/nginx/conf/nginx.conf |
| Nginx Web Root | /opt/nginx/html |
| Nginx Logs | /opt/nginx/logs/{access.log, error.log} |

</br>

```` 	
# Image Usage
# ------------ #

# nginx version = 1.22.1
$sudo docker build --tag nginx-centos:1.22.1 --build-arg nginx_ver=1.22.1 .

# Test and dump the configuration
$sudo docker run -itd nginx-centos:1.22.1 -T

# Run a nginx container and test
$sudo docker run -itd --name my_nginx-test -p 8080:8080 nginx-centos:1.22.1
$sudo docker exec my_nginx-test -T
$sudo docker logs my_nginx-test
$curl http:/localhost:8080 

# Run a nginx container and map custom nginx.conf file and static html pages
$ sudo docker run -itd --name my_wwww \
                       -p 8080:8080 \
					   -v /website/html:/opt/nginx/html \
					   -v /website/nginx.conf:/opt/nginx/conf/nginx.conf \
					   nginx-centos:1.22.1

````

</br>

## *Nginx on Ubuntu*
Dockerfile = build/ubuntu/Dockerfile
</br>

>This Container image definition file doesn't use multi-stage builds. 
The Image is built, by adding the nginx repo and installing from the repo using package manager.
Version depends on the repo.
>
</br>

| ubuntu version | ubuntu_codename  |
| -------------- | ---------------- |
|   22.10        | kinetic          |
|   22.04        | jammy            |
|   20.04        | focal            |
|   20.10        | groovy           |
|   21.04        | hirsute          |
|   19.10        | eoan             |
|   19.04        | disco            |


</br>

##### Default locations:-
| File | Location |
| ---- | -------- |
| Nginx executable | /opt/nginx/bin/nginx |
| Nginx Conf | /opt/nginx/conf/nginx.conf |
| Nginx Web Root | /opt/nginx/html |
| Nginx Logs | /opt/nginx/logs/{access.log, error.log} |

>

```` 	
# Image Usage
# ------------ #

# ubuntu 22.04 codename=jammy
$sudo docker build --tag nginx-ubuntu:jammy --build-arg ubuntu_codename=jammy .

# ubuntu 18.04 codename = bionic
$sudo docker build --tag nginx-ubuntu:bionic --build-arg ubuntu_codename=bionic .

#ubuntu 19.04 codename=disco
$sudo docker build --tag nginx-ubuntu:disco --build-arg ubuntu_codename=disco .

# Test and dump the configuration
$sudo docker run -itd nginx-ubuntu:jammy -T

# Run a nginx container and test
$sudo docker run -itd --name my_nginx-test -p 8080:8080 nginx-ubuntu:jammy
$sudo docker exec my_nginx-test -T
$sudo docker logs my_nginx-test
$curl http:/localhost:8080 

# Run a nginx container and map custom nginx.conf file and static html pages
$ sudo docker run -itd --name my_wwww \
                       -p 8080:8080 \
					   -v /website/html:/opt/nginx/html \
					   -v /website/nginx.conf:/opt/nginx/conf/nginx.conf \
					   nginx-ubuntu:jammy

````

</br>



### Full Nginx sample configuration file
[nginx.conf](https:/www.nginx.com/resources/wiki/start/topics/examples/full/)

