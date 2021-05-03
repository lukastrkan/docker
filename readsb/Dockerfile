FROM debian:buster-slim as builder
ENV TZ=Europe/Prague
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
RUN apt update && \
apt-get install --reinstall ca-certificates -y && \
apt install --no-install-suggests --no-install-recommends collectd-core rrdtool librrd-dev \
libprotobuf-c-dev protobuf-c-compiler libncurses5-dev libusb-1.0-0-dev \
lighttpd build-essential binutils cmake debhelper dh-systemd pkg-config \
fakeroot git -y

FROM builder as builded
WORKDIR /src
RUN git clone --branch dev --depth 1 --single-branch https://github.com/mictronics/readsb-protobuf readsb && \
cd readsb && \
make

FROM debian:buster-slim
WORKDIR /usr/local/bin/
COPY --from=builded /src/readsb/readsb /usr/local/bin/

RUN chmod +x /usr/local/bin/readsb && \
apt update && \
apt install libncurses6 libprotobuf-c-dev libc6-dev librrd8 -y
ENV COMMAND = ""
CMD readsb ${COMMAND}
