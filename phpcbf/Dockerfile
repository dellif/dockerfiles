FROM php:7.2-cli-alpine3.11

LABEL maintainer="donglifan <donggefor2018@163.com>"

ARG PHPCBF_VERSION=3.5.6

# repositorie mirror、timezone
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.ustc.edu.cn/' /etc/apk/repositories ; \
    apk add --no-cache --virtual .timezone-deps tzdata; \
    ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime ; \
    echo 'Asia/Shanghai' > /etc/timezone; \
    rm -rf /var/cache/apk/* /tmp/* /var/tmp/* /var/lib/apt/lists/*

RUN curl -L https://github.com/squizlabs/PHP_CodeSniffer/releases/download/$PHPCBF_VERSION/phpcbf.phar > /usr/local/bin/phpcbf \
    && chmod +x /usr/local/bin/phpcbf \
    && rm -rf /var/cache/apk/* /var/tmp/* /tmp/*

WORKDIR /tmp
