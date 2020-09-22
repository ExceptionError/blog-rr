FROM php:7.4-cli

RUN apt-get update \
  && apt-get install -y --no-install-recommends libzip-dev unzip \
  && docker-php-ext-install zip \
  && docker-php-ext-install opcache \
  && docker-php-ext-enable opcache

COPY --from=composer /usr/bin/composer /usr/bin/composer

ENV RR_VERSION 1.8.3
RUN mkdir /tmp/rr \
  && cd /tmp/rr \
  && echo "{\"require\":{\"spiral/roadrunner\":\"${RR_VERSION}\"}}" >> composer.json \
  && composer install \
  && vendor/bin/rr get-binary -l /usr/local/bin \
  && rm -rf /tmp/rr

WORKDIR /var/www

ENTRYPOINT ["/usr/local/bin/rr", "serve", "-d", "-c", "/var/www/app/.rr.yaml"]
