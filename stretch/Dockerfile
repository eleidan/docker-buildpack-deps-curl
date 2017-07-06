FROM eleidan/debian:stretch

LABEL maintainer="Oleg Kulyk" \
      description="Debian GNU/Linux stretch, curl" \
      run="docker run -it --rm -v $(pwd):/home/phantom/app eleidan/buildpack-deps:stretch-curl" \
      version="2017-07-06"

ARG CUSTOM_USER="phantom"

ENV SERVICE_NAME="buildpack-deps:stretch-curl" \
    HOME="/home/$CUSTOM_USER" \
    GOSU_VERSION="1.10"

COPY docker-entrypoint.sh /

RUN apt-get update && apt-get install -y --no-install-recommends \
      ca-certificates \
      curl \
      wget \
      gnupg2 \
      dirmngr \
    && rm -rf /var/lib/apt/lists/*

RUN dpkgArch="$(dpkg --print-architecture | awk -F- '{ print $NF }')" \
    && wget -O /usr/local/bin/gosu "https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-$dpkgArch" \
    && chmod +x /usr/local/bin/gosu \
    && gosu nobody true \
    && useradd -m -s /bin/bash -u 1000 $CUSTOM_USER \
    && chmod a+x /docker-entrypoint.sh

ENTRYPOINT ["/docker-entrypoint.sh"]

CMD ["/bin/bash"]
