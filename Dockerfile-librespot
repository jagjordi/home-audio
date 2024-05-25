FROM debian:bullseye

ARG TARGETARCH=amd64
ARG SNAPCASTVERSION=0.27.0
RUN dpkg --add-architecture arm64
RUN dpkg --add-architecture armhf
RUN dpkg --add-architecture armel
RUN dpkg --add-architecture mipsel
RUN apt-get update && apt-get install wget ca-certificates -y
RUN update-ca-certificates --fresh

RUN apt install avahi-daemon -y 
RUN apt-get install -y curl git build-essential crossbuild-essential-arm64 crossbuild-essential-armel crossbuild-essential-armhf crossbuild-essential-mipsel pkg-config
RUN apt-get install -y libasound2-dev libasound2-dev:arm64 libasound2-dev:armel libasound2-dev:armhf libasound2-dev:mipsel
RUN apt-get install -y libpulse0 libpulse0:arm64 libpulse0:armel libpulse0:armhf
 
RUN curl https://sh.rustup.rs -sSf | sh -s -- -y
ENV PATH="/root/.cargo/bin/:${PATH}"
RUN rustup target add aarch64-unknown-linux-gnu
RUN rustup target add arm-unknown-linux-gnueabi
RUN rustup target add arm-unknown-linux-gnueabihf
RUN rustup target add mipsel-unknown-linux-gnu

RUN mkdir /.cargo && \
    echo '[target.aarch64-unknown-linux-gnu]\nlinker = "aarch64-linux-gnu-gcc"' > /.cargo/config && \
    echo '[target.arm-unknown-linux-gnueabihf]\nlinker = "arm-linux-gnueabihf-gcc"' >> /.cargo/config && \
    echo '[target.arm-unknown-linux-gnueabi]\nlinker = "arm-linux-gnueabi-gcc"' >> /.cargo/config && \
    echo '[target.mipsel-unknown-linux-gnu]\nlinker = "mipsel-linux-gnu-gcc"' >> /.cargo/config

RUN mkdir /build && \
    mkdir /pi-tools && \
    curl -L https://github.com/raspberrypi/tools/archive/648a6eeb1e3c2b40af4eb34d88941ee0edeb3e9a.tar.gz | tar xz --strip-components 1 -C /pi-tools

ENV PKG_CONFIG_ALLOW_CROSS=1
ENV PKG_CONFIG_PATH_aarch64-unknown-linux-gnu=/usr/lib/aarch64-linux-gnu/pkgconfig/
ENV PKG_CONFIG_PATH_arm-unknown-linux-gnueabihf=/usr/lib/arm-linux-gnueabihf/pkgconfig/
ENV PKG_CONFIG_PATH_arm-unknown-linux-gnueabi=/usr/lib/arm-linux-gnueabi/pkgconfig/
ENV PKG_CONFIG_PATH_mipsel-unknown-linux-gnu=/usr/lib/mipsel-linux-gnu/pkgconfig/

WORKDIR /tmp
RUN curl -L https://github.com/librespot-org/librespot/archive/refs/tags/v0.4.2.tar.gz | tar xz
WORKDIR /tmp/librespot-0.4.2
RUN cargo build --release
RUN cp ./target/release/librespot /usr/local/bin
