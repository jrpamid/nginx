# argument for nginx  version.
#  can be overridden  via command-line --build-arg nginx_codename=disco|trusty|bionic
ARG ubuntu_codename

# base ubuntu 18.04/bionic image
FROM ubuntu:${ubuntu_codename}
ARG ubuntu_codename

# Image meta data
LABEL name="nginx 1.16.1" \
      description="Nginx web and reverse proxy server" \
      author="jrpamid.cloudops@gmail.com" \
      ubuntu_codename=${ubuntu_codename}

ENV  UBUNTU_CODENAME=${ubuntu_codename}

RUN set -x \
    && apt-get update \
    && apt install --no-install-recommends --no-install-suggests -y apt-transport-https ca-certificates wget gnupg1 \
    && wget https://nginx.org/keys/nginx_signing.key \
    && apt-key add nginx_signing.key \
    && echo deb https://nginx.org/packages/mainline/ubuntu/ ${UBUNTU_CODENAME} nginx >> /etc/apt/sources.list \
    && apt-get update -y \
    && apt-get install -y nginx \
    && apt-get remove --purge --auto-remove -y wget gnupg2

# copy the default nginx.conf file
COPY --chown=nginx:nginx files-to-add/nginx.conf /etc/nginx/nginx.conf

#Rediret logfiles to stdout
RUN set -x \
    && mkdir -p /var/log/nginx \
    && mkdir -p /var/cache/nginx \
    && chown -R nginx:nginx /var/cache/nginx /var/log/nginx \
    && chmod 775 /var/cache/nginx /var/log/nginx \
    && touch /var/log/nginx/{access.log,error.log,nginx.pid} \
    && ln -sf  /dev/stdout /var/log/nginx/access.log \
    && ln -sf /dev/stderr /var/log/nginx/error.log 

# Expose the nginx port 
EXPOSE 8080

# Drop to nonroot  user
USER nginx

ENTRYPOINT ["nginx"]
CMD ["-g" ,"daemon off;"]




