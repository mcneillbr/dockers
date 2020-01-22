FROM alpine:3.6
# maintainer "Adrian B. Danieli - https://github.com/sickp"
LABEL redis_3.2 "Adrian B. Danieli - https://github.com/sickp"

ENV REDIS_VERSION=3.2.9

COPY ./rootfs/usr/local/bin/about /usr/local/bin/about
COPY ./rootfs/entrypoint.sh /bin/entrypoint.sh

COPY ./archives/gosu-amd64 /usr/local/bin/gosu
COPY ./archives/redis.3.2.9.tar.gz /tmp/${REDIS_VERSION}.tar.gz

RUN chmod +x /bin/entrypoint.sh
RUN chmod +x /usr/local/bin/about

RUN set -exo pipefail \
    && apk add --no-cache --virtual .build-deps build-base linux-headers openssl \
    && chmod +x /usr/local/bin/gosu \
    && cd /tmp \
    && tar xzf ${REDIS_VERSION}.tar.gz \
    && cd /tmp/redis-${REDIS_VERSION} \
    && make \
    && make install \
    && cp redis.conf /etc/redis.conf \
    && sed -i -e 's/bind 127.0.0.1/bind 0.0.0.0/' /etc/redis.conf \
    && adduser -D redis \
    && apk del .build-deps \
    && rm -rf /tmp/*

EXPOSE 6379
ENTRYPOINT ["/bin/entrypoint.sh"]
CMD ["redis-server", "/etc/redis.conf"]
VOLUME ["/data"]
WORKDIR /data

