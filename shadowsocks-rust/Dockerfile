FROM debian:10.10

ARG SHADOWSOCKS_RUST_VERSION=1.11.1

RUN apt-get update && apt-get install -y --no-install-recommends \
      wget \
      ca-certificates \
      xz-utils \
    && rm -rf /var/lib/apt/lists/*

RUN wget -O shadowsocks-rust.tar.xz https://github.com/shadowsocks/shadowsocks-rust/releases/download/v${SHADOWSOCKS_RUST_VERSION}/shadowsocks-v${SHADOWSOCKS_RUST_VERSION}.x86_64-unknown-linux-gnu.tar.xz --progress=dot:giga \
    && tar -xJf shadowsocks-rust.tar.xz -C /usr/local/bin/ \
    && rm -f shadowsocks-rust.tar.xz

ARG V2RAY_PLUGIN_VERSION=1.3.1

RUN wget -O v2ray-plugin.tar.gz https://github.com/shadowsocks/v2ray-plugin/releases/download/v${V2RAY_PLUGIN_VERSION}/v2ray-plugin-linux-amd64-v${V2RAY_PLUGIN_VERSION}.tar.gz --progress=dot:giga \
    && tar -xzf v2ray-plugin.tar.gz \
    && mv v2ray-plugin_linux_amd64 /usr/local/bin/v2ray-plugin \
    && rm -f v2ray-plugin.tar.gz

ENV METHOD=chacha20-ietf-poly1305
ENV PASSWORD=
ENV DNS_SERVER=cloudflare
ENV V2RAY_PATH=v2ray
ENV DOMAIN=
ENV V2RAY_CERT_PATH=
ENV V2RAY_KEY_PATH=

EXPOSE 8008

CMD exec ssserver \
      --tcp-fast-open \
      --tcp-no-delay \
      --server-addr 0.0.0.0:8008 \
      --password ${PASSWORD} \
      --encrypt-method ${METHOD} \
      --dns ${DNS_SERVER} \
      --plugin v2ray-plugin \
      --plugin-opts "server;tls;fast-open;path=/${V2RAY_PATH};host=${DOMAIN};cert=${V2RAY_CERT_PATH};key=${V2RAY_KEY_PATH};loglevel=none"
