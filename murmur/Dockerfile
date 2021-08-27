FROM debian:10-slim as builder
ARG MUMBLE_VERSION=1.3.4
RUN apt-get update && apt-get install -y \
        build-essential \
        pkg-config \
        qt5-default \
        libboost-dev \
        libasound2-dev \
        libssl-dev \
        libspeechd-dev \
        libzeroc-ice-dev \
        libpulse-dev \
        libcap-dev \
        libprotobuf-dev \
        protobuf-compiler \
        protobuf-compiler-grpc \
        libprotoc-dev \
        libogg-dev \
        libavahi-compat-libdnssd-dev \
        libsndfile1-dev \
        libgrpc++-dev \
        libxi-dev \
        libbz2-dev \
        qtcreator \
    git
RUN git clone -b $MUMBLE_VERSION https://github.com/mumble-voip/mumble
WORKDIR /mumble
RUN git submodule update --init --recursive
RUN qmake -recursive main.pro CONFIG+="no-client grpc"
RUN make release


FROM debian:10-slim
RUN adduser murmur
RUN apt-get update && apt-get install -y \
        libcap2 \
        libzeroc-ice3.7 \
        libprotobuf17 \
        libgrpc6 \
        libgrpc++1 \
        libavahi-compat-libdnssd1 \
        libqt5core5a \
        libqt5network5 \
        libqt5sql5 \
        libqt5xml5 \
        libqt5dbus5 \
    binutils \
    && strip --remove-section=.note.ABI-tag /usr/lib/*-linux-gnu/libQt5Core.so.5 \
    && apt remove -y binutils \
        && rm -rf /var/lib/apt/lists/*
COPY --from=builder /mumble/release/murmurd /usr/bin/murmurd
COPY --from=builder /mumble/scripts/murmur.ini /etc/murmur/murmur.ini
RUN mkdir /var/lib/murmur && \
        chown murmur:murmur /var/lib/murmur && \
        sed -i 's/^database=$/database=\/var\/lib\/murmur\/murmur.sqlite/' /etc/murmur/murmur.ini
EXPOSE 64738/tcp 64738/udp 50051
VOLUME /var/lib/murmur
USER murmur
CMD /usr/bin/murmurd -v -fg -ini /etc/murmur/murmur.ini