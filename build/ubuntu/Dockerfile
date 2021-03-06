# argument for nginx  version.
#  can be overridden  via command-line --build-arg nginx_codename=disco|trusty|bionic
ARG ubuntu_codename=groovy

# base ubuntu 20.10/groovy
FROM ubuntu:${ubuntu_codename}
ARG ubuntu_codename

ENV  ubuntu_codename=${ubuntu_codename} \
     user=www-data \
     group=www-data

RUN set -x \
    && apt-get update \
    && apt-get install --no-install-recommends --no-install-suggests -y apt-transport-https ca-certificates wget gnupg2 lsb-release \
    && wget https://nginx.org/keys/nginx_signing.key \
    && apt-key add nginx_signing.key \
    && echo "deb https://nginx.org/packages/ubuntu ${ubuntu_codename} nginx" >> /etc/apt/sources.list \
    && apt-get update -y \
    && apt-get install -y nginx

# copy the default nginx.conf file
COPY files-to-add/nginx.conf /etc/nginx/nginx.conf

#Rediret logfiles to stdout
RUN set -x \
    && mkdir -p /var/log/nginx \
    && mkdir -p /var/cache/nginx \
    && chown -R ${user}:${group} /var/cache/nginx /var/log/nginx \
    && chmod 775 /var/cache/nginx /var/log/nginx \
    && touch /var/log/nginx/{access.log,error.log,nginx.pid} \
    && ln -sf  /dev/stdout /var/log/nginx/access.log \
    && ln -sf /dev/stderr /var/log/nginx/error.log 

# Expose the nginx port 
EXPOSE 8080

# Drop to nonroot  user
USER ${user}

# Image meta data
LABEL name="nginx" \
      description="Nginx web and reverse proxy server" \
      author="jrpamid.cloudops@gmail.com" \
      ubuntu_codename=${ubuntu_codename}

ENTRYPOINT ["nginx"]

CMD ["-g" ,"daemon off;"]




