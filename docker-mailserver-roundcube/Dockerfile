FROM technicalguru/php:8.2.18-apache-2.4.57.0

RUN export DEBIAN_FRONTEND=noninteractive && apt-get update && apt-get install -y --no-install-recommends \
    wget \
    && rm -rf /var/lib/apt/lists/*

#ADD etc/php/ /usr/local/etc/php/conf.d/
#ADD etc/conf/ /etc/apache2/conf-enabled/
#ADD etc/mods/ /etc/apache2/mods-enabled/
#ADD etc/sites/ /etc/apache2/sites-enabled/
#ADD src/    /var/www/html/
RUN chown -R www-data:www-data /var/www/html
ENV ROUNDCUBE_VERSION="1.6.6"
ENV ROUNDCUBE_REVISION="0"

RUN cd /var/www/html \
    && wget -O roundcube.tar.gz https://github.com/roundcube/roundcubemail/releases/download/${ROUNDCUBE_VERSION}/roundcubemail-${ROUNDCUBE_VERSION}-complete.tar.gz \
    && tar --strip-components=1 -xvf roundcube.tar.gz \
    && rm roundcube.tar.gz;

COPY config.inc.php /var/www/html/config/config.inc.php

RUN chown -R www-data:www-data .


