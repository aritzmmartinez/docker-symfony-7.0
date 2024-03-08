FROM php:8.2-fpm

# Set UID as an environment variable
ENV UID=1000

EXPOSE $UID

# Create appuser
RUN id -u appuser 2>/dev/null || useradd --uid $UID -m -s /bin/bash appuser

# Create SSH config directory
RUN mkdir -p /home/appuser/.ssh && chown -R appuser:appuser /home/appuser/

# Configure SSH for appuser
RUN echo "StrictHostKeyChecking no" >> /home/appuser/.ssh/config
RUN echo "export COLUMNS=300" >> /home/appuser/.bashrc
RUN echo "alias sf=/appdata/www/bin/console" >> /home/appuser/.bashrc

COPY ./php.ini /usr/local/etc/php/

# Install required packages
RUN apt-get update \
    && apt-get install -y \
        git \
        acl \
        openssl \
        openssh-client \
        wget \
        zip \
        vim \
        librabbitmq-dev \
        libssh-dev \
        libpng-dev \
        zlib1g-dev \
        libzip-dev \
        libxml2-dev \
        libicu-dev \
        # Install Node.js and npm using NodeSource script
        && curl -fsSL https://deb.nodesource.com/setup_16.x | bash - \
        && apt-get install -y nodejs \
        # Clean up
        && apt-get clean \
        && rm -rf /var/lib/apt/lists/*

# Continue with other PHP extensions and tools installation
RUN docker-php-ext-install intl pdo pdo_mysql zip gd soap bcmath sockets

# Install Xdebug only if it is not already installed
RUN if [ ! -e /usr/local/lib/php/extensions/no-debug-non-zts-*/xdebug.so ]; then \
    pecl install xdebug && docker-php-ext-enable xdebug; \
fi


# Install AMQP only if it is not already installed
RUN if [ ! -e /usr/local/lib/php/extensions/no-debug-non-zts-*/amqp.so ]; then \
    pecl install amqp && docker-php-ext-enable amqp; \
fi

RUN curl --insecure https://getcomposer.org/composer.phar -o /usr/bin/composer && chmod +x /usr/bin/composer
RUN composer self-update

RUN wget https://github.com/FriendsOfPHP/PHP-CS-Fixer/releases/download/v3.2.1/php-cs-fixer.phar -O php-cs-fixer
RUN chmod a+x php-cs-fixer
RUN mv php-cs-fixer /usr/local/bin/php-cs-fixer

RUN mkdir -p /appdata/www

WORKDIR /appdata/www
