# Bareos director Dockerfile
FROM       ubuntu:bionic
MAINTAINER Barcus <barcus@tou.nu>

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update -qq && \
    apt-get install -qq curl gnupg && \
    curl -Ls http://download.bareos.org/bareos/release/19.2/xUbuntu_18.04/Release.key | apt-key --keyring /etc/apt/trusted.gpg.d/breos-keyring.gpg add - && \
    echo 'deb http://download.bareos.org/bareos/release/19.2/xUbuntu_18.04/ /' > /etc/apt/sources.list.d/bareos.list && \
    echo 'bareos-database-common bareos-database-common/dbconfig-install boolean false' | debconf-set-selections && \
    echo 'bareos-database-common bareos-database-common/install-error select ignore' | debconf-set-selections && \
    echo 'bareos-database-common bareos-database-common/database-type select mysql' | debconf-set-selections && \
    echo 'bareos-database-common bareos-database-common/missing-db-package-error select ignore' | debconf-set-selections && \
    echo 'postfix postfix/main_mailer_type select No configuration' | debconf-set-selections && \
    apt-get update -qq && \
    apt-get install -qq -y bareos bareos-database-mysql mysql-client bareos-tools && \
    apt-get clean -qq -y && \
    apt-get autoremove --purge -qq -y

COPY docker-entrypoint.sh /
RUN chmod u+x /docker-entrypoint.sh
RUN tar cfvz /bareos-dir.tgz /etc/bareos

EXPOSE 9101

VOLUME /etc/bareos
VOLUME /var/lib/bareos

ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["/usr/sbin/bareos-dir", "-u", "bareos", "-f"]
