# MIT License
# Copyright (c) 2017 Nicola Worthington <nicolaw@tfb.net>

FROM debian:stretch
LABEL author="Nicola Worthington <nicolaw@tfb.net>"

ENV DEBIAN_FRONTEND noninteractive

RUN echo "deb http://ftp.debian.org/debian jessie-backports main" > /etc/apt/sources.list.d/backports.list

RUN apt-get -qq -o Dpkg::Use-Pty=0 update && \
    apt-get -qq -o Dpkg::Use-Pty=0 install -y --no-install-recommends \
    libboost-all-dev \
    libssl-dev=1.0.2l-1~bpo8+1 \
    default-libmysqlclient-dev \
    netcat \
 < /dev/null > /dev/null \
 && rm -rf /var/lib/apt/lists/*

# Configurable via INSTALL_PREFIX from the Makefile, and/or using
# --define CMAKE_INSTALL_PREFIX=/your/path with the docker/build/ container,
# (which is also published on Ducker Hub as nicolaw/trinitycore).
ENV install_prefix /opt/trinitycore

WORKDIR "${install_prefix}/bin"
VOLUME "${install_prefix}/etc"

# Wait for the database server to come up first.
ADD https://raw.githubusercontent.com/vishnubob/wait-for-it/master/wait-for-it.sh "${install_prefix}/bin/wait-for-it.sh"
COPY authserver.sh "${install_prefix}/bin/authserver.sh"
COPY authserver "${install_prefix}/bin/authserver"

RUN chmod +x "${install_prefix}/bin"/*

ENV DEBIAN_FRONTEND newt

ENTRYPOINT ["./authserver.sh"]

