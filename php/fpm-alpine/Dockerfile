ARG PHP_VERSION

FROM php:${PHP_VERSION}

COPY --from=composer /usr/bin/composer /usr/bin/composer

RUN set -eux; \
    apk add --no-cache fcgi libpng-dev libxslt-dev freetype-dev icu-dev libjpeg-turbo-dev libpng-dev libtool \
                        libwebp-dev libzip-dev zlib-dev bash;

RUN set -eux; \
    docker-php-ext-install gd ctype dom exif intl pdo pdo_mysql xml xsl zip;

RUN set -eux; \
    addgroup -g 3000 docker; \
    adduser -u 3000 -G docker -h /home/docker -s /bin/sh -D docker; \
    USER=docker; \
    GROUP=docker; \
    curl -SsL https://github.com/boxboat/fixuid/releases/download/v0.5.1/fixuid-0.5.1-linux-amd64.tar.gz | tar -C /usr/local/bin -xzf -; \
    chown root:root /usr/local/bin/fixuid; \
    chmod 4755 /usr/local/bin/fixuid; \
    mkdir -p /etc/fixuid; \
    printf "user: $USER\ngroup: $GROUP\n" > /etc/fixuid/config.yml; \

RUN curl -sS https://get.symfony.com/cli/installer | bash; \
    mv /root/.symfony/bin/symfony /usr/local/bin/symfony; \
    chmod +x /usr/local/bin/symfony; \
    chown $USER:$GROUP /usr/local/bin/symfony;

USER docker:docker

WORKDIR /app

ENTRYPOINT ["fixuid"]
CMD ["php-fpm"]
