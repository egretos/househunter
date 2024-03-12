FROM php:8.2-fpm

RUN apt update && apt install -y \
    build-essential \
    default-mysql-client \
    libpng-dev \
    libicu-dev \
    libxml2-dev \
    libmagickwand-dev \
    libjpeg62-turbo-dev \
    libfreetype6-dev \
    locales \
    zip \
    jpegoptim optipng pngquant gifsicle \
    vim \
    unzip \
    git \
    curl \
    nodejs \
    npm

# Install extensions
RUN pecl install imagick
RUN docker-php-ext-enable imagick
RUN docker-php-ext-install pdo_mysql
RUN docker-php-ext-install exif
RUN docker-php-ext-install pcntl
RUN docker-php-ext-install intl
RUN docker-php-ext-install bcmath
RUN docker-php-ext-configure gd --enable-gd --with-freetype --with-jpeg --with-webp
RUN docker-php-ext-install gd
RUN pecl install xdebug \
    && docker-php-ext-enable xdebug

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

#in some cases we will be needed to pass this out of docker-compose.yml
ARG USER_ID=1000
ARG GROUP_ID=1000

RUN groupadd --gid ${GROUP_ID} user \
    && adduser --disabled-password --gecos '' --uid ${USER_ID} --gid ${GROUP_ID} user

USER user

WORKDIR /var/www
EXPOSE 9000
CMD ["php-fpm"]