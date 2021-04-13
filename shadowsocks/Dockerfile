FROM debian:10.9 AS build

ARG SHADOWSOCKS_VERSION=3.3.5

WORKDIR /build

RUN apt-get update && apt-get install -y --no-install-recommends \
      wget \
      ca-certificates \
      autoconf \
      automake \
      libtool \
      gettext \
      build-essential \
      libpcre3-dev \
      libev-dev \
      libc-ares-dev \
      libmbedtls-dev \
      libsodium-dev \
    && rm -rf /var/lib/apt/lists/*

RUN wget -O shadowsocks-libev.tar.gz https://github.com/shadowsocks/shadowsocks-libev/releases/download/v${SHADOWSOCKS_VERSION}/shadowsocks-libev-${SHADOWSOCKS_VERSION}.tar.gz --progress=dot:giga \
    && tar -xzf shadowsocks-libev.tar.gz --strip 1 \
    && ./configure --disable-documentation \
    && make install


FROM debian:10.9 AS run

RUN apt-get update && apt-get install -y --no-install-recommends \
      libev4 \
      libsodium23 \
      libmbedtls12 \
      libpcre3 \
      libc-ares2 \
      wget \
      ca-certificates \
      rng-tools \
      tzdata \
    && rm -rf /var/lib/apt/lists/*

COPY --from=build /usr/local/bin/ss-* /usr/local/bin/

ARG V2RAY_PLUGIN_VERSION=1.3.1

RUN wget -O v2ray-plugin.tar.gz https://github.com/shadowsocks/v2ray-plugin/releases/download/v${V2RAY_PLUGIN_VERSION}/v2ray-plugin-linux-amd64-v${V2RAY_PLUGIN_VERSION}.tar.gz --progress=dot:giga \
    && tar -xzf v2ray-plugin.tar.gz \
    && mv v2ray-plugin_linux_amd64 /usr/local/bin/v2ray-plugin \
    && rm -f v2ray-plugin.tar.gz

ENV TIMEOUT=300
ENV METHOD=xchacha20-ietf-poly1305

ENV PASSWORD=
ENV DNS_SERVER=1.1.1.1
ENV V2RAY_PATH=v2ray
ENV DOMAIN=
ENV V2RAY_CERT_PATH=
ENV V2RAY_KEY_PATH=

EXPOSE 8008

USER nobody

CMD ss-server \
      -s 0.0.0.0 \
      -p 8008 \
      -k ${PASSWORD} \
      -m ${METHOD} \
      -t ${TIMEOUT} \
      -d ${DNS_SERVER} \
      --no-delay \
      --fast-open \
      --reuse-port \
      --plugin v2ray-plugin \
      --plugin-opts "server;tls;fast-open;path=/${V2RAY_PATH};host=${DOMAIN};cert=${V2RAY_CERT_PATH};key=${V2RAY_KEY_PATH};loglevel=none"
