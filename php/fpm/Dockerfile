ARG PHP_VERSION

FROM php:${PHP_VERSION}

COPY --from=composer /usr/bin/composer /usr/bin/composer

RUN set -eux; \
    apt update -y; \
    apt install -y libfcgi-bin libpng-dev libxslt-dev libfreetype6-dev libicu-dev libjpeg62-turbo-dev libpng-dev libtool \
                    libwebp-dev libzip-dev zlib1g; \
    apt clean;

RUN curl -sS https://get.symfony.com/cli/installer | bash; \
    mv /root/.symfony/bin/symfony /usr/local/bin/symfony; \
    chmod +x /usr/local/bin/symfony;

RUN set -eux; \
    docker-php-ext-install gd ctype dom exif intl pdo pdo_mysql xml xsl zip;

WORKDIR /app
