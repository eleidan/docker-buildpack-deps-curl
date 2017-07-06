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
      gosu \
    && rm -rf /var/lib/apt/lists/*

RUN useradd -m -s /bin/bash -u 1000 $CUSTOM_USER \
    && chmod a+x /docker-entrypoint.sh

ENTRYPOINT ["/docker-entrypoint.sh"]

CMD ["/bin/bash"]
