# MIT License
# Copyright (c) 2017 Nicola Worthington <nicolaw@tfb.net>

FROM debian:stretch
LABEL author="Nicola Worthington <nicolaw@tfb.net>"

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get -qq -o Dpkg::Use-Pty=0 update && \
    apt-get -qq -o Dpkg::Use-Pty=0 install -y --no-install-recommends \
    software-properties-common python3-software-properties \
    curl \
 < /dev/null > /dev/null

RUN curl -O -L https://github.com/neechbear/blip/releases/download/v0.9-1/blip_0.9-1_all.deb && \
    apt install -y ./*.deb && rm *.deb

RUN apt-get update
RUN apt-get install -y --no-install-recommends \
    default-libmysqlclient-dev \
    libboost-all-dev \
    zlib1g-dev

RUN echo "deb http://ftp.debian.org/debian jessie-backports main" > /etc/apt/sources.list
RUN apt-get update
RUN apt-get install -y -t jessie-backports libssl-dev=1.0.2l-1~bpo8+1

RUN rm -rf /var/lib/apt/lists/*
#RUN apt-get -qq -o Dpkg::Use-Pty=0 update && \
#    apt-get -qq -o Dpkg::Use-Pty=0 install -y --no-install-recommends \
#    libboost-all-dev \
#    libssl-dev=1.0.2l-1~bpo8+1 \
#    default-libmysqlclient-dev \
# < /dev/null > /dev/null \
# && rm -rf /var/lib/apt/lists/*

ENV install_prefix /opt/trinitycore
ENV PATH "/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:${install_prefix}/bin"

WORKDIR "${install_prefix}/bin"

COPY tools.sh /tools.sh

COPY mapextractor "${install_prefix}/bin/mapextractor"
COPY mmaps_generator "${install_prefix}/bin/mmaps_generator"
COPY vmap4assembler "${install_prefix}/bin/vmap4assembler"
COPY vmap4extractor "${install_prefix}/bin/vmap4extractor"

RUN chmod +x "${install_prefix}/bin"/* /tools.sh

VOLUME /artifacts

ENV DEBIAN_FRONTEND newt

ENTRYPOINT ["/tools.sh"]
CMD ["-o", "/artifacts", "-i", "/World_of_Warcraft"]

