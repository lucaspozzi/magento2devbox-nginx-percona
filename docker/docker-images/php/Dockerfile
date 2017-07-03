FROM php:7.0.20-fpm

RUN apt-get update

# Install iconv, mcryot, gd
RUN apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng12-dev \
        git \
        mysql-client \
        libxml2-dev \
    && docker-php-ext-install -j$(nproc) iconv mcrypt \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd

## Install memcached
RUN apt-get install -y libmemcached-dev \
  && git clone https://github.com/php-memcached-dev/php-memcached /usr/src/php/ext/memcached \
  && cd /usr/src/php/ext/memcached && git checkout -b php7 origin/php7 \
  && docker-php-ext-configure memcached \
  && docker-php-ext-install memcached

## Install intl
RUN apt-get install -y libicu-dev zlib1g-dev g++ \
    && docker-php-ext-configure intl \
    && docker-php-ext-install intl

# Install mbstring
RUN docker-php-ext-install mbstring

# Install curl
RUN apt-get install -y libcurl4-openssl-dev \
    && docker-php-ext-install curl

# Install zip
RUN docker-php-ext-install zip

# Install soap
RUN docker-php-ext-install soap

# Install json
RUN docker-php-ext-install json

# Install extensions through the scripts the container provides
# Here we install the pdo_mysql extensions to access MySQL.
RUN docker-php-ext-install pdo_mysql

# Install XSL
RUN apt-get install -y libxslt-dev
RUN docker-php-ext-install xsl
RUN docker-php-ext-configure xsl

# Install bcmath
RUN docker-php-ext-install bcmath

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --filename=composer --install-dir="/bin"

RUN usermod -u 1000 www-data
RUN echo 'date.timezone="GMT"' >> /usr/local/etc/php/conf.d/date.ini
RUN echo 'opcache.enable=1' >> /usr/local/etc/php/conf.d/opcache.conf
RUN echo 'opcache.validate_timestamps=0' >> /usr/local/etc/php/conf.d/opcache.conf
RUN echo 'opcache.fast_shutdown=1' >> /usr/local/etc/php/conf.d/opcache.conf

RUN mkdir /mail/log

WORKDIR /var/www/html

#Install Magento
#RUN cd /var/www/html && composer update && php bin/magento setup:install --base-url=http://localhost.internationalvapor.com/ --db-host=db --db-name=magento2 --db-user=root --db-password=root --admin-firstname=Magento --admin-lastname=User --admin-email=user@example.com --admin-user=admin --admin-password=admin123 --language=en_US --currency=USD --timezone=America/Chicago --use-rewrites=1 --backend-frontname=admin
