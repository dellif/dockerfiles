FROM php:7.2-fpm-alpine

LABEL maintainer="donglifan<donggefor2018@163.com>"

# repositorie mirror、timezone
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.ustc.edu.cn/' /etc/apk/repositories ; \
    apk add --no-cache --virtual .timezone-deps tzdata; \
    ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime ; \
    echo 'Asia/Shanghai' > /etc/timezone; \
    rm -rf /var/cache/apk/* /tmp/* /var/tmp/* /var/lib/apt/lists/*

ARG SWOOLE_VERSION=4.4.1
ARG UNAME=dongge
ARG UID=1000
ARG GID=1000

RUN set -ex ; \
	apk add --no-cache --virtual .build-deps \
	    $PHPIZE_DEPS \
	    autoconf \
	    libaio-dev \
	    openssl-dev \
	    imagemagick-dev \
	    freetype-dev \
	    icu-dev \
	    curl-dev \
	    git \
	    libxml2-dev \
	    libmemcached-dev \
		libjpeg-turbo-dev \
		rabbitmq-c-dev \
		libpng-dev; \
	docker-php-ext-configure gd --with-png-dir=/usr/include --with-jpeg-dir=/usr/include --with-freetype-dir=/usr/include/freetype2; \
	docker-php-ext-configure intl; \
	docker-php-ext-install gd mysqli opcache pdo_mysql zip intl exif pcntl bcmath sockets soap; \
	pecl install redis-4.2.0; \
	pecl install memcached-3.1.3; \
	pecl install imagick-3.4.3; \
	pecl install mongodb-1.5.3; \
	pecl install amqp-1.9.4; \
	# swoole
    curl -SL "https://github.com/swoole/swoole-src/archive/v${SWOOLE_VERSION}.tar.gz" -o swoole.tar.gz; \
    mkdir -p swoole; \
    tar -xf swoole.tar.gz -C swoole --strip-components=1; \
    rm swoole.tar.gz; \
    ( \
        cd swoole; \
        phpize; \
        ./configure --enable-mysqlnd --enable-openssl; \
        make -j$(nproc) ; \
        make install \
    ); \
    rm -r swoole; \
    # solr
    git clone https://github.com/php/pecl-search_engine-solr.git --depth=1; \
    ( \
        cd pecl-search_engine-solr; \
        phpize; \
        ./configure; \
        make -j$(nproc) ; \
        make install \
    ); \
    rm -r pecl-search_engine-solr; \
	docker-php-ext-enable redis memcached imagick swoole solr mongodb soap;\
	runDeps="$( \
		scanelf --needed --nobanner --format '%n#p' --recursive /usr/local/lib/php/extensions \
			| tr ',' '\n' \
			| sort -u \
			| awk 'system("[ -e /usr/local/lib/" $1 " ]") == 0 { next } { print "so:" $1 }' \
	)"; \
	apk add --virtual .epwk-phpexts-rundeps $runDeps; \
	apk del .build-deps; \
	rm -rf /var/cache/apk/* /tmp/* /var/tmp/* /var/lib/apt/lists/* /var/www/html

# set recommended PHP.ini settings
# see https://secure.php.net/manual/en/opcache.installation.php
RUN { \
		echo 'opcache.memory_consumption=128'; \
		echo 'opcache.interned_strings_buffer=8'; \
		echo 'opcache.max_accelerated_files=4000'; \
		echo 'opcache.revalidate_freq=2'; \
		echo 'opcache.fast_shutdown=1'; \
		echo 'opcache.enable_cli=1'; \
	} > /usr/local/etc/php/conf.d/opcache-recommended.ini

# Iconv fix: https://github.com/docker-library/php/issues/240#issuecomment-305038173
RUN apk add --no-cache --repository http://dl-cdn.alpinelinux.org/alpine/edge/community/ gnu-libiconv
ENV LD_PRELOAD=/usr/lib/preloadable_libiconv.so

COPY php7.2.ini /usr/local/etc/php/php.ini

#RUN apk --no-cache add shadow; \
#    usermod -u 1000 www-data

# run as host user dongge
RUN adduser -D -u ${UID} ${UNAME}
USER ${UNAME}

WORKDIR /var/www
