# Nginx
[Nginx Documentaion](https://nginx.org/en/docs/)
>nginx [engine x] is an HTTP and reverse proxy server, a mail proxy server, and a generic TCP/UDP proxy server
>

## Alpine Linux as Base Image
Dockerfile = nginx/build/alpine/Dockerfile

>This Container image definition file is an example of multi-stage builds. 
The Image is built, by compiling from the Nginx source with its dependencies over alpine as base os. 
The final image only contains the compiled executable and  required config files.
Nginx Version to be compiled and  built can be passed as a build arg. Default version = 1.16.1
>
| File | Location |
| ---- | -------- |
| Nginx executable | /opt/nginx/bin/nginx |
| Nginx Conf | /opt/nginx/conf/nginx.conf |
| Nginx Web Root | /opt/nginx/html |
| Nginx Logs | /opt/nginx/logs/{access.log, error.log} |


```` 	
# Image Usage
# ------------ #

# nginx on alpine  build 
$sudo docker build --tag nginx_alpine:1.16.1 --build-arg nginx_ver=1.16.1 alpine_ver=3.10.3 .
$sudo docker build --tag nginx_alpine:1.17.1 --build-arg nginx_ver=1.17.1 alpine_ver=latest .

# Test and dump the configuration
$sudo docker run -itd nginx_alpine:1.16.1 -T

# Run a nginx container and test
$sudo docker run -itd --name my_nginx_test -p 8080:8080 nginx_alpine:1.16.1
$sudo docker exec my_nginx_test -T
$sudo docker logs my_nginx_test
$curl http://localhost:8080 

# Run a nginx container and map custom nginx.conf file and static html pages
$ sudo docker run -itd --name my_wwww \
                       -p 8080:8080 \
					   -v /website/html:/usr/share/nginx/html \
					   -v /website/nginx.conf:/etc/nginx/nginx.conf \
					   nginx_alpine:1.16.1

````


## CentOS 7 as Base Image
Dockerfile = nginx/build/centos/Dockerfile

>This Container image definition file is an example of multi-stage builds. 
The Image is built, by compiling from the Nginx source with its dependencies. 
The final image only contains the compiled executable and  required config files.
Nginx Version to be compiled and  built can be passed as a build arg. Default version = 1.16.1
>
| File | Location |
| ---- | -------- |
| Nginx executable | /opt/nginx/bin/nginx |
| Nginx Conf | /opt/nginx/conf/nginx.conf |
| Nginx Web Root | /opt/nginx/html |
| Nginx Logs | /opt/nginx/logs/{access.log, error.log} |


```` 	
# Image Usage
# ------------ #

#nginx version = 1.17.1
$sudo docker build --tag nginx_centos:1.17.1 --build-arg nginx_ver=1.17.1 .

# Test and dump the configuration
$sudo docker run -itd nginx_centos:1.17.1 -T

# Run a nginx container and test
$sudo docker run -itd --name my_nginx_test -p 8080:8080 nginx_centos:1.17.1
$sudo docker exec my_nginx_test -T
$sudo docker logs my_nginx_test
$curl http://localhost:8080 

# Run a nginx container and map custom nginx.conf file and static html pages
$ sudo docker run -itd --name my_wwww \
                       -p 8080:8080 \
					   -v /website/html:/usr/share/nginx/html \
					   -v /website/nginx.conf:/etc/nginx/nginx.conf \
					   nginx_centos:1.17.1

````


## Ubuntu as Base Image
Dockerfile = nginx/build/ubuntu/Dockerfile

>This Container image is build using the Nginx.org pre built stable packages from the Nginx repo.
The base ubuntu image to be used can be provided as a build argument.

##### Default locations:-
| File | Location |
| ---- | -------- |
| Nginx executable | /usr/sbin/nginx/nginx |
| Nginx Conf | /etc/nginx/nginx.conf |
| Nginx Web Root | /usr/share/nginx/html |
| Nginx Logs | /var/log/nginx/{access.log, error.log} |

>

```` 	
# Image Usage
# ------------ #

#ubuntu 16.04 codename=xenial
$sudo docker build --tag nginx_ubuntu:xenial --build-arg ubuntu_codename=xenial .

#ubuntu 18.04 codename = bionic
$sudo docker build --tag nginx_ubuntu:bionic --build-arg ubuntu_codename=bionic .
#ubuntu 19.04 codename=disco
$sudo docker build --tag nginx_ubuntu:disco --build-arg ubuntu_codename=disco .

# Test and dump the configuration
$sudo docker run -itd nginx_ubuntu:disco -T

# Run a nginx container and test
$sudo docker run -itd --name my_nginx_test -p 8080:8080 nginx_ubuntu:disco
$sudo docker exec my_nginx_test -T
$sudo docker logs my_nginx_test
$curl http://localhost:8080 

# Run a nginx container and map custom nginx.conf file and static html pages
$ sudo docker run -itd --name my_wwww \
                       -p 8080:8080 \
					   -v /website/html:/usr/share/nginx/html \
					   -v /website/nginx.conf:/etc/nginx/nginx.conf \
					   nginx_ubuntu:disco

````



#### Full Nginx sample configuration file
[link] <https://www.nginx.com/resources/wiki/start/topics/examples/full/>

