FROM eleidan/ubuntu:trusty

LABEL maintainer="Oleg Kulyk" \
      description="Ubuntu 14.04 trusty, curl" \
      run="docker run -it --rm -v $(pwd):/home/phantom/app eleidan/buildpack-deps-curl:trusty" \
      version="2017-07-09"

ARG CUSTOM_USER="phantom"

ENV SERVICE_NAME="buildpack-deps-curl:trusty" \
    HOME="/home/$CUSTOM_USER" \
    GOSU_VERSION="1.10"

COPY docker-entrypoint.sh /

RUN apt-get update && apt-get install -y --no-install-recommends \
      ca-certificates \
      curl \
      wget \
    && rm -rf /var/lib/apt/lists/*

RUN set -ex; \
    if ! command -v gpg > /dev/null; then \
      apt-get update; \
      apt-get install -y --no-install-recommends \
        gnupg2 \
        dirmngr \
      ; \
      rm -rf /var/lib/apt/lists/*; \
    fi

RUN dpkgArch="$(dpkg --print-architecture | awk -F- '{ print $NF }')" \
    && wget -O /usr/local/bin/gosu "https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-$dpkgArch" \
    && wget -O /usr/local/bin/gosu.asc "https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-$dpkgArch.asc" \
    && export GNUPGHOME="$(mktemp -d)" \
    && gpg --keyserver ha.pool.sks-keyservers.net --recv-keys B42F6819007F00F88E364FD4036A9C25BF357DD4 \
    && gpg --batch --verify /usr/local/bin/gosu.asc /usr/local/bin/gosu \
    && rm -r "$GNUPGHOME" /usr/local/bin/gosu.asc \
    && chmod +x /usr/local/bin/gosu \
    && gosu nobody true \
    && useradd -m -s /bin/bash -u 1000 $CUSTOM_USER \
    && chmod a+x /docker-entrypoint.sh

ENTRYPOINT ["/docker-entrypoint.sh"]

CMD ["/bin/bash"]
