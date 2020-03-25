FROM mongo:4.0.17-xenial

LABEL maintainer="donglifan <donggefor2018@163.com>"

# Timezone
ENV TZ=Asia/Shanghai
RUN sed -i 's/archive.ubuntu.com/mirrors.aliyun.com/g' /etc/apt/sources.list \
          && apt-get update \
          && ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone \
          && apt-get install tzdata \
          && apt-get clean \
          && apt-get autoclean \
          && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* \
          && dpkg-reconfigure -f noninteractive tzdata

#COPY mongo.conf /usr/local/etc/mongo/mongo.conf
