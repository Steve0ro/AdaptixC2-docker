FROM ubuntu:24.04

ENV PATH="/usr/local/go/bin:${PATH}"
ARG CMAKE_VERSION=3.31.5
EXPOSE 50203

RUN apt-get update && \
    apt-get install -y software-properties-common && \
    add-apt-repository ppa:kubuntu-ppa/backports -y && \
    apt-get update && \
    apt-get install -y --no-install-recommends \
        build-essential \
        qt6-base-dev \
        qt6-websockets-dev \
        git \
        wget \
        sudo \
        libcap2-bin \
        zlib1g-dev \
        libssl-dev \
        mingw-w64 \
        libnet-ifconfig-wrapper-perl \
        vim \
        curl \
        ca-certificates && \
    rm -rf /var/lib/apt/lists/*

ADD https://go.dev/dl/go1.23.10.linux-amd64.tar.gz /tmp/go1.23.10.linux-amd64.tar.gz
RUN rm -rf /usr/local/go && \
    tar -C /usr/local -xzf /tmp/go1.23.10.linux-amd64.tar.gz && \
    rm /tmp/go1.23.10.linux-amd64.tar.gz


RUN wget https://github.com/Kitware/CMake/releases/download/v${CMAKE_VERSION}/cmake-${CMAKE_VERSION}.tar.gz && \
    tar -zxvf cmake-${CMAKE_VERSION}.tar.gz && \
    cd cmake-${CMAKE_VERSION} && \
    ./bootstrap && \
    make -j$(nproc) && \
    make install && \
    cd .. && \
    rm -rf cmake-${CMAKE_VERSION}*

WORKDIR /app
COPY . .

RUN make server
RUN make extenders
RUN openssl req -x509 -nodes \
    -newkey rsa:2048 \
    -keyout ./dist/server.rsa.key \
    -out ./dist/server.rsa.crt \
    -days 3650 \
    -subj "/C=US/ST=TX/L=Austin/O=Server/OU=Eng/CN=server.local"

CMD ["/app/dist/adaptixserver", "-profile", "/app/dist/profile.json", "-debug"]
