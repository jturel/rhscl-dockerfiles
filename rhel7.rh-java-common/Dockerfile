FROM rhel7

MAINTAINER docker@softwarecollections.org

RUN yum install -y --setopt=tsflags=nodocs yum-utils && \
    yum-config-manager --enable rhel-server-rhscl-7-rpms && \
    yum-config-manager --enable rhel-7-server-optional-rpms && \
    yum clean all

RUN yum install -y --setopt=tsflags=nodocs rh-java-common && yum clean all



ENV	BASH_ENV=/etc/profile.d/cont-env.sh


ADD ./enablerh-java-common.sh /usr/share/cont-layer/common/env/enablerh-java-common.sh
ADD ./usr /usr
ADD ./etc /etc
ADD ./root /root

ENV HOME /home/default
RUN     groupadd -r default -f -g 1001 && \
        useradd -u 1001 -r -g default -d ${HOME} -s /sbin/nologin \
                        -c "Default Application User" default

USER 1001

ENTRYPOINT ["/usr/bin/container-entrypoint"]

CMD ["container-usage"]

