FROM php:5.6-fpm

RUN apt-get update && apt-get install -y \
        bzip2 \
        bsdtar \
        libcurl4-openssl-dev \
        libpng12-dev \
        libmagickwand-dev \
        libicu-dev \
        libldap2-dev

RUN docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/ \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install mysql json curl gd intl pdo_mysql ldap ctype xml \
    && pecl install imagick-3.3.0RC2 \
    && pecl install apcu-4.0.10 \
    && pear channel-discover pear.symfony.com \
    && pear channel-discover pear.doctrine-project.org \
    && pear channel-discover pear.twig-project.org \
    && pear install pear.doctrine-project.org/DoctrineORM \
            pear.doctrine-project.org/DoctrineSymfonyYaml \
            pear.doctrine-project.org/DoctrineSymfonyConsole \
            twig/Twig

COPY php.ini /usr/local/etc/php/php.ini
RUN pear config-set php_ini /usr/local/etc/php/php.ini \
    && pecl config-set php_ini /usr/local/etc/php/php.ini

ENV PK_VERSION 1.4.0

RUN cd /var/www \
    && curl -sL https://downloads.partkeepr.org/partkeepr-${PK_VERSION}.tbz2 | bsdtar -xf- \
    && mv partkeepr-${PK_VERSION} partkeepr \
    && chown -R www-data:www-data /var/www/partkeepr

VOLUME /var/www/partkeepr
WORKDIR /var/www/partkeepr

ADD config.php /var/www/partkeepr
