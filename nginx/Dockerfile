FROM nginx:1.21.0

ENV NGINX_ENVSUBST_OUTPUT_DIR=/etc/nginx

ENV DNS_SERVER=1.1.1.1
ENV DOMAIN=
ENV V2RAY_HOST=
ENV V2RAY_PORT=8008
ENV V2RAY_PATH=v2ray
ENV V2RAY_CERT_PATH=
ENV V2RAY_KEY_PATH=
ENV REDIRECT_URL=https://youtube.com

COPY nginx.conf.template /etc/nginx/templates/nginx.conf.template

COPY blockips.conf /etc/nginx/blockips.conf

EXPOSE 443
