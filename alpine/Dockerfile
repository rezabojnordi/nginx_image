ARG VERSION=ubuntu
#FROM nginx:${VERSION} 
FROM alpine
#FROM nginx:${VERSION} as builder

# ENV MORE_HEADERS_VERSION=0.33
# ENV NCHAN_VERSION 1.20.1
# ENV MORE_HEADERS_GITREPO=openresty/headers-more-nginx-module
ENV nginx_module_vts 0.1.18
ENV NGINX_VERSION 1.20.1
ARG DEBIAN_FRONTEND=noninteractive
# Download sources

# For latest build deps, see https://github.com/nginxinc/docker-nginx/blob/master/mainline/alpine/Dockerfile
##COPY proxy.conf /etc/apt/apt.conf.d/proxy.conf
#COPY sources.list /etc/apt/sources.list
RUN  apk add --no-cache --virtual .build-deps \
    gcc \
    libc-dev \
    make \
    openssl-dev \
    pcre-dev \
    zlib-dev \
    linux-headers \
    libxslt-dev \
    gd-dev \
    geoip-dev \
    perl-dev \
    libedit-dev \
    mercurial \
    bash \
    alpine-sdk \
    findutils \
    build-base \
    pcre \
    openssl \
    zlib \
    git \
    wget \
    nano \
    vim
#CMD ["/usr/sbin/sshd", "-D"]
RUN wget "http://nginx.org/download/nginx-${NGINX_VERSION}.tar.gz" -O nginx-${NGINX_VERSION}.tar.gz && \
    git clone https://github.com/vozlt/nginx-module-vts && \
    tar xzvf nginx-${NGINX_VERSION}.tar.gz && \
    cd nginx-${NGINX_VERSION} && \ 
    sh -c "./configure --with-http_ssl_module --with-http_sub_module --with-http_realip_module --with-stream_ssl_preread_module --with-http_v2_module --with-stream --with-file-aio --with-threads --with-http_auth_request_module --add-module=../nginx-module-vts" && \
    make && \ 
    make install && \
    mkdir -p /usr/local/nginx/conf/vhost/ && \
    mkdir -p /var/cache/nginx/ && \
    mkdir -p /var/local/nginx/conf/ssl
COPY nginx.conf /usr/local/nginx/conf/nginx.conf

SHELL ["/bin/bash", "-eo", "pipefail", "-c"]
CMD ["/usr/local/nginx/sbin/nginx", "-g", "daemon off;"]
