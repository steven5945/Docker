FROM centos:6.8
MAINTAINER Steven "278566109@qq.com" 20170323
COPY CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo
ENV TZ Asia/Shanghai
ENV JAVA_HOME /opt/app/jdk1.8.0_91
ENV CLASSPATH .:%JAVA_HOME%/lib/dt.jar:%JAVA_HOME%/lib/tools.jar
ENV PATH $PATH:$JAVA_HOME/bin:/opt/app/apache-maven-3.2.5/bin
ENV rdeps="crontabs"
ENV bdeps="gcc gcc-c++ autoconf automake unzip wget"

# Add user and move app directory
RUN useradd elasearch

# Install elasticsearch ik
RUN set -xe \
    && yum makecache && yum install -y $rdeps $bdeps \
    # Download tar files
    && mkdir -p /opt/app && cd /opt/app \
    && wget http://dl-in.izhaoshebei.com/elasticsearch.tar.gz \
    && tar xf elasticsearch.tar.gz --strip-components 1 \
    && chown -R elasearch:elasearch /opt/app/ \
    && cd /opt/app/elasticsearch-2.3.3/bin/ \
    && ./plugin install mobz/elasticsearch-head \
	&& cd /opt/app/elasticsearch-ik \
    && mvn clean && mvn compile && mvn package \
	&& mkdir -p /opt/app/elasticsearch-2.3.3/plugins/ik \
    && cp -rf target/releases/* /opt/app/elasticsearch-2.3.3/plugins/ik/ \
	&& cd /opt/app/elasticsearch-2.3.3/plugins/ik/ \
    && unzip -qx elasticsearch-analysis-ik-1.9.3.zip \
    # Clean up
    && rm -f /opt/app/elasticsearch.tar.gz \
	&& rm -f elasticsearch-analysis-ik-1.9.3.zip \
	&& rm -rf /opt/app/elasticsearch-ik \
	&& rm -rf /opt/app/apache-maven-3.2.5 \
    && yum -y remove $bdeps && yum clean all \
    && rm -rf /var/cache/yum/*

COPY start.sh /usr/bin/
CMD ["start.sh"]
