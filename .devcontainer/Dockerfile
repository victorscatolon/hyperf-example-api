# Dev Container Dockerfile - adaptado com Xdebug para GitHub Codespaces

FROM hyperf/hyperf:8.3-alpine-v3.19-swoole
LABEL maintainer="Hyperf Developers <group@hyperf.io>" version="1.0" license="MIT" app.name="Hyperf"

##
# ---------- env settings ----------
##
ARG timezone
ENV TIMEZONE=${timezone:-"America/Sao_Paulo"} \
    APP_ENV=dev \
    SCAN_CACHEABLE=(false)

# Instala dependências adicionais + Xdebug
RUN set -ex \
    && apk update \
    && apk add --no-cache $PHPIZE_DEPS curl \
    && pecl install xdebug \
    && docker-php-ext-enable xdebug \
    \
    # Configurações Xdebug
    && echo "zend_extension=xdebug.so" >> /etc/php8/conf.d/50_xdebug.ini \
    && echo "xdebug.mode=debug" >> /etc/php8/conf.d/50_xdebug.ini \
    && echo "xdebug.start_with_request=yes" >> /etc/php8/conf.d/50_xdebug.ini \
    && echo "xdebug.discover_client_host=true" >> /etc/php8/conf.d/50_xdebug.ini \
    && echo "xdebug.client_port=9003" >> /etc/php8/conf.d/50_xdebug.ini \
    \
    # Configurações PHP gerais
    && { \
    echo "upload_max_filesize=128M"; \
    echo "post_max_size=128M"; \
    echo "memory_limit=1G"; \
    echo "date.timezone=${TIMEZONE}"; \
    } | tee /etc/php8/conf.d/99_overrides.ini \
    \
    # Timezone do sistema
    && ln -sf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime \
    && echo "${TIMEZONE}" > /etc/timezone \
    \
    # Limpeza
    && rm -rf /var/cache/apk/* /tmp/* /usr/share/man \
    && echo -e "\033[42;37m Build Completed :).\033[0m\n"

# Pasta de trabalho
WORKDIR /opt/www

# Copia e instala dependências PHP
COPY . /opt/www
RUN composer install --no-scripts --no-interaction --prefer-dist

EXPOSE 9501 9003
