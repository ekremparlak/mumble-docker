FROM golang:1.18 as builder
RUN apt update && apt install -y libopus-dev
RUN git clone https://github.com/Stieneee/mumble-discord-bridge.git /tmp/mumble-discord-bridge
WORKDIR /tmp/mumble-discord-bridge
RUN go install github.com/goreleaser/goreleaser@main
RUN goreleaser build --skip-validate --single-target


FROM debian:10-slim
RUN apt update && apt install -y libopus0 ca-certificates && rm -rf /var/lib/apt/lists/*
COPY --from=builder /tmp/mumble-discord-bridge/dist/mumble-discord-bridge_linux_*/mumble-discord-bridge /usr/local/bin/mumble-discord-bridge
CMD mumble-discord-bridge