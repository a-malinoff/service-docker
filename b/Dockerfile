FROM php:8.1.2-fpm
MAINTAINER shad541@gmail.com

# Install modules

RUN apt-get update && apt-get install -y \
    procps \
    apt-utils \
    libxml2-dev \
    libpng-dev \
    libicu-dev \
    libc-client-dev \
    zlib1g-dev \
    libzip-dev \
    libonig-dev \
    libpq-dev \
    iputils-ping \
    --no-install-recommends

RUN docker-php-ext-enable opcache
RUN docker-php-ext-configure intl
RUN docker-php-ext-install zip mbstring intl xml soap pdo_pgsql pgsql bcmath

RUN apt-get purge -y g++  && apt-get autoremove -y && rm -r /var/lib/apt/lists/* && rm -rf /tmp/*

ADD ./php.ini /usr/local/etc/php/
RUN sed -i 's/user = www-data/;user = www-data/g' /usr/local/etc/php-fpm.d/www.conf \
    && sed -i 's/group = www-data/;group = www-data/g' /usr/local/etc/php-fpm.d/www.conf 

RUN useradd --uid 1000 --home /home/user user

RUN mkdir /home/user && chown -R user:user /home/user

RUN curl -1sLf 'https://dl.cloudsmith.io/public/symfony/stable/setup.deb.sh' | bash
RUN apt install -y symfony-cli

USER user
WORKDIR /app
