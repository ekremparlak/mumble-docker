FROM alpine as builder
ARG npm_config_cache=$(mktemp -d)
RUN apk add --no-cache git nodejs npm yarn python3 build-base
USER 1000
WORKDIR /tmp
RUN git clone https://github.com/johni0702/mumble-web
WORKDIR /tmp/mumble-web
RUN yarn add node-sass
RUN npm install

FROM caddy
COPY --from=builder /tmp/mumble-web/dist /usr/share/caddy/