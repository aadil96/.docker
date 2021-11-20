FROM composer:2.1.2 as vendor

COPY database/ composer.json composer.lock ./

RUN composer install \
    --no-interaction \
    --no-plugins \
    --no-scripts \
    --no-dev \
    --prefer-dist


FROM node:14.18.1 as modules

WORKDIR /app
RUN mkdir ./public

COPY package.json webpack.mix.js ./

COPY resources/js ./resources/js
COPY resources/css ./resources/css

RUN npm install
RUN npm run dev

FROM php:7.4-fpm

RUN docker-php-ext-install pdo pdo_mysql && \
    docker-php-ext-install mysqli && docker-php-ext-enable mysqli

RUN groupadd -g 1000 www && useradd -u 1000 -ms /bin/bash -g www www

COPY --chown=www:www . /var/www/html
COPY --from=vendor app/vendor/ ./vendor/
COPY --from=modules /app/node_modules/ ./node_modules/
COPY --from=modules app/public/css ./public/css
COPY --from=modules app/public/js ./public/js

USER www

EXPOSE 9000

CMD ["php-fpm"]
