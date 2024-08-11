# syntax=docker.io/docker/dockerfile:1

ARG BASE_IMAGE=docker.io/l3tnun/epgstation:v2.10.0-debian

FROM $BASE_IMAGE AS sqlite3-pcre-build

RUN apt-get update && apt-get install -y build-essential pkg-config libsqlite3-dev libpcre3-dev

RUN --mount=type=bind,source=./sqlite3-pcre,target=/tmp/sqlite3-pcre,rw \
  cd /tmp/sqlite3-pcre && \
  make -j $(nproc) && make install

FROM $BASE_IMAGE
COPY --from=sqlite3-pcre-build /usr/lib/sqlite3/pcre.so /usr/local/lib/sqlite3/pcre.so

# Enable non-free repository to install intel-media-va-driver-non-free
RUN sed -i -e 's/^Components: main$/Components: main contrib non-free non-free-firmware/' /etc/apt/sources.list.d/debian.sources

RUN apt-get update && apt-get install -y libpcre3 ffmpeg libavcodec-extra intel-media-va-driver-non-free

ENTRYPOINT ["docker-entrypoint.sh"]
CMD ["npm", "start"]
