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
ARG REPOS=/etc/apk/repositories

RUN echo http://nl.alpinelinux.org/alpine/edge/main >> ${REPOS} && \
    echo http://nl.alpinelinux.org/alpine/edge/community >> ${REPOS} && \
    echo http://nl.alpinelinux.org/alpine/edge/testing >> ${REPOS} && \
    apk update && \
    apk upgrade && \
    apk add --no-cache \
        build-base \
        util-linux \
        vips \
        vips-dev \
        vips-tools \
        fftw \
        fftw-dev \
        bash \
        bash-doc \
        bash-completion \
        ca-certificates \
        openssl \
        git \
        curl \
        yarn \
        && \
    yarn global add \
        gatsby-cli \
        netlify-cli \
        surge 

VOLUME ${WORK_DIR}
WORKDIR ${WORK_DIR}

COPY ${GATSD_DIRNAME}/_gatsd-shim ${SHIM_DIR}/gatsd
COPY ${GATSD_DIRNAME}/_bashrc /etc/profile.d/bashrc.sh
    
CMD ["/usr/bin/env","bash","-l"]