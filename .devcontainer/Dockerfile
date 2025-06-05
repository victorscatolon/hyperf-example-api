FROM hyperf/hyperf:8.3-alpine-v3.19-swoole

ARG timezone
ENV TIMEZONE=${timezone:-"America/Sao_Paulo"} \
    APP_ENV=dev \
    SCAN_CACHEABLE=(false)

RUN set -ex \
    && apk update \
    && apk add --no-cache \
    autoconf \
    g++ \
    make \
    php83-dev \
    php83-pear \
    linux-headers \
    curl \
    && ln -sf /usr/bin/pecl83 /usr/local/bin/pecl \
    && ln -sf /usr/bin/phpize83 /usr/local/bin/phpize \
    && pecl install xdebug \
    \
    && echo "zend_extension=$(find /usr/lib/php*/modules/ -name xdebug.so)" > /etc/php83/conf.d/50_xdebug.ini \
    && echo "xdebug.mode=debug" >> /etc/php83/conf.d/50_xdebug.ini \
    && echo "xdebug.start_with_request=yes" >> /etc/php83/conf.d/50_xdebug.ini \
    && echo "xdebug.discover_client_host=true" >> /etc/php83/conf.d/50_xdebug.ini \
    && echo "xdebug.client_port=9003" >> /etc/php83/conf.d/50_xdebug.ini \
    \
    && echo "upload_max_filesize=128M" >> /etc/php83/conf.d/99_overrides.ini \
    && echo "post_max_size=128M" >> /etc/php83/conf.d/99_overrides.ini \
    && echo "memory_limit=2G" >> /etc/php83/conf.d/99_overrides.ini \
    && echo "date.timezone=${TIMEZONE}" >> /etc/php83/conf.d/99_overrides.ini \
    \
    && ln -sf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime \
    && echo "${TIMEZONE}" > /etc/timezone \
    \
    && rm -rf /var/cache/apk/* /tmp/* /usr/share/man \
    && echo -e "\033[42;37m Build Completed :).\033[0m\n"

WORKDIR /opt/www
COPY . /opt/www
RUN composer install --no-scripts --no-interaction --prefer-dist

EXPOSE 9501 9003
