FROM drupal:8.6.13-apache

RUN a2enmod rewrite && \
    a2enmod ssl

RUN service apache2 restart

RUN mkdir /etc/apache2/ssl

RUN openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -keyout /etc/apache2/ssl/apache.key \
  -out /etc/apache2/ssl/apache.crt \
  -subj "/C=FR/ST=Paris/L=London/O=Global Security/OU=IT Department/CN=homly"

COPY devops/homly.local.conf /etc/apache2/sites-available/homly.conf

RUN a2ensite homly

# Install dependencies
RUN apt-get update && apt-get install -qq -y \
  git \
  mysql-client \
  unzip \
  libjpeg62-turbo-dev \
  libfreetype6-dev \
  curl

#Install nodejs
RUN curl -o /usr/local/bin/n https://raw.githubusercontent.com/visionmedia/n/master/bin/n
RUN chmod +x /usr/local/bin/n
RUN n 9

# Require composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php -r "if (hash_file('sha384', 'composer-setup.php') === '48e3236262b34d30969dca3c37281b3b4bbe3221bda826ac6a9a62d6444cdb0dcd0615698a5cbe587c3f0fe57a54d8f5') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
RUN php composer-setup.php --install-dir=/usr/bin --filename=composer
RUN php -r "unlink('composer-setup.php');"

WORKDIR /var/www/html
