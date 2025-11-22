FROM php:8.3-fpm-alpine

# Install system dependencies
RUN apk add --no-cache \
    curl \
    git \
    zip \
    unzip \
    oniguruma-dev \
    libxml2-dev \
    icu-dev \
    libzip-dev \
    postgresql-dev

# Install PHP extensions required by Laravel
RUN docker-php-ext-install \
    pdo_pgsql \
    mbstring \
    zip \
    bcmath \
    intl

# Install Composer
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

# Set working directory
WORKDIR /var/www/html

# Copy project files
COPY . .

# Install dependencies
RUN composer install --no-dev --prefer-dist --no-interaction --optimize-autoloader

# Generate key (will ignore if .env not set)
RUN cp .env.example .env || true
RUN php artisan key:generate || true

# Expose Render's required port
EXPOSE 10000

# Run Laravel using PHP built-in server
CMD php artisan serve --host=0.0.0.0 --port=10000