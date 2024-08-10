# syntax=docker.io/docker/dockerfile:1

ARG BASE_IMAGE=docker.io/mirakc/mirakc:3.1.4-alpine

FROM $BASE_IMAGE AS b25-build

RUN apk add --no-cache cmake g++ libtool make pkgconf pcsc-lite-dev

RUN --mount=type=bind,source=./libaribb25,target=/tmp/libaribb25/src \
  cmake -S /tmp/libaribb25/src -B /tmp/libaribb25/build \
    -D CMAKE_BUILD_TYPE=Release -D CMAKE_INSTALL_PREFIX=/opt/libaribb25 && \
  cd /tmp/libaribb25/build && \
  make -j $(nproc) && make install

FROM $BASE_IMAGE
COPY --from=b25-build /opt/libaribb25 /usr/local/
RUN apk add --no-cache ccid pcsc-lite-libs

ENTRYPOINT []
CMD ["/bin/sh", "-c", "pcscd && /usr/local/bin/mirakc"]
