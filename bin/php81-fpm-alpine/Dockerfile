FROM php:8.1-fpm-alpine

RUN apk update; \
    apk upgrade;

RUN apk add \
    $PHPIZE_DEPS \
    bash \
    curl \
    ca-certificates \
    openssl \
    git \
    tzdata \
    openntpd \
    zip \
    unzip \
    dialog \
    nano \
    wget \
    mysql-client \
    build-base \
    gnupg \
    musl-locales \
    nodejs npm \
    composer     
    # curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
    
# Workarround until imagick is available in pecl with php8 support
# Imagick Commit to install
# https://github.com/Imagick/imagick
# ARG IMAGICK_COMMIT="661405abe21d12003207bc8eb0963fafc2c02ee4"

# RUN cd /var/tmp && \
#     git clone https://github.com/Imagick/imagick && \
#     cd imagick && \
#     git checkout ${IMAGICK_COMMIT} && \
#     phpize && \
#     ./configure && \
#     make && \
#     make install && \
#     cd .. && \
#     rm -rf imagick && \
#     docker-php-ext-enable imagick

# install pecl extension
RUN pecl install ds && \
    # pecl install imagick && \
    pecl install igbinary && \
    # pecl install memcached && \
    # pecl install mcrypt && \
    docker-php-ext-enable ds igbinary

# install xdebug
# RUN pecl install xdebug && docker-php-ext-enable xdebug && mkdir /var/log/xdebug

# Install redis
RUN pecl install redis-5.3.3 && \
    docker-php-ext-enable redis

RUN pecl install mongodb && docker-php-ext-enable mongodb

RUN yes "" | pecl install msgpack && \
    docker-php-ext-enable msgpack

# install APCu
RUN pecl install apcu && \
    docker-php-ext-enable apcu --ini-name docker-php-ext-10-apcu.ini

# RUN docker-php-ext-configure imap --with-kerberos --with-imap-ssl && \
#     docker-php-ext-install imap iconv

ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/

RUN chmod +x /usr/local/bin/install-php-extensions
    # install-php-extensions gd xdebug

# install GD
# RUN docker-php-ext-configure gd \
#     --with-jpeg \
#     --with-xpm \
#     --with-webp \
#     --with-freetype \
    # && docker-php-ext-install -j$(nproc) gd

RUN install-php-extensions gd imap iconv pdo_dblib

# fix for docker-php-ext-install pdo_dblib
# https://stackoverflow.com/questions/43617752/docker-php-and-freetds-cannot-find-freetds-in-know-installation-directories
# RUN ln -s /usr/lib/x86_64-linux-gnu/libsybdb.so /usr/lib/
# RUN docker-php-ext-install pdo_dblib

RUN install-php-extensions bcmath bz2 calendar ctype curl dba dom
RUN install-php-extensions fileinfo exif ftp gettext gmp
RUN install-php-extensions intl ldap mbstring mysqli
RUN install-php-extensions opcache pcntl pspell
RUN install-php-extensions pdo_mysql pdo_pgsql pdo_sqlite pgsql phar posix
RUN install-php-extensions session shmop simplexml soap sockets sodium
RUN install-php-extensions sysvmsg sysvsem sysvshm
RUN install-php-extensions snmp
RUN install-php-extensions xml xsl
RUN install-php-extensions tidy zip
RUN install-php-extensions filter

# install MSSQL support and ODBC driver
# RUN curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add - && \
# 	curl https://packages.microsoft.com/config/debian/8/prod.list > /etc/apt/sources.list.d/mssql-release.list && \
# 	export DEBIAN_FRONTEND=noninteractive && apt-get update -y && \
# 	ACCEPT_EULA=Y apt-get install -y msodbcsql unixodbc-dev
# RUN set -xe \
# 	&& pecl install pdo_sqlsrv \
# 	&& docker-php-ext-enable pdo_sqlsrv \
# 	&& apt-get purge -y unixodbc-dev && apt-get autoremove -y && apt-get clean

# RUN docker-php-ext-configure spl && docker-php-ext-install spl

# set locale to utf-8
# RUN echo "en_US.UTF-8 UTF-8" > /etc/locale.gen && locale-gen
# ENV LANG='en_US.UTF-8' LANGUAGE='en_US:en' LC_ALL='en_US.UTF-8'

#--------------------------------------------------------------------------
# Final Touches
#--------------------------------------------------------------------------

# install required libs for health check
# RUN apt-get -y install libfcgi0ldbl nano htop iotop lsof cron mariadb-client redis-tools wget

# install composer
# RUN EXPECTED_CHECKSUM="$(wget -q -O - https://composer.github.io/installer.sig)" && \
#     php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
#     ACTUAL_CHECKSUM="$(php -r "echo hash_file('sha384', 'composer-setup.php');")" && \
#     if [ "$EXPECTED_CHECKSUM" != "$ACTUAL_CHECKSUM" ]; then >&2 echo 'ERROR: Invalid installer checksum' && exit 1; fi

# RUN php composer-setup.php --quiet && rm composer-setup.php && \
#     mv composer.phar /usr/local/sbin/composer && \
#     chmod +x /usr/local/sbin/composer

# install NewRelic agent
# RUN echo 'deb [trusted=yes] http://apt.newrelic.com/debian/ newrelic non-free' | tee /etc/apt/sources.list.d/newrelic.list && \
#     curl https://download.newrelic.com/548C16BF.gpg | apt-key add - && \
#     apt-get -y update && \
#     DEBIAN_FRONTEND=noninteractive apt-get -y install newrelic-php5 newrelic-sysmond && \
#     export NR_INSTALL_SILENT=1 && newrelic-install install

# install SendGrid
# RUN echo "postfix postfix/mailname string localhost" | debconf-set-selections && \
#     echo "postfix postfix/main_mailer_type string 'Internet Site'" | debconf-set-selections && \
#     DEBIAN_FRONTEND=noninteractive apt-get install postfix libsasl2-modules -y

# Set default work directory
# ADD scripts/* /usr/local/bin/
# RUN chmod +x  /usr/local/bin/*

# Add default configuration files
# ADD configs/php.ini /usr/local/etc/php/
# ADD configs/www.conf /usr/local/etc/php-fpm.d/

# Health check
# RUN echo '#!/bin/bash' > /healthcheck && \
#     echo 'env -i SCRIPT_NAME=/health SCRIPT_FILENAME=/health REQUEST_METHOD=GET cgi-fcgi -bind -connect 127.0.0.1:9000 || exit 1' >> /healthcheck && \
#     chmod +x /healthcheck


# Clean up
RUN rm -rf /var/cache/apk/* /tmp/* /var/tmp/*

# WORKDIR /