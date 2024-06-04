FROM technicalguru/php:8.2.18-apache-2.4.57.0

RUN apt-get update &&  apt-get update && apt-get install -y --no-install-recommends \
    patch \
    && rm -rf /var/lib/apt/lists/*

#ADD etc/php/ /usr/local/etc/php/conf.d/
#ADD etc/conf/ /etc/apache2/conf-enabled/
#ADD etc/mods/ /etc/apache2/mods-enabled/
#ADD etc/sites/ /etc/apache2/sites-enabled/
RUN chown -R www-data:www-data /var/www/html

ENV PFA_VERSION="3.3.13"
ENV PFA_REVISION="0"
ENV PFA_URL="https://github.com/postfixadmin/postfixadmin/archive/postfixadmin-${PFA_VERSION}.tar.gz"
RUN set -xe \
    && cd /var/www/html \
    && curl -o postfixadmin.tar.gz -L "$PFA_URL" \
    && tar --strip-components=1 -xvf postfixadmin.tar.gz \
    && rm postfixadmin.tar.gz \
    && mkdir templates_c 

ADD src/    /var/www/html/
#RUN patch model/MailboxHandler.php < MailboxHandler.php.patch

RUN chown -R www-data:www-data .

