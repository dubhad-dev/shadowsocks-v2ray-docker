FROM debian:10.4 AS build

ARG UNBOUND_VERSION=1.11.0

WORKDIR /build

RUN apt-get update && apt-get install -y --no-install-recommends \
      wget \
      ca-certificates \
      build-essential \
      libssl-dev \
      libexpat1-dev \
      libldns-dev \
      libevent-dev \
    && rm -rf /var/lib/apt/lists/*

RUN wget -O unbound.tar.gz https://nlnetlabs.nl/downloads/unbound/unbound-${UNBOUND_VERSION}.tar.gz --progress=dot:giga \
    && tar -xzf unbound.tar.gz --strip 1 \
    && ./configure --with-libevent \
    && make install


FROM debian:10.4 AS run

RUN apt-get update && apt-get install -y --no-install-recommends \
      wget \
      ca-certificates \
      openssl \
      libevent-2.1-6 \
      libexpat1 \
      gettext-base \
    && rm -rf /var/lib/apt/lists/*

COPY --from=build /usr/local/sbin/unbound* /usr/local/sbin/
COPY --from=build /usr/local/lib/libunbound* /usr/local/lib/
RUN ldconfig

WORKDIR /usr/local/etc/unbound

RUN useradd -Urs /usr/bin/nologin unbound

RUN wget -O root.hints https://www.internic.net/domain/named.cache --progress=dot:giga

COPY unbound.conf.template ./templates/unbound.conf.template

RUN unbound-anchor -a root.key || :

ENV THREADS=1
ENV SLABS=1

RUN mkdir /docker-entrypoint.d
COPY docker-entrypoint.sh /
COPY 20-envsubst-on-templates.sh /docker-entrypoint.d
ENTRYPOINT ["/docker-entrypoint.sh"]

EXPOSE 53/tcp
EXPOSE 53/udp

CMD ["unbound"]