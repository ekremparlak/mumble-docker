FROM node:14-alpine as builder
RUN apk add --no-cache git python2 build-base
USER node
WORKDIR /tmp
RUN git clone https://github.com/Johni0702/mumble-web
WORKDIR /tmp/mumble-web
RUN npm install

FROM caddy
COPY --from=builder /tmp/mumble-web/dist /usr/share/caddy/