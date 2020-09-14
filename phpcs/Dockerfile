FROM php:7.2-cli-alpine3.11

LABEL maintainer="donglifan <donggefor2018@163.com>"

ARG PHPCS_VERSION=3.5.6

# repositorie mirror、timezone
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.ustc.edu.cn/' /etc/apk/repositories ; \
    apk add --no-cache --virtual .timezone-deps tzdata; \
    ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime ; \
    echo 'Asia/Shanghai' > /etc/timezone; \
    rm -rf /var/cache/apk/* /tmp/* /var/tmp/* /var/lib/apt/lists/*

# phpcs
RUN curl -L https://github.com/squizlabs/PHP_CodeSniffer/releases/download/$PHPCS_VERSION/phpcs.phar > /usr/local/bin/phpcs \
    && chmod +x /usr/local/bin/phpcs \
    && rm -rf /var/cache/apk/* /var/tmp/* /tmp/*

# phpcbf
RUN curl -L https://github.com/squizlabs/PHP_CodeSniffer/releases/download/$PHPCBF_VERSION/phpcbf.phar > /usr/local/bin/phpcbf \
    && chmod +x /usr/local/bin/phpcbf \
    && rm -rf /var/cache/apk/* /var/tmp/* /tmp/*

# 使用docker compose 启动时需要将工作区代码挂载进去
WORKDIR /tmp
