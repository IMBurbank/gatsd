ARG NODE_VER=10
ARG DISTRO=alpine
ARG BASE_IMAGE=node:${NODE_VER}-${DISTRO}

FROM ${BASE_IMAGE}
LABEL maintainer="Isaac Burbank <contact@isaacburbank.com>"

ARG SITE_NAME=site
ARG WORK_DIR="/${SITE_NAME}"
ARG GATSD_DIRNAME=gatsd
ARG GATSD_DIR="${WORK_DIR}/${GATSD_DIRNAME}"
ARG SHIM_DIR=/bin

RUN apk update && \
    apk upgrade && \
    apk add --update --no-cache \
        bash \
        bash-completion \
        bash-doc \
        build-base \
        ca-certificates \
        curl \
        fftw-dev \
        g++ \
        gcc \
        git \
        libc6-compat \
        make \
        openssl \
        util-linux \
        vips-dev \
        yarn \
        --repository http://mirror.clarkson.edu/alpine/edge/testing/ \
        --repository http://mirror.clarkson.edu/alpine/edge/main
        
RUN yarn global add \
        gatsby-cli \
        netlify-cli \
        surge 

VOLUME ${WORK_DIR}
WORKDIR ${WORK_DIR}

COPY ${GATSD_DIRNAME}/_gatsd-shim ${SHIM_DIR}/gatsd
COPY ${GATSD_DIRNAME}/.bashrc ${GATSD_DIRNAME}/_entry.sh /

ENTRYPOINT ["/_entry.sh"]
CMD ["/usr/bin/env", "bash", "-l"]