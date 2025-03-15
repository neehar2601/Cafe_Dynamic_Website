# FROM php:7.4-apache
# RUN sed -i 's|http://deb.debian.org/debian/|http://ftp.debian.org/debian/|g' /etc/apt/sources.list
# RUN apt-get update -y
# RUN docker-php-ext-install mysqli
# COPY . /var/www/html/
# EXPOSE 80



FROM php:7.4-apache

# Update Debian repository URL and install necessary extensions
RUN sed -i 's|http://deb.debian.org/debian/|http://ftp.debian.org/debian/|g' /etc/apt/sources.list \
    && apt-get update -y \
    && docker-php-ext-install mysqli

# Create argo-rollouts directory and copy contents into it
RUN mkdir -p /var/www/html/argo-rollouts
COPY . /var/www/html/argo-rollouts

# Update Apache configuration
RUN sed -i '/DocumentRoot/c\DocumentRoot /var/www/html' /etc/apache2/sites-available/000-default.conf \
    && echo 'Alias "/argo-rollouts" "/var/www/html/argo-rollouts"' >> /etc/apache2/sites-available/000-default.conf \
    && echo '<Directory "/var/www/html/argo-rollouts">' >> /etc/apache2/sites-available/000-default.conf \
    && echo 'Require all granted' >> /etc/apache2/sites-available/000-default.conf \
    && echo '</Directory>' >> /etc/apache2/sites-available/000-default.conf

# Ensure Apache runs in the foreground
CMD ["apache2-foreground"]

EXPOSE 80

