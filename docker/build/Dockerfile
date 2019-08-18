# MIT License
# Copyright (c) 2017 Nicola Worthington <nicolaw@tfb.net>

FROM debian:stretch
LABEL author="Nicola Worthington <nicolaw@tfb.net>"

ARG REPO=https://github.com/pfcm/trinitycore-1

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get -qq -o Dpkg::Use-Pty=0 update && \
    apt-get -qq -o Dpkg::Use-Pty=0 install -y --no-install-recommends \
    software-properties-common \
    python3-software-properties \
    curl \
 < /dev/null > /dev/null

RUN curl -O -L https://github.com/neechbear/blip/releases/download/v0.10/blip_0.10-1_all.deb && \
    apt install -y ./*.deb && rm *.deb

# https://github.com/TrinityCore/TrinityCore/blob/master/.travis.yml
# https://trinitycore.atlassian.net/wiki/display/tc/Requirements
RUN apt-get -qq -o Dpkg::Use-Pty=0 update && \
    apt-get -qq -o Dpkg::Use-Pty=0 install -y --no-install-recommends \
    blip \
    g++ \
    gcc \
    git \
    jq \
    cmake \
    make \
    p7zip-full \
    libboost-all-dev \
    libssl-dev \
    default-libmysqlclient-dev \
    libreadline6-dev \
    zlib1g-dev \
    libbz2-dev \
    libncurses5-dev \
 && rm -rf /var/lib/apt/lists/*

WORKDIR /

COPY build.sh /
RUN chmod +x /build.sh

VOLUME /artifacts

ENV DEBIAN_FRONTEND newt

ENTRYPOINT ["/build.sh"]
CMD ["-o", "/artifacts"]

