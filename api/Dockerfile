# Reference Architecture: R2Upload API Core
FROM php:8.4-fpm-alpine

RUN apk add --no-cache \
    postgresql-dev \
    libzip-dev \
    unzip \
    && docker-php-ext-install pdo pdo_pgsql zip opcache pcntl

RUN apk add --no-cache pcre-dev $PHPIZE_DEPS \
    && pecl install redis \
    && docker-php-ext-enable redis \
    && apk del $PHPIZE_DEPS

WORKDIR /var/www/html
COPY . .

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer
RUN composer install --no-dev --optimize-autoloader --no-interaction

RUN chown -R www-data:www-data /var/www/html/storage

RUN php artisan config:cache \
    && php artisan route:cache \
    && php artisan event:cache

EXPOSE 9000
CMD ["php-fpm"]
