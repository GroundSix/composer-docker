# Composer Docker Container
FROM php:5.6-cli
MAINTAINER Rob Loach <robloach@gmail.com>

ENV DEBIAN_FRONTEND noninteractive

# Dependencies
RUN echo "deb http://packages.dotdeb.org wheezy all" >> /etc/apt/sources.list
RUN echo "deb-src http://packages.dotdeb.org wheezy all" >> /etc/apt/sources.list
RUN curl http://www.dotdeb.org/dotdeb.gpg | apt-key add -
RUN apt-get update
RUN apt-get upgrade -y
RUN apt-get install -y zlib1g zlib1g-dev php5-common git mercurial subversion
RUN apt-get install -y php5-curl php5-gd php5-json php5-mcrypt php5-mysql php5-pgsql php5-readline php5-sqlite sqlite3 mysql-client php5-mysql mcrypt php-pear

# Enable any modules
RUN php5enmod curl json mysql mysqli pdo_mysql pdo_sqlite readline gd mcrypt pdo sqlite3
RUN pecl install zip
RUN echo "extension=zip.so" > /usr/local/etc/php/conf.d/extension-zip.ini

RUN apt-get install libmcrypt-dev
RUN /bin/bash -c "cd /usr/src/php/ext/mcrypt/; phpize"
RUN /bin/bash -c "cd /usr/src/php/ext/mcrypt/; ./configure"
RUN make -C /usr/src/php/ext/mcrypt/
RUN make -C /usr/src/php/ext/mcrypt/ install
RUN cp /etc/php5/mods-available/mcrypt.ini /usr/local/etc/php/conf.d/mcrypt.ini

RUN /bin/bash -c "cd /usr/src/php/ext/mbstring/; phpize"
RUN /bin/bash -c "cd /usr/src/php/ext/mbstring/; ./configure"
RUN make -C /usr/src/php/ext/mbstring/
RUN make -C /usr/src/php/ext/mbstring/ install
RUN echo "extension=mbstring.so" > /etc/php5/mods-available/mbstring.ini
RUN cp /etc/php5/mods-available/mbstring.ini /usr/local/etc/php/conf.d/mbstring.ini

# Set memory limit
RUN echo "memory_limit=1024M" > /usr/local/etc/php/conf.d/memory-limit.ini

# Set environmental variables
ENV COMPOSER_HOME /root/composer

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Display version information
RUN php --version
RUN composer --version

# Set up the application directory.
VOLUME ["/app"]
WORKDIR /app
