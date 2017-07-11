FROM alpine:3.4
MAINTAINER Steven lei "278566109@qq.com" 20170711
RUN echo 'http://mirrors.aliyun.com/alpine/v3.4/main/' > /etc/apk/repositories
ENV TZ Asia/Shanghai
# ensure www user exists
RUN set -xe \
    && addgroup -g 82 -S www \
    && adduser -u 82 -s /sbin/nologin -D -S -G www www \
    && apk add --no-cache --virtual .build-deps gcc make curl \
    && apk add --no-cache --virtual .run-deps libc-dev openssl-dev pcre-dev zlib-dev \
    && mkdir /src && cd /src \
    && CPUS=$(getconf _NPROCESSORS_ONLN) \
    && curl -fSL http://www.canonware.com/download/jemalloc/jemalloc-4.2.1.tar.bz2 -o jemalloc.tar.gz \
    && curl -fSL http://tengine.taobao.org/download/tengine-2.2.0.tar.gz -o tengine.tar.gz \
    # Install jemalloc
    && tar xf jemalloc.tar.gz && cd jemalloc-4.2.1 \
    && ./configure --prefix=/usr/local/jemalloc && make -j $CPUS && make install \
    # Install tengine
    && cd /src && tar xf tengine.tar.gz && cd tengine-2.2.0 \
    && ./configure \
        --prefix=/usr/local/tengine \
        --user=www \
        --group=www \
        --with-http_stub_status_module \
        --with-http_auth_request_module \
        --with-http_ssl_module \
        --with-http_realip_module \
        --with-http_concat_module \
        --with-jemalloc=/src/jemalloc-4.2.1 \
    && make -j $CPUS && make install \
    && apk del .build-deps && rm -rf /src/*

EXPOSE 80 443
CMD ["/usr/local/tengine/sbin/nginx", "-g", "daemon off;"]

    
