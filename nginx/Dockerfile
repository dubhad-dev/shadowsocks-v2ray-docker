FROM nginx:1.19.2

ENV NGINX_ENVSUBST_OUTPUT_DIR=/etc/nginx

ENV DNS_SERVER=1.1.1.1
ENV DOMAIN=
ENV V2RAY_HOST=
ENV SERVER_PORT=8008
ENV V2RAY_PATH=v2ray
ENV V2RAY_CERT_PATH=
ENV V2RAY_KEY_PATH=
ENV DHPARAM_PATH=/etc/nginx/ssl/dhparams.pem
ENV REDIRECT_URL=https://youtube.com
ENV NOFILE_LIMIT=131072

COPY nginx.conf.template /etc/nginx/templates/nginx.conf.template

COPY blockips.conf /etc/nginx/blockips.conf

EXPOSE 443