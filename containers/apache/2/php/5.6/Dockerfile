FROM debian:buster-slim

LABEL maintainer="Areal <hello@arealidea.ru>"

ENV APACHE_RUN_USER = www-data \
    APACHE_RUN_GROUP = www-data \
    APACHE_LOG_DIR = /var/log/apache2 \
    APACHE_LOCK_DIR = /var/lock/apache2 \
    APACHE_RUN_DIR = /var/run/apache2 \
    APACHE_PID_FILE = /var/run/apache2.pid

RUN apt update && \
    apt upgrade -y && \
    apt install -y curl apt-transport-https ca-certificates rsyslog cron && \
    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.0/install.sh | bash && \
    . ~/.bashrc && \
    nvm install node && \
    ln -s ~/.nvm/versions/node/*/bin/node /usr/local/bin/node && \
    ln -s ~/.nvm/versions/node/*/bin/npx /usr/local/bin/npx && \
    ln -s ~/.nvm/versions/node/*/bin/npm /usr/local/bin/npm && \
    curl -o /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg && \
    echo 'deb https://packages.sury.org/php/ buster main' > /etc/apt/sources.list.d/php.list && \
    apt update && \
    apt install -y apache2 php5.6 php5.6-cli libapache2-mod-php5.6 \
        php5.6-mysql \
        php5.6-mbstring \
        php5.6-gd \
        php5.6-curl \
        php5.6-xdebug \
        php5.6-opcache \
        php5.6-xml \
        composer

COPY ./rootfs /

RUN a2enmod remoteip && \
    a2enconf remoteip && \
    a2enmod php5.6 && \
    a2enmod rewrite && \
    usermod -u 1000 www-data

WORKDIR /var/www/html

EXPOSE 80

CMD /usr/sbin/apache2ctl -D FOREGROUND
