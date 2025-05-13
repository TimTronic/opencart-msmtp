FROM php:8.2-apache

RUN apt-get update \
  && apt-get install -y \
                     wait-for-it \
                     unzip \
                     libfreetype6-dev \
                     libjpeg62-turbo-dev \
                     libpng-dev \
                     libzip-dev \
                     libcurl3-dev \
					 libwebp-dev \
  && docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp \
  && docker-php-ext-install -j$(nproc) gd zip mysqli curl \
  && docker-php-ext-enable gd zip mysqli curl

RUN a2enmod rewrite

### MODIFICATION BEGINS

ENV MSMTP_MAILHOST=mailserver
ENV MSMTP_TLS=off
ENV MSMTP_STARTTLS=off
ENV MSMTP_TLS_CERTCHECK=off
ENV MSMTP_AUTH=off
ENV MSMTP_FROM=mailer
ENV MSMTP_PORT=25
ENV MSMTP_LOGFILE=/var/log/msmtp.log

RUN --mount=type=cache,target=/var/cache/apt \
    --mount=type=cache,target=/var/lib/apt \
    DEBIAN_FRONTEND=noninteractive \
    apt-get update && apt-get dist-upgrade -y && apt-get install msmtp rsync gettext-base --no-install-recommends -y
   
COPY msmtp-tree/ /
ENTRYPOINT ["entrypoint-override.sh"]

### MODIFICATION ENDS