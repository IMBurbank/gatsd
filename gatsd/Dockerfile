
# Copyright 2018 GatsD Authors. All Rights Reserved.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
# ============================================================================
#
# GatsD Dockerfile Development Container
#
# You can use this image to develop your GatsbyJS project completely within
# the container.
ARG NODE_VER=10
ARG DISTRO=alpine
ARG BASE_IMAGE=node:${NODE_VER}-${DISTRO}

FROM ${BASE_IMAGE}
LABEL maintainer="Isaac Burbank <contact@isaacburbank.com>"

ARG SITE_NAME
ARG WORK_DIR
ARG GATSD_VERSION
ARG GATSD_DIRNAME
ARG GATSD_DIR
ARG GATSBY_PORT

ARG SHIM_DIR=/bin
ARG REPOS=/etc/apk/repositories

ENV SITE_NAME ${SITE_NAME:-site}
ENV WORK_DIR ${WORK_DIR:-/${SITE_NAME}}
ENV GATSD_VERSION ${GATSD_VERSION:-nonrelrease}
ENV GATSD_DIRNAME ${GATSD_DIRNAME:-gatsby-docker}
ENV GATSD_DIR ${GATSD_DIR:-${WORK_DIR}/${GATSD_DIRNAME}}
ENV GATSBY_PORT ${GATSBY_PORT:-8000}

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
        yarn

RUN yarn global add \
        gatsby-cli \
        netlify-cli \
        surge
        
VOLUME ${WORK_DIR}
WORKDIR ${WORK_DIR}

COPY ${GATSD_DIRNAME}/_gatsd-shim ${GATSD_DIRNAME}/bash.bashrc ./

RUN chmod a+rx _gatsd-shim && mv _gatsd-shim ${SHIM_DIR}/gatsd && \
    chmod a+rwx bash.bashrc && mv bash.bashrc /etc/profile.d/bashrc.sh
    
CMD ["/usr/bin/env","bash","-l"]