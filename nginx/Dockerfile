FROM nginx:alpine

MAINTAINER idoplay <hlxo@hotmail.com>

ADD nginx.conf /etc/nginx/
COPY sites/*.conf /etc/nginx/sites-available/
VOLUME /var/www
# fix a problem, change application source from dl-cdn.alpinelinux.org to aliyun source.
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/' /etc/apk/repositories

RUN apk update \
    && apk upgrade \
    && apk add --no-cache bash \
    && adduser -D -H -u 1000 -s /bin/bash www-data \
    && rm /etc/nginx/conf.d/default.conf

#CMD ["nginx","-g","deamon off;"]
#CMD ["nginx"]
#访问正常，但是nginx配置文件无法修改，只能启动运行。如果在测试环境需要修改配置文件重启等，可以使用
ENTRYPOINT /usr/sbin/nginx -c /etc/nginx/nginx.conf && /bin/bash
EXPOSE 80 443