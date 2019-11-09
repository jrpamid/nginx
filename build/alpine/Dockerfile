# Base OS Image
ARG alpine_ver
FROM alpine:$alpine_ver as build

# default nginx stable version.
# nginx_ver can be over-ridden with build argument
ARG nginx_ver=1.16.1

# Install the required dependencies to compile and build  from the source code
RUN apk add --no-cache --virtual .build-deps \
            build-base \ 
            wget \ 
            tar \
            gnupg \
            openssl \
            openssl-dev \ 
            zlib \
            zlib-dev \
            pcre \
            pcre-dev \
            linux-headers

# working dir where the source code has to be downloaded to 
WORKDIR /tmp
 
# Download Nginx stable version source code
RUN set -x \
    # nginx source code
    && wget http://nginx.org/download/nginx-${nginx_ver}.tar.gz \
    && tar xvzf nginx-${nginx_ver}.tar.gz \
    && cd nginx-${nginx_ver} \
    && ./configure --prefix=/opt/nginx/ \
		--sbin-path=/opt/nginx/bin/nginx \
		--conf-path=/opt/nginx/conf/nginx.conf \ 
		--error-log-path=/opt/nginx/logs/error.log \ 
		--pid-path=/opt/nginx/logs/nginx.pid \ 
		--lock-path=/opt/nginx/logs/nginx.lock \ 
		--user=nginx \
		--group=nginx \
		--with-threads \
		--with-file-aio \
		--with-http_ssl_module \ 
		--with-http_v2_module \
		--with-http_realip_module \ 
		--with-http_addition_module \ 
		--with-http_sub_module \
		--with-http_dav_module \
		--with-http_flv_module \
		--with-http_mp4_module \
		--with-http_gunzip_module \ 
		--with-http_gzip_static_module \ 
		--with-http_auth_request_module \
		--with-http_random_index_module \
		--with-http_secure_link_module \
		--with-http_slice_module \
		--with-http_stub_status_module \ 
		--http-log-path=/opt/nginx/logs/access.log \
		--http-client-body-temp-path=/var/cache/nginx/client_temp \ 
		--http-proxy-temp-path=/var/cache/nginx/proxy_temp \
		--http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp \ 
		--http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp \
		--http-scgi-temp-path=/var/cache/nginx/scgi_temp \
		--with-mail \
		--with-mail_ssl_module \ 
		--with-stream \
		--with-stream_ssl_module \ 
		--with-stream_realip_module \
    && make \
    && make install \
    && strip  /opt/nginx/bin/nginx


# Final Minimal Image 
FROM alpine:${alpine_ver} 

# LABELS, MetaData
LABEL author=jrpamid.cloudops@gmail.com \
      name="nginx" \
      description="opensource high performance, light weight web and reverse proxy server"

# Environement Variables
ENV NGINX_USER=nginx \
    NGINX_UID=1004 \
    NGINX_PORT=8080 


COPY --from=build /opt/ /opt/

# Create Nginx user and group accounts
RUN set -x \
    && apk add pcre \
    && addgroup ${NGINX_USER} \
    && adduser -u ${NGINX_UID} -G ${NGINX_USER} -S  ${NGINX_USER} \
    && mkdir -p /opt/nginx/logs /var/cache/nginx \
    && touch /opt/nginx/logs/{access.log,error,nginx.pid} \
    && chown -R ${NGINX_USER}:${NGINX_GROUP} /opt/nginx/logs /var/cache/nginx \
    && chmod -R 777 /opt/nginx/logs \
    && ln -sf /dev/stdout  /opt/nginx/logs/access.log \
    && ln -sf /dev/stderr /opt/nginx/logs/error.log 

COPY files-to-add/nginx.conf /opt/nginx/conf/

# Change default stop signal from SIGTERM  to SIGQUIT for graceful  shutdown
STOPSIGNAL SIGQUIT

# Exposing the port 8080 
EXPOSE ${NGINX_PORT}/tcp

# Drop down from root user to nginx:nginx user/group
USER ${NGINX_USER}

# Execute the Nginx as a front ground process
ENTRYPOINT ["/opt/nginx/bin/nginx"]
CMD ["-g", "daemon off;"]



