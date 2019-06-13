FROM redis:latest

LABEL maintainer="mallor <mallor@163.com>"

## For security settings uncomment, make the dir, copy conf, and also start with the conf, to use it
RUN mkdir -p /etc/redis/

RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
RUN echo 'Asia/Shanghai' >/etc/timezone

EXPOSE 6379

CMD ["redis-server","/etc/redis/6379.conf"]
