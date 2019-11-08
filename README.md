## Nginx
[link] <https://nginx.org/en/docs/>
nginx [engine x] is an HTTP and reverse proxy server, a mail proxy server, and a generic TCP/UDP proxy server

### Nginx on Ubuntu
````
This Container image is build using the Nginx.org pre built stable packages from the Nginx repo.
The base ubuntu image to be used can be provided as a build argument and the appropiate nginx package will be installed.
Default locations:-
Nginx binary = /usr/sbin/nginx/nginx
Nginx Conf = /etc/nginx/nginx.conf
Nginx Web Root = /usr/share/nginx/html
Nginx Logs = /var/log/nginx/{access.log, error.log}
````
```` 	
#ubuntu 16.04 codename=xenial
$sudo docker build --tag nginx_ubuntu:xenial --build-arg ubuntu_codename=xenial .

#ubuntu 18.04 codename = bionic
$sudo docker build --tag nginx_ubuntu:bionic --build-arg ubuntu_codename=bionic .
#ubuntu 19.04 codename=disco
$sudo docker build --tag nginx_ubuntu:disco --build-arg ubuntu_codename=disco .
````
##### 


#### Full Nginx sample configuration file
[link] <https://www.nginx.com/resources/wiki/start/topics/examples/full/>

