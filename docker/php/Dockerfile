FROM php:8.1.0-fpm

WORKDIR /var/www/html
RUN docker-php-ext-install mysqli
RUN docker-php-ext-install pdo_mysql

# Install ext-imap
RUN apt-get update && apt-get install -y libc-client-dev libkrb5-dev && rm -r /var/lib/apt/lists/*
RUN docker-php-ext-configure imap --with-kerberos --with-imap-ssl \
    && docker-php-ext-install imap

# Install ext-zip
RUN apt-get update && apt-get install -y \
    zlib1g-dev \
    libzip-dev
RUN docker-php-ext-install zip

# Install ext-gd
RUN apt-get update && apt-get install -y \
            libfreetype6-dev \
            libjpeg62-turbo-dev \
            libpng-dev \
        && docker-php-ext-configure gd --with-freetype --with-jpeg \
        && docker-php-ext-install -j$(nproc) gd

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN groupadd -g 1000 cristiano \
&& useradd -r -u 1000 -g 1000 cristiano \
&& usermod -a -G cristiano www-data \
&& usermod -a -G www-data cristiano

RUN chown -R cristiano:cristiano /var/www/html

# Install git
RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y git

# Install nano
RUN apt-get update
RUN apt-get install -y nano

EXPOSE 9000