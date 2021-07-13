FROM haoling/jenkins-swarm:3.27
MAINTAINER Steffen Bleul <sbl@blacklabelops.com>

# Need root to build image
USER root

# Add ssl certificates
COPY www.docker.com.crt /etc/pki/ca-trust/source/anchors/

# install web tools
RUN yum install -y epel-release && \
    yum install -y \
    unzip \
    zip \
    gzip \
    tar \
    curl \
    wget && \
    yum clean all && rm -rf /var/cache/yum/*
RUN curl https://bootstrap.pypa.io/pip/2.7/get-pip.py | python - &&\
    python -m pip install --upgrade "pip < 21.0"

# install Docker cli
ENV DOCKER_VERSION 1.12.1
ENV DOCKER_HOST tcp://docker:2375

RUN curl -fSL "https://get.docker.com/builds/Linux/x86_64/docker-${DOCKER_VERSION}.tgz" -o /tmp/docker.tgz && \
    tar -xzvf /tmp/docker.tgz && \
    mv docker/* /usr/local/bin/ && \
    chmod +x /usr/local/bin/docker && \
    pip install docker-compose && \
    rm -rf /var/log/* && rm -rf /tmp/*

# Switch back to user jenkins
USER $CONTAINER_UID
COPY docker-entrypoint.sh /home/jenkins/entrypoint.sh
ENTRYPOINT ["/home/jenkins/entrypoint.sh"]
CMD ["swarm"]
