FROM alpine as builder
RUN apk add git libnice-dev openssl-dev rustup clang-libs build-base
RUN rustup-init -y --default-toolchain 1.48
RUN git clone https://github.com/johni0702/mumble-web-proxy
WORKDIR /mumble-web-proxy
RUN RUSTFLAGS="-C target-feature=-crt-static" /root/.cargo/bin/cargo build --release

FROM alpine
ENV MUMBLE_SERVER murmur
ENV MUMBLE_PORT 64738
ENV WS_PORT 64737
COPY --from=builder /mumble-web-proxy/target/release/mumble-web-proxy /usr/local/bin/mumble-web-proxy
RUN apk --no-cache add ca-certificates libnice libgcc
RUN adduser -H -D murmur
USER murmur
CMD mumble-web-proxy --accept-invalid-certificate --listen-ws $WS_PORT --server $MUMBLE_SERVER:$MUMBLE_PORT