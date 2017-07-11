FROM centos:6.8
MAINTAINER Steven lei "278566109@qq.com" 20170424
ENV TZ Asia/Shanghai
COPY CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo
ENV bdeps="gcc gcc-c++ autoconf automake wget unzip"
ENV rdeps="openssl openssl-devel crontabs gmp-devel libicu-devel zlib libxml libjpeg freetype libpng gd curl libiconv zlib-devel libxml2-devel libxslt-devel libjpeg-devel freetype-devel libpng-devel gd-devel curl-devel bzip2 bzip2-devel libevent-devel patch libevent-devel"

RUN set -xe \
    && yum makecache && yum -y update \
    && yum install -y $bdeps $rdeps \
    && useradd -s /sbin/nologin www \
    ### Install PHP ext and module.
    && mkdir /src && cd /src/ \
    # download packets
    && wget http://dl-in.izhaoshebei.com/lnmp_packets/libiconv-1.13.tar.gz \
    && wget http://dl-in.izhaoshebei.com/lnmp_packets/libmcrypt-2.5.8.tar.gz \
    && wget http://dl-in.izhaoshebei.com/lnmp_packets/mhash-0.9.9.9.tar.gz \
    && wget http://dl-in.izhaoshebei.com/lnmp_packets/mcrypt-2.6.8.tar.gz \
    && wget http://dl-in.izhaoshebei.com/lnmp_packets/php-5.5.30.tar.gz \
    && wget http://dl-in.izhaoshebei.com/lnmp_packets/memcache-3.0.8.tgz \
    && wget http://dl-in.izhaoshebei.com/lnmp_packets/mongo-1.6.12.tgz \
    && wget http://dl-in.izhaoshebei.com/lnmp_packets/redis-2.2.7.tgz \
    && wget http://dl-in.izhaoshebei.com/lnmp_packets/vin.tar.gz \
    && wget http://dl-in.izhaoshebei.com/lnmp_packets/cvin.tar.gz \
    && wget http://dl-in.izhaoshebei.com/lnmp_packets/cphalcon-2.0.9.tar.gz \
    && wget http://dl-in.izhaoshebei.com/lnmp_packets/pub_sub.tar.gz \
    && wget http://dl-in.izhaoshebei.com/lnmp_packets/trie_filter.tar.gz \
    && wget http://dl-in.izhaoshebei.com/lnmp_packets/ffmpeg.tar.gz \
    # Set CPU Cores
    && CPUS=`grep 'core id' /proc/cpuinfo | sort -u | wc -l` \
    # Install libiconv
    && tar xf libiconv-1.13.tar.gz && cd libiconv-1.13 \
    && ./configure --prefix=/usr/local \
    && make -j $CPUS && make install \
    # Install libmcrypt
    && cd /src/ && tar xf libmcrypt-2.5.8.tar.gz && cd libmcrypt-2.5.8/ \
    && ./configure && make -j $CPUS && make install \
    && /sbin/ldconfig && cd libltdl \
    && ./configure --enable-ltdl-install && make -j $CPUS && make install \
    # Install mhash
    && cd /src/ && tar xf mhash-0.9.9.9.tar.gz && cd mhash-0.9.9.9/ \
    && ./configure && make -j $CPUS && make install \
    && ln -s /usr/local/lib/libmcrypt.la /usr/lib/libmcrypt.la \
    && ln -s /usr/local/lib/libmcrypt.so /usr/lib/libmcrypt.so \
    && ln -s /usr/local/lib/libmcrypt.so.4 /usr/lib/libmcrypt.so.4 \
    && ln -s /usr/local/lib/libmcrypt.so.4.4.8 /usr/lib/libmcrypt.so.4.4.8 \
    && ln -s /usr/local/lib/libmhash.a /usr/lib/libmhash.a \
    && ln -s /usr/local/lib/libmhash.la /usr/lib/libmhash.la \
    && ln -s /usr/local/lib/libmhash.so /usr/lib/libmhash.so \
    && ln -s /usr/local/lib/libmhash.so.2 /usr/lib/libmhash.so.2 \
    && ln -s /usr/local/lib/libmhash.so.2.0.1 /usr/lib/libmhash.so.2.0.1 \
    && ln -s /usr/lib64/mysql /usr/lib/ \
    && ln -s /usr/local/lib/libiconv.so.2 /usr/lib/libiconv.so.2 \
    && ln -s /usr/local/lib/libiconv.so.2 /usr/lib64/libiconv.so.2 \
    # Install mcrypt
    && cd /src/ && tar xf mcrypt-2.6.8.tar.gz && cd mcrypt-2.6.8 \
    && /sbin/ldconfig && ./configure && make -j $CPUS && make install \
    # Install php
    && cd /src/ && tar xf php-5.5.30.tar.gz && cd php-5.5.30 \
    && ./configure --prefix=/opt/app/php --with-config-file-path=/opt/app/php/etc --with-mysql=mysqlnd --with-mysqli=mysqlnd --with-pdo-mysql=mysqlnd --with-gd --with-zlib --enable-bcmath --enable-shmop --with-curl --enable-fpm --enable-mbstring --enable-gd-native-ttf --with-openssl --enable-pcntl --enable-sockets --with-xmlrpc --enable-soap --without-pear --with-gettext --with-mcrypt --with-libdir=lib64 --with-freetype-dir=/usr --with-png-dir=/usr --enable-sysvmsg --enable-sysvshm --enable-sysvsem --with-gmp --with-jpeg-dir=/usr --with-libxml-dir=/usr --enable-exif --with-bz2 --enable-intl --enable-opcache --with-mhash --with-iconv --enable-pdo --enable-zip \
    && make -j $CPUS ZEND_EXTRA_LIBS='-liconv' && make install \
    && mkdir -p /tmp/session && chmod 777 /tmp/session \
    # Install memcache ext 
    && cd /src/ && tar xf memcache-3.0.8.tgz && cd memcache-3.0.8/ && /opt/app/php/bin/phpize \
    && ./configure --with-php-config=/opt/app/php/bin/php-config && make -j $CPUS && make install \
    # Install Mongo ext
    && cd /src/ && tar xf mongo-1.6.12.tgz && cd mongo-1.6.12 && /opt/app/php/bin/phpize \
    && ./configure --with-php-config=/opt/app/php/bin/php-config && make -j $CPUS && make install \
    # Install redis ext
    && cd /src/ && tar xf redis-2.2.7.tgz && cd redis-2.2.7 && /opt/app/php/bin/phpize \
    && ./configure --with-php-config=/opt/app/php/bin/php-config && make -j $CPUS && make install \
    # Install vin ext
    && cd /src/ && tar xf vin.tar.gz && cd vin && /opt/app/php/bin/phpize && chmod +x configure \
    && ./configure --with-php-config=/opt/app/php/bin/php-config --enable-vin && make -j $CPUS && make install \
    # Install cvin ext
    && cd /src/ && tar xf cvin.tar.gz && cd cvin && /opt/app/php/bin/phpize && chmod +x configure \
    && ./configure --with-php-config=/opt/app/php/bin/php-config && make -j $CPUS && make install \
    # Install cphalcon ext
    && cd /src/ && tar xf cphalcon-2.0.9.tar.gz && cd cphalcon-2.0.9/build && chmod +x install && ./install \
    ### Install subsribe
    && cd /src/ && tar xf pub_sub.tar.gz && cd hiredis && make -j $CPUS && make install \
    && cd /src/subsribe && gcc -o tt subscribe.c /usr/local/lib/libhiredis.a /usr/local/include/hiredis/adapters/libevent.h -levent \
    && mv tt /opt/app/ \
    # Install trie_filter
    && cd /src/ && tar xf trie_filter.tar.gz && cd trie_filter/libdatrie-0.2.9 \
    && ./configure LDFLAGS=-L/usr/local/lib LIBS=-liconv && make -j $CPUS && make install \
    && cd /src/trie_filter/php-ext-trie-filter && /opt/app/php/bin/phpize \
    && ./configure --with-php-config=/opt/app/php/bin/php-config && make -j $CPUS && make install \
    # Install dpp 
    && cd /src/trie_filter/ && gcc -o dpp dpp.c -ldatrie && mkdir /opt/app/trie_filter && mv dpp /opt/app/trie_filter \
    # Install yasm
    && cd /src/ && tar xf ffmpeg.tar.gz && cd ffmpeg/yasm-1.3.0/ && ./configure && make -j $CPUS && make install \
    # Install lame
    && cd /src/ffmpeg/lame-3.99.5 && ./configure && make -j $CPUS && make install \
    # Install opencore-amr
    && cd /src/ffmpeg/opencore-amr-0.1.3 && ./configure && make -j $CPUS && make install \
    # Install amrnb
    && cd /src/ffmpeg/amrnb-11.0.0.0 && ./configure && make -j $CPUS && make install \
    # Install amrwb
    && cd /src/ffmpeg/amrwb-11.0.0.0 && ./configure && make -j $CPUS && make install \
    # Install ffmpeg
    && cd /src/ffmpeg/ffmpeg-2.5.3 \
    && ./configure --prefix=/opt/app/ffmpeg --enable-libmp3lame --enable-libopencore-amrnb --enable-libopencore-amrwb --enable-version3 --enable-shared \
    && make -j $CPUS && make install \
    && echo '/opt/app/ffmpeg/lib/' >> /etc/ld.so.conf && echo '/usr/local/lib/' >> /etc/ld.so.conf && ldconfig -v \
    # Clean up
    && yum remove -y $bdeps \
    && yum clean all \
    && /bin/rm -rf /var/cache/yum/* \
    && rm -rf /src/*
COPY php.ini /opt/app/php/etc/
COPY php-fpm.conf /opt/app/php/etc/
CMD ["/opt/app/php/sbin/php-fpm","-F"]
