FROM donglf681/php:7.2-cli-alpine3.12-v3

LABEL maintainer="donglifan <dongggefor2018@163.com>"

# repositorie mirror、timezone
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.ustc.edu.cn/' /etc/apk/repositories ; \
    apk add --no-cache --virtual .timezone-deps tzdata; \
    ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime ; \
    echo 'Asia/Shanghai' > /etc/timezone; \
    rm -rf /var/cache/apk/* /tmp/* /var/tmp/* /var/lib/apt/lists/*

ENV COMPOSER_REPO_PACKAGIST "https://mirrors.aliyun.com/composer/"
ENV COMPOSER_ALLOW_SUPERUSER 1
ENV COMPOSER_HOME /tmp
ENV COMPOSER_VERSION 1.10.1
USER root

RUN set -ex ; \
	apk add --no-cache --virtual .build-deps \
	    $PHPIZE_DEPS \
	    bzip2-dev; \
	docker-php-ext-install bz2; \
	runDeps="$( \
		scanelf --needed --nobanner --format '%n#p' --recursive /usr/local/lib/php/extensions \
			| tr ',' '\n' \
			| sort -u \
			| awk 'system("[ -e /usr/local/lib/" $1 " ]") == 0 { next } { print "so:" $1 }' \
	)"; \
	apk add --virtual .qyt-phpexts-rundeps $runDeps; \
	apk del .build-deps; \
	rm -rf /var/cache/apk/* /tmp/* /var/tmp/* /var/lib/apt/lists/* /var/www/html

COPY composer.phar /tmp/

# Composer
RUN mv /tmp/composer.phar /usr/local/bin/composer \
	&& chmod +x /usr/local/bin/composer \
    && composer self-update --clean-backups \
	&& composer --ansi --version --no-interaction \
    && composer config -g repo.packagist composer ${COMPOSER_REPO_PACKAGIST}

WORKDIR /app

ENTRYPOINT ["composer"]