ARG VERSION=ubuntu
#FROM nginx:${VERSION} 
FROM ubuntu:20.04
#FROM nginx:${VERSION} as builder

# ENV MORE_HEADERS_VERSION=0.33
# ENV NCHAN_VERSION 1.20.1
# ENV MORE_HEADERS_GITREPO=openresty/headers-more-nginx-module
ENV nginx_module_vts 0.1.18
ENV NGINX_VERSION 1.20.1
# Download sources

# For latest build deps, see https://github.com/nginxinc/docker-nginx/blob/master/mainline/alpine/Dockerfile
##COPY proxy.conf /etc/apt/apt.conf.d/proxy.conf
COPY sources.list /etc/apt/sources.list
RUN apt -y update && apt install -y libpcre3 libpcre3-dev zlib1g zlib1g-dev openssl libssl-dev gcc make git vim wget

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
# RUN rm /usr/local/nginx/conf/nginx.conf
# WORKDIR /usr/src/nginx/nginx-${NGINX_VERSION}
## WORKDIR ./nginx-${NGINX_VERSION}

# Reuse same cli arguments as the nginx:alpine image used to build
# RUN sh -c "./configure --with-http_ssl_module --with-http_sub_module --with-http_realip_module --with-stream_ssl_preread_module --with-http_v2_module --with-stream --with-file-aio --with-threads --with-http_auth_request_module --add-module=/root/nginx-module-vts" && make modules
#RUN sh -c "./configure --with-compat $CONFARGS --add-dynamic-module=/usr/src/extra_module/*" && make modules
# Production container starts here
# FROM nginx:${VERSION}

# COPY --from=builder /usr/src/nginx/nginx-${NGINX_VERSION}/objs/*_module.so /etc/nginx/modules/

# .... skipped inserting config files and stuff ...

# Validate the config
CMD ["/usr/local/nginx/sbin/nginx", "-g", "daemon off;"]
