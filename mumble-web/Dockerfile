FROM alpine as builder
RUN apk add --no-cache git nodejs npm
RUN git clone https://github.com/johni0702/mumble-web
WORKDIR /mumble-web
RUN npm install

FROM caddy
COPY --from=builder /mumble-web/dist /usr/share/caddy/
