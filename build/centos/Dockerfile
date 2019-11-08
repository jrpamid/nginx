FROM centos:7 as base

ARG nginx_ver=1.16.1

RUN yum update -y && \
    yum install -y epel-release && \
    yum groupinstall -y "Development Tools" && \
    yum install -y wget \
                   tar \
                   libxslt \
                   libxslt-devel \
                   perl \
                   perl-devel \
                   perl-ExtUtils-Embed \
                   libxml2 \
                   libxml2-devel \
                   gd \
                   gd-devel && \
    yum clean all

WORKDIR /tmp

RUN wget http://nginx.org/download/nginx-${nginx_ver}.tar.gz && \
    tar xvzf nginx-${nginx_ver}.tar.gz && \
    wget https://ftp.pcre.org/pub/pcre/pcre-8.43.tar.gz && \
    tar xvzf pcre-8.43.tar.gz && \
    wget https://www.zlib.net/zlib-1.2.11.tar.gz && \
    tar xvzf zlib-1.2.11.tar.gz && \
    wget https://www.openssl.org/source/openssl-1.1.1d.tar.gz && \
    tar xvzf openssl-1.1.1d.tar.gz && \
    cd nginx-${nginx_ver} && \
    ./configure --prefix=/opt/nginx \
            --sbin-path=/opt/nginx/bin/nginx \
            --modules-path=/usr/lib64/nginx/modules \
            --conf-path=/opt/nginx/conf/nginx.conf \
            --error-log-path=/opt/nginx/logs/error.log \
            --pid-path=/tmp/nginx.pid \
            --lock-path=/tmp/nginx.lock \
            --user=nginx \
            --group=nginx \
            --build=CentOS \
            --builddir=nginx-1.16.1 \
            --with-select_module \
            --with-poll_module \
            --with-threads \
            --with-file-aio \
            --with-http_ssl_module \
            --with-http_v2_module \
            --with-http_realip_module \
            --with-http_addition_module \
            --with-http_xslt_module=dynamic \
            --with-http_image_filter_module=dynamic \
            --with-http_sub_module \
            --with-http_dav_module \
            --with-http_flv_module \
            --with-http_mp4_module \
            --with-http_gunzip_module \
            --with-http_gzip_static_module \
            --with-http_auth_request_module \
            --with-http_random_index_module \
            --with-http_secure_link_module \
            --with-http_degradation_module \
            --with-http_slice_module \
            --with-http_stub_status_module \
            --http-log-path=/opt/nginx/logs/access.log \
            --http-client-body-temp-path=/var/cache/nginx/client_temp \
            --http-proxy-temp-path=/var/cache/nginx/proxy_temp \
            --http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp \
            --http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp \
            --http-scgi-temp-path=/var/cache/nginx/scgi_temp \
            --with-mail=dynamic \
            --with-mail_ssl_module \
            --with-stream=dynamic \
            --with-stream_ssl_module \
            --with-stream_realip_module \
            --with-stream_ssl_preread_module \
            --with-compat \
            --with-pcre=../pcre-8.43 \
            --with-pcre-jit \
            --with-zlib=../zlib-1.2.11 \
            --with-openssl=../openssl-1.1.1d \
            --with-openssl-opt=no-nextprotoneg \
            --with-debug  && \
      make &&\
      make install &&\
      rm -f /opt/nginx/conf/*.default &&\
      rm -f /opt/nginx/conf/{koi-utf,koi-win,win-utf} 

########################### Final Image #################################

FROM centos:7

LABEL name="nginx 1.16.1" \
      description="nginx webserver for centos 7" \
      author="jrpamid.cloudops@gmail.com"


RUN set -x &&\
    mkdir -p /var/cache/nginx &&\
    mkdir -p /opt/nginx && \
    useradd --uid 1004 \
            --system \
	    --shell /sbin/nologin \
	    --comment "Nginx User" \
	    --home /var/cache/nginx \
	    --user-group nginx  && \
    chown nginx:nginx /var/cache/nginx  
 
# copy the nginx binaries from base build image
COPY --from=base /opt/nginx/ /opt/nginx/

# copy the nginx.conf file
COPY --chown=nginx:nginx files-to-add/nginx.conf /opt/nginx/conf/

# redirect the nginx logs to stdout/terminal
RUN set -x \
    && touch /opt/nginx/logs/{access.log,error.log} \ 
    && chown -R nginx:nginx  /opt/nginx/logs \
    && ln -sf /dev/stdout /opt/nginx/logs/access.log \
    && ln -sf  /dev/stderr /opt/nginx/logs/error.log 


# graceful termination
STOPSIGNAL SIGQUIT 

# nginx port 
EXPOSE 8080

# drop from  root user to nginx user
USER nginx

# run nginx daemon in foreground
ENTRYPOINT ["/opt/nginx/bin/nginx"]
CMD ["-g", "daemon off;"]
 

