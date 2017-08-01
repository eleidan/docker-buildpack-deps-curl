FROM eleidan/centos:7.3.1611

LABEL maintainer="Oleg Kulyk" \
      description="CentOS 7.3.1611, curl" \
      run="docker run -it --rm -v $(pwd):/home/phantom/app eleidan/buildpack-deps-curl:centos7.3" \
      version="2017-08-01"

RUN yum -y install \
      epel-release \
    && yum -y update \
    && yum -y install \
      ca-certificates \
      curl \
      wget \
    && yum clean all

RUN set -ex; \
    if ! command -v gpg > /dev/null; then \
      yum -y install \
        gnupg2 \
        dirmngr \
      ; \
      yum clean all; \
    fi

# Install gosu utility
ENV GOSU_VERSION="1.10"
RUN set -ex; \
    \
    yum -y install dpkg; \
    \
    dpkgArch="$(dpkg --print-architecture | awk -F- '{ print $NF }')"; \
    wget -O /usr/bin/gosu "https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-$dpkgArch"; \
    wget -O /tmp/gosu.asc "https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-$dpkgArch.asc"; \
    \
    # verify the signature
    export GNUPGHOME="$(mktemp -d)"; \
    gpg --keyserver ha.pool.sks-keyservers.net --recv-keys B42F6819007F00F88E364FD4036A9C25BF357DD4; \
    gpg --batch --verify /tmp/gosu.asc /usr/bin/gosu; \
    rm -r "$GNUPGHOME" /tmp/gosu.asc; \
    \
    chmod +x /usr/bin/gosu; \
    # verify that the binary works
    gosu nobody true; \
    \
    yum -y remove dpkg; \
    yum clean all

COPY docker-entrypoint.sh /usr/local/bin/
RUN chmod a+x /usr/local/bin/docker-entrypoint.sh
ENTRYPOINT ["docker-entrypoint.sh"]

ARG CUSTOM_USER="phantom"
RUN useradd -m -s /bin/bash -u 1000 -U $CUSTOM_USER

ENV SERVICE_NAME="buildpack-deps-curl:centos7.3" \
    HOME="/home/$CUSTOM_USER"

CMD ["/bin/bash"]
