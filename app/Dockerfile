FROM php:8.3-apache

# --- Dépendances système ---
RUN set -eux; \
    apt-get update; \
    apt-get install -y --no-install-recommends \
      git unzip pkg-config \
      libicu-dev libzip-dev libsqlite3-dev \
      libjpeg62-turbo-dev libpng-dev libfreetype6-dev; \
    docker-php-ext-configure gd --with-freetype --with-jpeg; \
    docker-php-ext-install -j"$(nproc)" intl pdo_mysql pdo_sqlite zip gd opcache; \
    a2enmod rewrite headers; \
    rm -rf /var/lib/apt/lists/*

# --- Composer global ---
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer
ENV COMPOSER_ALLOW_SUPERUSER=1

# --- Config Apache ---
ENV APACHE_DOCUMENT_ROOT=/var/www/html/public
RUN sed -ri 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' \
      /etc/apache2/sites-available/000-default.conf \
      /etc/apache2/apache2.conf

WORKDIR /var/www/html

# Pas de COPY du code ici :
# le code Symfony sera monté par un volume (../symfony-simple-app)

EXPOSE 80

CMD ["apache2-foreground"]
