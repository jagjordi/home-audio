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
RUN DEBARCH="$TARGETARCH"; \
    if [ "$DEBARCH" = "arm" ]; then DEBARCH=armhf; fi; \
    wget "https://github.com/badaix/snapcast/releases/download/v${SNAPCASTVERSION}/snapserver_${SNAPCASTVERSION}-1_${DEBARCH}.deb" -O /tmp/snapserver.deb

#RUN dpkg -i --force-all /tmp/snapserver.deb
#RUN apt-get -f install -y
RUN apt install /tmp/snapserver.deb -y
