FROM centos:6.8
MAINTAINER Steven "278566109@qq.com"
ENV TZ Asia/Shanghai
ADD CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo
RUN set -xe \
    && yum makecache \
    && yum install -y passwd openssl openssh-server \
    && /bin/rm -rf /var/cache/yum/* \
    &&  yum clean all 
RUN echo 'password' | passwd --stdin root
RUN ssh-keygen -q -t rsa -f /etc/ssh/ssh_host_rsa_key -N ''
RUN ssh-keygen -q -t dsa -f /etc/ssh/ssh_host_dsa_key -N ''
RUN ssh-keygen -q -t ecdsa -f /etc/ssh/ssh_host_ecdsa_key -N ''
RUN sed -i '/^session\s\+required\s\+pam_loginuid.so/s/^/#/' /etc/pam.d/sshd
EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]
