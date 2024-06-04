FROM debian:12

RUN echo "postfix postfix/mailname string cntt.k35.com" | debconf-set-selections
RUN echo "postfix postfix/main_mailer_type string 'k35.com'" | debconf-set-selections

ENV PF_VERSION="3.7.9"
ENV PF_REVISION="0"
ENV PF_PACKAGE="3.7.9-0+deb12u1"
RUN export DEBIAN_FRONTEND=noninteractive \
    && PF_VERSION=${PF_PACKAGE} \
    && apt-get update \
    && apt-get install -y --no-install-recommends \
    default-mysql-client \
    apt-utils \
    procps \
    postfix=${PF_VERSION} \
    postfix-mysql=${PF_VERSION} \
    dovecot-core \
    dovecot-imapd \
    dovecot-pop3d \
    dovecot-lmtpd \
    dovecot-mysql \
    dovecot-sieve \
    dovecot-managesieved \
    dovecot-antispam \
    mailutils \
    vim \
    libsasl2-modules \
    rsyslog \
    dnsutils \
    telnet \
    opendkim \
    opendkim-tools \
    ca-certificates \
    && rm -rf /var/lib/apt/lists/*

RUN mkdir /usr/local/mailserver \
    && mkdir /usr/local/mailserver/templates \
    && mkdir /usr/local/mailserver/templates/postfix \
    && mkdir /usr/local/mailserver/templates/dovecot \
    && mkdir /usr/local/mailserver/templates/dovecot-conf-d \
    && mkdir /usr/local/mailserver/templates/sieve \
    && mkdir /usr/local/mailserver/templates/sql \
    && mkdir /usr/local/mailserver/templates/aliases \
    && mkdir /etc/postfix/sql \
    && mkdir /etc/opendkim \
    && mkdir /etc/opendkim/keys \
    && mkdir /var/vmail 


ADD src/ /usr/local/mailserver/
ADD etc/postfix/ /usr/local/mailserver/templates/postfix/
ADD etc/dovecot/ /usr/local/mailserver/templates/dovecot/
ADD etc/dovecot-conf-d/ /usr/local/mailserver/templates/dovecot-conf-d/
ADD etc/sieve/ /usr/local/mailserver/templates/sieve/
ADD etc/aliases/ /usr/local/mailserver/templates/aliases/
ADD etc/sql/ /usr/local/mailserver/templates/sql/
ADD etc/postfix/sasl_passwd /etc/postfix/sasl_passwd
RUN chmod 600 /etc/postfix/sasl_passwd \
    && postmap /etc/postfix/sasl_passwd

RUN chmod 755 /usr/local/mailserver/*.sh \
    && sync \
    && /usr/local/mailserver/reset-server.sh \
    && touch /etc/postfix/postscreen_access \
    && touch /etc/postfix/without_ptr \
    && adduser --gecos --disabled-login --disabled-password --home /var/vmail vmail \
    && chown vmail:vmail /usr/local/mailserver/spampipe.sh \
    && mkdir /var/vmail/mailboxes \
    && chown -R vmail:vmail /var/vmail \
    && chmod -R 770 /var/vmail \
    && cd /etc/opendkim \
    && opendkim-genkey --selector=key1 --bits=2048 --directory=keys \
    && chown opendkim /etc/opendkim/keys/key1.private \
    && usermod -aG opendkim postfix

#####################################################################
#  Running the container
#####################################################################
WORKDIR /usr/local/mailserver
# SMTP Port
EXPOSE 25
# POP3 Port
EXPOSE 110
# IMAP Port
EXPOSE 143
# SMTPS Port
EXPOSE 587
# IMAPS Port
EXPOSE 993
# POP3S Port
EXPOSE 995
# SMTP Port (used for internal delivery from amavis, do not expose to the outside world!)
EXPOSE 10025
#CMD ["/usr/local/mailserver/loop.sh"]
CMD ["/usr/local/mailserver/entrypoint.sh"]

