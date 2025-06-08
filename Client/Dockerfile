FROM golang:1.23.10-bookworm

ENV QT_PLUGIN_PATH=/usr/lib/x86_64-linux-gnu/qt5/plugins
ENV XDG_RUNTIME_DIR=/tmp/runtime-root
ARG CMAKE_VERSION=3.31.5

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        build-essential \
        qt6-base-dev \
        qt6-websockets-dev \
        qtbase5-dev \
        qtbase5-dev-tools \
        libqt5sql5 \
        libqt5sql5-sqlite \
        libsqlite3-dev \
        git \
        wget \
        sudo \
        libcap2-bin \
        zlib1g-dev \
        libssl-dev \
        mingw-w64 \
        curl \
        ca-certificates && \
    rm -rf /var/lib/apt/lists/*



RUN wget https://github.com/Kitware/CMake/releases/download/v${CMAKE_VERSION}/cmake-${CMAKE_VERSION}.tar.gz && \
    tar -zxvf cmake-${CMAKE_VERSION}.tar.gz && \
    cd cmake-${CMAKE_VERSION} && \
    ./bootstrap && \
    make -j$(nproc) && \
    make install && \
    cd .. && \
    rm -rf cmake-${CMAKE_VERSION}*


RUN mkdir -p /tmp/runtime-root
RUN mkdir -p /app/payloads

WORKDIR /app
VOLUME ["/app/payloads"]


COPY . .
RUN make client
ENTRYPOINT ["./dist/AdaptixClient"]
