FROM php:7.4-apache

# Copy virtual host into container
COPY 000-default.conf /etc/apache2/sites-available/000-default.conf

# Enable rewrite mode
RUN a2enmod rewrite

# Install necessary packages
RUN apt-get update && \
    apt-get install \
    libzip-dev \
    wget \
    git \
    unzip \
    -y --no-install-recommends

# Install PHP Extensions
RUN docker-php-ext-install zip pdo_mysql

RUN pecl install -o -f xdebug-3.1.5 \
    && docker-php-ext-enable xdebug
#     && rm -rf /tmp/pear

# Copy php.ini
COPY ./php.ini /usr/local/etc/php/

# Change the current working directory
WORKDIR /var/www

# Change the owner of the container document root
RUN chown -R www-data:www-data /var/www

# Create a new user
RUN adduser --disabled-password --gecos '' developer

# Add user to the group
RUN chown -R developer:www-data /var/www

RUN chmod 755 /var/www

# Switch to this user
USER developer

# Start Apache in foreground
CMD ["apache2-foreground"]