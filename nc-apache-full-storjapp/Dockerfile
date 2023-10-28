FROM nextcloud:stable-apache

RUN set -ex; \
    \
    apt-get update; \
    apt-get install -y --no-install-recommends \
        ffmpeg \
        ghostscript \
        libmagickcore-6.q16-6-extra \
        procps \
        smbclient \
        supervisor \
    ; \
    rm -rf /var/lib/apt/lists/*

RUN set -ex; \
    \
    savedAptMark="$(apt-mark showmanual)"; \
    \
    apt-get update; \
    apt-get install -y --no-install-recommends \
        libbz2-dev \
        libc-client-dev \
        libkrb5-dev \
        libsmbclient-dev \
        libffi-dev \
    ; \
    \
    docker-php-ext-configure imap --with-kerberos --with-imap-ssl; \
    docker-php-ext-install \
        bz2 \
        imap \
        ffi \
    ; \
    pecl install smbclient; \
    docker-php-ext-enable smbclient; \
    \
# reset apt-mark's "manual" list so that "purge --auto-remove" will remove all build dependencies
    apt-mark auto '.*' > /dev/null; \
    apt-mark manual $savedAptMark; \
    ldd "$(php -r 'echo ini_get("extension_dir");')"/*.so \
        | awk '/=>/ { print $3 }' \
        | sort -u \
        | xargs -r dpkg-query -S \
        | cut -d: -f1 \
        | sort -u \
        | xargs -rt apt-mark manual; \
    \
    apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false; \
    rm -rf /var/lib/apt/lists/*

RUN echo ffi.enable=true > /usr/local/etc/php/conf.d/ffi.ini

#COPY storj-ffi.ini /usr/local/etc/php/conf.d/storj-ffi.ini
#COPY storj.config.php /usr/src/nextcloud/config/storj.config.php

# See here for possible updates storj app https://link.storjshare.io/raw/jvfizc37vgr5ohyxwreg7abnxxrq/nextcloud-app-assets/
#RUN mkdir -p /usr/src/nextcloud/apps \
#    && cd /usr/src/nextcloud/apps \
#    && curl --output storj.tar.gz https://link.storjshare.io/raw/jvfizc37vgr5ohyxwreg7abnxxrq/nextcloud-app-assets/storj-v0.0.9.tar.gz \
#    && tar -x -f storj.tar.gz \
#    && rm storj.tar.gz

RUN mkdir -p \
    /var/log/supervisord \
    /var/run/supervisord \
;

COPY supervisord.conf /

ENV NEXTCLOUD_UPDATE=1

CMD ["/usr/bin/supervisord", "-c", "/supervisord.conf"]
