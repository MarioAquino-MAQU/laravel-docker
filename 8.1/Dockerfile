FROM php:8.1-fpm

# Install production dependencies
RUN apt-get update && apt-get install -y \
    bash \
    curl \
    default-mysql-client \
    dpkg \
    g++ \
    gcc \
    git \
    #icu-dev \
    #icu-libs \
    #imagemagick \
    libcurl4-openssl-dev \
    libmagickwand-dev \
    libmagickcore-dev \
    libc-dev \
    #libturbojpeg \
    libpng-dev \
    libzip-dev \
    libfreetype6-dev \
    libonig-dev \
    make \
    nodejs \
    yarn \
    openssh-client \
    #postgresql-libs \
    postgresql-common \
    postgresql-server-dev-all \
    libsqlite3-dev \
    rsync \
    zlib1g-dev \
    odbcinst \
    odbcinst1debian2 \
    libodbc1

#ODBC config...
RUN apt-get install unixodbc unixodbc-dev -y \
 && docker-php-ext-configure pdo_odbc --with-pdo-odbc=unixODBC,/usr \
 && docker-php-ext-install pdo_odbc

COPY ./ibm-iaccess-1.1.0.14-1.0.amd64.deb /ibm-iaccess-1.1.0.14-1.0.amd64.deb
RUN dpkg -i /ibm-iaccess-1.1.0.14-1.0.amd64.deb \
    && rm /ibm-iaccess-1.1.0.14-1.0.amd64.deb


# Install PECL and PEAR extensions
RUN pecl install \
    redis \
    imagick \
    xdebug

# Enable PECL and PEAR extensions
RUN docker-php-ext-enable \
    redis \
    imagick \
    xdebug

## Configure php extensions
RUN docker-php-ext-configure gd --with-freetype --with-jpeg

## Install php extensions
RUN docker-php-ext-install \
    bcmath \
    calendar \
    curl \
    exif \
    gd \
    iconv \
    intl \
    mbstring \
    pdo \
    pdo_mysql \
    pdo_pgsql \
    pdo_sqlite \
    pcntl \
    soap \
    sockets \
    tokenizer \
    xml \
    zip

# Install composer
ENV COMPOSER_HOME /composer
ENV PATH ./vendor/bin:/composer/vendor/bin:$PATH
ENV COMPOSER_ALLOW_SUPERUSER 1
RUN curl -s https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin/ --filename=composer

# Install PHP_CodeSniffer
RUN composer global require "squizlabs/php_codesniffer=*"

# Cleanup dev dependencies


# Setup working directory
WORKDIR /var/www
