# Dockerfile.php-apache - SISCAPACITEC
FROM php:8.4.14-apache

ENV TZ="America/Sao_Paulo"

# Instala Git + dependências e extensões PHP
RUN apt-get update && apt-get install -y --no-install-recommends \
    git \
    unzip \
    libpq-dev \
    libzip-dev \
    libpng-dev \
    libonig-dev \
    zlib1g-dev \
    libicu-dev \
    libxml2-dev \
    && docker-php-ext-install pdo pdo_mysql zip gd mbstring intl xml \
    && pecl install redis \
    && docker-php-ext-enable redis \
    && rm -rf /var/lib/apt/lists/*

# Instala Composer global
COPY --from=composer/composer:latest-bin /composer /usr/local/bin/composer
RUN composer --version

# Habilita mod_rewrite do Apache
RUN a2enmod rewrite

# Configura DocumentRoot para Laravel
RUN sed -i 's|/var/www/html|/var/www/public|g' /etc/apache2/sites-available/000-default.conf
RUN sed -i 's|/var/www/html|/var/www/public|g' /etc/apache2/apache2.conf

# Configura permissões
WORKDIR /var/www
RUN chown -R www-data:www-data /var/www

# Cria diretórios necessários do Laravel
RUN mkdir -p storage/{logs,framework/{cache,sessions,views}} bootstrap/cache \
    && chown -R www-data:www-data storage bootstrap/cache \
    && chmod -R 775 storage bootstrap/cache

EXPOSE 80

CMD ["apache2-foreground"]
