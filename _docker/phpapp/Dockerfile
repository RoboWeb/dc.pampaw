FROM php:7.4-cli-alpine3.11

ARG BUILD_DATE
ARG VCS_REF
ARG PAWPAM_VERSION
ARG APP_MAIN_DIR=/var/www/app
ARG REQUIRE_COMPOSER=false

LABEL org.label-schema.build-date=$BUILD_DATE \
      org.label-schema.vcs-ref=$VCS_REF \
      org.label-schema.schema-version=$PAWPAM_VERSION \
      org.label-schema.vendor="RoboWeb" \
      org.label-schema.maintainer="Tomasz Wolny" \
      org.label-schema.name="dc.pampaw" \
      org.label-schema.description="Docker Compose schema for PHP Applications With PHP (v.7.4.x), Apache (v.), MySQL (v.5.7.x)"

# PHP_INI_DIR to be symmetrical with official php docker image
ENV PHP_INI_DIR /etc/php7

# When using Composer, disable the warning about running commands as root/super user
ENV COMPOSER_ALLOW_SUPERUSER=1

# Persistent runtime dependencies
ARG DEPS="bash apache2 apache2-proxy composer git grep wget unzip zip"

ARG PHPM="php7 php7-bcmath php7-bz2 php7-common php7-ctype php7-curl php7-dom php7-exif php7-fileinfo php7-fpm \
          php7-gd php7-gettext php7-iconv php7-json php7-mbstring php7-mcrypt php7-mysqli php7-odbc php7-openssl \
          php7-pdo php7-pdo_dblib php7-pdo_odbc php7-pdo_mysql php7-phar \
          php7-session php7-shmop php7-simplexml php7-soap php7-sockets php7-sqlite3 \
          php7-tokenizer php7-xml php7-xmlreader php7-xmlrpc php7-xmlwriter php7-zlib php7-zip"

RUN apk update \
    apk upgrade

RUN apk add --no-cache $DEPS


RUN mkdir -p $APP_MAIN_DIR
RUN chown -R www-data:www-data $APP_MAIN_DIR 

RUN mkdir -p /var/log/php7 \
    && chown -R www-data:www-data /var/log/php7

RUN apk add --no-cache $PHPM

COPY ./confs /

# Copying application source files to the htdocs folder
COPY ./src $APP_MAIN_DIR
WORKDIR $APP_MAIN_DIR/public
RUN if [ "$REQUIRE_COMPOSER" = "true" ]; then composer install; fi

VOLUME $APP_MAIN_DIR

COPY ./scripts /scripts
RUN chmod -R 755 /scripts

EXPOSE 80

CMD ["/scripts/run.sh"]
