# Use PHP 8.2
FROM php:8.2-fpm

# Install common php extension dependencies and MySQL PDO
RUN apt-get update && apt-get install -y \
    libfreetype-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    zlib1g-dev \
    libzip-dev \
    unzip \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install zip \
    && docker-php-ext-install pdo_mysql

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Set the working directory
WORKDIR /var/www/app

# Copy application files
COPY . /var/www/app

# Ensure the necessary directories are writable and have correct ownership
RUN chown -R www-data:www-data /var/www/app \
    && chmod -R 775 /var/www/app/storage \
    && chmod -R 775 /var/www/app/bootstrap/cache

# Copy composer.json and install dependencies
COPY composer.json ./
RUN composer install

# Set the default command to run php-fpm
CMD ["php-fpm"]
