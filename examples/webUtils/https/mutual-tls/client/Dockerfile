FROM node:12.18.4-alpine AS base

WORKDIR /opt/example

RUN apk --no-cache add \
    bash \
    g++ \
    ca-certificates \
    lz4-dev \
    musl-dev \
    cyrus-sasl-dev \
    openssl-dev \
    make \
    python


RUN apk add \
    --no-cache \
    --virtual \
    .build-deps \
    gcc \
    zlib-dev \
    libc-dev \
    bsd-compat-headers \
    py-setuptools bash

# library (essential files)
COPY package.json microservice-sdk-pkg/
COPY npm-shrinkwrap.json microservice-sdk-pkg/
COPY lib microservice-sdk-pkg/lib
COPY index.js microservice-sdk-pkg/

# example application
COPY examples/webUtils/https/mutual-tls/client/package*.json ./
COPY examples/webUtils/https/mutual-tls/client/index.js ./

RUN npm install --production

FROM node:12.18.4-alpine

WORKDIR /opt/example

VOLUME /opt/example/tls

RUN apk --no-cache add \
    libsasl \
    lz4-libs

COPY --from=base /opt/example .

CMD ["node", "index.js"]
