FROM debian:bullseye
RUN apt update && apt install -y \
    python3-pip \
    python3-gst-1.0 \
    gir1.2-gstreamer-1.0 \
    gir1.2-gst-plugins-base-1.0 \
    gstreamer1.0-plugins-good \
    gstreamer1.0-plugins-ugly \
    gstreamer1.0-tools \
    && rm -rf /var/lib/apt/lists/*
RUN python3 -m pip install --upgrade pip setuptools wheel
RUN python3 -m pip install --upgrade Mopidy \
                                     Mopidy-Mobile \
                                     Mopidy-Local \
                                     Mopidy-Iris
EXPOSE 6683
