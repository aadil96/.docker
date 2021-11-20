FROM nginx:1.21

COPY .docker/nginx/nginx.conf /etc/nginx/nginx.conf
COPY .docker/nginx/conf.d/default.conf /etc/nginx/conf.d/
COPY . /var/www/html

ARG DOMAIN=localhost

ENV SERVER_NAME $DOMAIN

RUN sed -i "s/_placeholder/$SERVER_NAME/g" /etc/nginx/conf.d/default.conf
