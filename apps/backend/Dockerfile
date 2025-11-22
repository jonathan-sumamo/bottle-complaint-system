FROM php:8.3-alpine

RUN apk add --no-cache \
    curl \
    zip \
    unzip \
    git \
    libpq \
    libzip-dev \
    oniguruma-dev \
    postgresql-dev

RUN docker-php-ext-install pdo_pgsql mbstring zip

COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

WORKDIR /var/www/html

COPY . .

RUN composer install --no-dev --prefer-dist --no-interaction --optimize-autoloader
RUN cp .env.example .env || true
RUN php artisan key:generate

EXPOSE 10000

CMD php artisan serve --host=0.0.0.0 --port=10000

