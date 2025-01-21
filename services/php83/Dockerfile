ARG PHP_VERSION=php:8.3-fpm-alpine
FROM ${PHP_VERSION}

ARG TZ
ARG PHP_EXTENSIONS
ARG CONTAINER_PACKAGE_URL
ENV TZ=${TZ}

RUN if [ "${CONTAINER_PACKAGE_URL}" != "" ] ; then sed -i "s/dl-cdn.alpinelinux.org/${CONTAINER_PACKAGE_URL}/g" /etc/apk/repositories ; fi

RUN apk --no-cache add tzdata curl \
    && cp "/usr/share/zoneinfo/$TZ" /etc/localtime \
    && echo "$TZ" > /etc/timezone

# Install PHP extensions
RUN curl -sSLf -o /usr/local/bin/install-php-extensions https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions \
&& chmod +x /usr/local/bin/install-php-extensions \
&& install-php-extensions $PHP_EXTENSIONS

# Install composer and change it's cache home
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
ENV COMPOSER_HOME=/tmp/composer

# php image's www-data user uid & gid are 82, change them to 1000 (primary user)
RUN apk --no-cache add shadow && usermod -u 1000 www-data && groupmod -g 1000 www-data

# Set work directory
WORKDIR /www
