FROM rhel6

MAINTAINER docker@softwarecollections.org

RUN yum install -y --setopt=tsflags=nodocs yum-utils && \
    yum-config-manager --enable rhel-server-rhscl-6-rpms && \
    yum-config-manager --enable rhel-6-server-optional-rpms && \
    yum clean all

RUN yum install -y --setopt=tsflags=nodocs rh-mysql56 hostname gettext which && yum clean all


EXPOSE 3306

ENV	MYSQL_VERSION=5.6 \
	HOME=/var/lib/mysql \
	BASH_ENV=/etc/profile.d/cont-env.sh

LABEL	openshift.io/tags="database,mysql,rh-mysql56"  \
	k8s.io/display-name="MySQL 5.6"  \
	openshift.io/expose-services="3306:mysql"  \
	k8s.io/description="MySQL is a multi-user, multi-threaded SQL database server" 

ADD ./.bashrc $HOME/.bashrc
ADD ./usr /usr
ADD ./my.cnf /etc/opt/rh/rh-mysql56/my.cnf
ADD ./enablerh-mysql56.sh /usr/share/cont-layer/common/env/enablerh-mysql56.sh
ADD ./etc /etc
ADD ./root /root

RUN	/usr/libexec/cont-setup && \
	:

USER 27

VOLUME ["/var/lib/mysql/data"]

ENTRYPOINT ["/usr/bin/container-entrypoint"]

CMD ["cont-mysqld"]


