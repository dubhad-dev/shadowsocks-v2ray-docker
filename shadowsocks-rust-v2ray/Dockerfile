FROM rust:1.60.0 AS build

ARG SHADOWSOCKS_RUST_VERSION=1.14.3

WORKDIR /build

RUN wget -O shadowsocks-rust.tar.xz https://github.com/shadowsocks/shadowsocks-rust/archive/refs/tags/v${SHADOWSOCKS_RUST_VERSION}.tar.gz --progress=dot:giga \
    && tar -xzf shadowsocks-rust.tar.xz --strip 1 \
    && cargo build --features dns-over-tls,dns-over-https --release \
    && make install TARGET=release


FROM debian:11.3 AS run
    
COPY --from=build /usr/local/bin/ss* /usr/local/bin/
    
ARG V2RAY_PLUGIN_VERSION=1.3.1

RUN apt-get update && apt-get install -y --no-install-recommends \
      wget \
      ca-certificates \
      gettext-base \
      gosu \
    && rm -rf /var/lib/apt/lists/*

RUN wget -O v2ray-plugin.tar.gz https://github.com/shadowsocks/v2ray-plugin/releases/download/v${V2RAY_PLUGIN_VERSION}/v2ray-plugin-linux-amd64-v${V2RAY_PLUGIN_VERSION}.tar.gz --progress=dot:giga \
    && tar -xzf v2ray-plugin.tar.gz \
    && mv v2ray-plugin_linux_amd64 /usr/local/bin/v2ray-plugin \
    && rm -f v2ray-plugin.tar.gz

RUN mkdir /docker-entrypoint.d
COPY docker-entrypoint.sh /
COPY 20-envsubst-on-templates.sh /docker-entrypoint.d
ENTRYPOINT ["/docker-entrypoint.sh"]

COPY config.json.template /usr/local/etc/shadowsocks/templates/config.json.template

EXPOSE 8008

CMD ["ssserver", "-c", "/usr/local/etc/shadowsocks/config.json"]
