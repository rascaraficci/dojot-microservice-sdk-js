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

RUN apk add --no-cache --virtual .build-deps gcc zlib-dev libc-dev bsd-compat-headers py-setuptools bash

COPY package.json package.json
COPY npm-shrinkwrap.json npm-shrinkwrap.json

RUN npm install

COPY lib ./lib
COPY index.js index.js
COPY examples/consumer/sample.js ./app/sample.js

FROM node:12.18.4-alpine

WORKDIR /opt/example

RUN apk --no-cache add \
    libsasl \
    lz4-libs

COPY --from=base /opt/example /opt/example
CMD ["node", "app/sample.js"]