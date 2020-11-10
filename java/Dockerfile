FROM alpine:3.12

LABEL maintainer="donglf<dlf@88.com>"

# repositorie mirror、timezone
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.ustc.edu.cn/' /etc/apk/repositories ; \
    apk add --no-cache --virtual .timezone-deps tzdata; \
    ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime ; \
    echo 'Asia/Shanghai' > /etc/timezone; \
    rm -rf /var/cache/apk/* /tmp/* /var/tmp/* /var/lib/apt/lists/*
# jdk 复制并解压
ADD jdk-8u202-linux-x64.tar.gz /usr/local/

# apache-tomcat-6.0.32 复制并解压
ADD apache-tomcat-6.0.32.tar.gz /usr/local/

# Alpine安装 Glibc https://github.com/sgerrand/alpine-pkg-glibc
RUN apk --no-cache add ca-certificates \
    && wget -q -O /etc/apk/keys/sgerrand.rsa.pub https://alpine-pkgs.sgerrand.com/sgerrand.rsa.pub \
    && wget https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.30-r0/glibc-2.30-r0.apk \
    && apk add glibc-2.30-r0.apk \
    && rm -rf *.apk \
    && rm -rf /var/cache/apk/*

ENV JAVA_HOME=/usr/local/jdk1.8.0_202
ENV CATALINA_HOME=/usr/local/apache-tomcat-6.0.32
ENV JRE_HOME=${JAVA_HOME}/jre
ENV CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
ENV PATH=.:${JAVA_HOME}/bin:${CATALINA_HOME}/bin:$PATH

WORKDIR ${CATALINA_HOME}/bin
