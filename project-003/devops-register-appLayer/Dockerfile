FROM php:8.3-fpm

WORKDIR /var/www/html

# Installer les dépendances système
RUN apt-get update && apt-get install -y \
    curl \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    libzip-dev \
    zip \
    unzip \
    git \
    nano

# Installer les extensions PHP
RUN docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd zip

# Installer Composer
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

# Copier TOUTES les sources (y compris artisan)
COPY . .

# Installer les dépendances Composer
RUN composer install --no-interaction --prefer-dist --optimize-autoloader

# Générer la clé Laravel
RUN php artisan key:generate

# Configurer les permissions
RUN chown -R www-data:www-data /var/www/html/storage /var/www/html/bootstrap/cache

EXPOSE 9000

# Commande d'exécution (PHP-FPM pour un usage production-friendly)
CMD ["php-fpm"]

