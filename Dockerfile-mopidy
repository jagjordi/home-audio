FROM debian:bullseye
RUN apt update
RUN apt install -y \
    build-essential \
    python3-dev \
    python3-pip \
    python3-gst-1.0 \
    gir1.2-gstreamer-1.0 \
    gir1.2-gst-plugins-base-1.0 \
    gstreamer1.0-plugins-good \
    gstreamer1.0-plugins-ugly \
    gstreamer1.0-plugins-bad \
    gstreamer1.0-tools \
    libgstreamer1.0-dev \
    libgstreamer-plugins-base1.0-dev \
    pkg-config \
    curl \
    git
RUN git clone --depth 1 https://gitlab.freedesktop.org/gstreamer/gst-plugins-rs
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
WORKDIR  gst-plugins-rs
RUN cargo build --package gst-plugin-spotify --release
RUN install -m 644 target/release/libgstspotify.so $(pkg-config --variable=pluginsdir gstreamer-1.0)/
RUN gst-inspect-1.0 spotify
RUN python3 -m pip install --upgrade pip setuptools wheel
RUN python3 -m pip install --upgrade Mopidy \ 
                                     Mopidy-Youtube \ 
                                     Mopidy-Mobile \
                                     Mopidy-Iris \
                                     yt-dlp \
                                     ytmusicapi
RUN git clone https://github.com/mopidy/mopidy-spotify
WORKDIR mopidy-spotify
RUN python3 -m pip install .
EXPOSE 6683
