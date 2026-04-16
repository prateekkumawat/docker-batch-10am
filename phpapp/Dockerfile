FROM php:8.1-apache

WORKDIR /var/www/html

COPY src/ .

RUN docker-php-ext-install mysqli pdo pdo_mysql

EXPOSE 80

CMD ["apache2-foreground"]