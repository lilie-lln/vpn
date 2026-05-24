FROM teddysun/xray:latest
RUN apk add --no-cache gettext
WORKDIR /etc/xray
COPY config.template.json .
CMD envsubst < config.template.json > config.json && xray run -config config.json