# Pull base image.
FROM php:5.6-apache
COPY config/php.ini /usr/local/etc/php/

# ADD BASHRC CONFIG
COPY config/bashrc /root/
RUN mv /root/bashrc /root/.bashrc

RUN apt-get clean && apt-get update && apt-get install -y --fix-missing \
  ruby-dev \
  rubygems \
  imagemagick \
  graphviz \
  sudo \
  git \
  vim \
  memcached \
  libmemcached-tools \
  php5-memcached \
  php5-dev \
  libpng12-dev \
  libjpeg62-turbo-dev \
  libmcrypt-dev \
  libxml2-dev \
  libxslt1-dev \
  mysql-client \
  php5-mysqlnd \
  zip \
  wget \
  linux-libc-dev \
  libyaml-dev \
  apt-transport-https

# Install PECL packages
COPY docker-php-pecl-install /usr/local/bin/
RUN chmod +x /usr/local/bin/docker-php-pecl-install
RUN docker-php-pecl-install \
  uploadprogress-1.0.3.1 \
  memcache-3.0.8 \
  yaml-1.2.0 \
  mongo-1.6.12

COPY docker-php-ext-install /usr/local/bin/
RUN chmod +x /usr/local/bin/docker-php-ext-install
RUN docker-php-ext-configure gd --with-jpeg-dir=/usr/include/ \
&& docker-php-ext-install \
  gd \
  mbstring \
  mcrypt \
  zip \
  soap \
  mysql \
  pdo_mysql \
  mysqli \
  xsl \
  opcache \
  calendar

COPY core/memcached.conf /etc/memcached.conf

# install xhprof and xhgui if needed
RUN \
docker-php-pecl-install xhprof-0.9.4; \
git clone https://github.com/perftools/xhgui.git /var/www/xhgui; \
chmod -R 0777 /var/www/xhgui/cache; \
cd /var/www/xhgui/ && php install.php; \
sed -i -e '$a\
[xhgui]\
auto_prepend_file = "/var/www/xhgui/external/header.php"' \
/usr/local/etc/php/php.ini

COPY core/xhgui/config.php /var/www/xhgui/config/

RUN rm -rf /var/www/html && \
  mkdir -p /var/lock/apache2 /var/run/apache2 /var/log/apache2 /var/www/html && \
  chown -R www-data:www-data /var/lock/apache2 /var/run/apache2 /var/log/apache2 /var/www/html

# installation of ssmtp
RUN DEBIAN_FRONTEND=noninteractive apt-get install --fix-missing -y ssmtp && rm -r /var/lib/apt/lists/*
ADD core/ssmtp.conf /etc/ssmtp/ssmtp.conf
ADD core/php-smtp.ini /usr/local/etc/php/conf.d/php-smtp.ini

COPY config/apache2.conf /etc/apache2
RUN chown -R www-data:www-data /var/www

# Installation of Opcode cache
RUN ( \
  echo "opcache.memory_consumption=128"; \
  echo "opcache.interned_strings_buffer=8"; \
  echo "opcache.max_accelerated_files=4000"; \
  echo "opcache.revalidate_freq=5"; \
  echo "opcache.fast_shutdown=1"; \
  echo "opcache.enable_cli=1"; \
  ) > /usr/local/etc/php/conf.d/opcache-recommended.ini

RUN a2enmod rewrite expires && service apache2 restart

# Our apache volume
VOLUME /var/www/html

# Expose 80 for apache
EXPOSE 80

# Set a custom entrypoint.
COPY core/docker-entrypoint.sh /
RUN chmod +x /docker-entrypoint.sh
ENTRYPOINT ["/docker-entrypoint.sh"]
