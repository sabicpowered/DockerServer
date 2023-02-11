FROM phpmyadmin:fpm-alpine

RUN apk update; \
    apk upgrade;

RUN mkdir -p /var/www/pma

# RUN apk add \
#     $PHPIZE_DEPS \
#     bash \
#     apache2 \
#     curl \
#     ca-certificates \
#     openssl \
#     git \
#     tzdata \
#     openntpd \
#     zip \
#     unzip \
#     dialog \
#     nano \
#     wget \
#     mysql-client \
#     build-base \
#     gnupg \
#     musl-locales \
#     nodejs npm \
#     composer

# RUN sed -i "s/#LoadModule\ rewrite_module/LoadModule\ rewrite_module/" /usr/local/apache2/conf/httpd.conf;
# COPY .bashrc /root/.bashrc

# Insure an SSL directory exists
# RUN mkdir -p /etc/apache2/ssl

# Enable SSL support
# RUN a2enmod ssl && a2enmod rewrite

# Enable apache modules
# RUN a2enmod rewrite headers

# RUN a2dismod mpm_prefork
# RUN a2enmod mpm_event proxy_fcgi setenvif

# Enable SSL support
# RUN a2enmod ssl
