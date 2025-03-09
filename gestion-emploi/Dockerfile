# Étape 1 : Utiliser une image PHP avec Apache
FROM php:8.2-apache

# Étape 2 : Installer les extensions PHP nécessaires
RUN docker-php-ext-install pdo pdo_mysql

# Étape 3 : Définir le répertoire de travail
WORKDIR /var/www/html

# Étape 4 : Copier les fichiers Laravel dans le conteneur
COPY . /var/www/html

# Étape 5 : Installer Composer et les dépendances Laravel
RUN apt-get update && apt-get install -y unzip curl git \
    && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
    && composer install --no-dev --optimize-autoloader

# Étape 6 : Donner les permissions à Laravel
RUN chown -R www-data:www-data /var/www/html/storage /var/www/html/bootstrap/cache

# Étape 7 : Exposer le port 80
EXPOSE 8005

# Étape 8 : Lancer Apache au démarrage
CMD ["apache2-foreground"]
