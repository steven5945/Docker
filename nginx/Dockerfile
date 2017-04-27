FROM centos:6.8
MAINTAINER Steven lei "278566109@qq.com"
ENV TZ Asia/Shanghai
ADD CentOS6-Base-aliyun.repo /etc/yum.repos.d/CentOS-Base.repo
RUN	set -xe \
    && yum makecache && yum -y update && \
	yum -y install gcc gcc-c++ autoconf automake zlib zlib-devel openssl openssl-devel pcre pcre-devel crontabs && \
	/bin/rm -rf /var/cache/yum/* && yum clean all && \
	useradd -s /sbin/nologin www
ADD	nginx-1.10.3.tar.gz /opt/src/
RUN mkdir -p /opt/app && cd /opt/src/nginx-1.10.3 && \
	./configure --user=www --group=www --prefix=/opt/app/nginx --with-http_stub_status_module --with-http_realip_module --with-http_ssl_module && \
	make && make install && rm -rf /opt/src/*
EXPOSE	80 443
CMD ["/opt/app/nginx/sbin/nginx", "-g", "daemon off;"]
