FROM nginx:latest

WORKDIR /home/

# Копирование файлов
COPY ./nginx.conf /etc/nginx/nginx.conf
COPY ./script.sh .


# Установка необходимых пакетов
RUN apt-get update; \
    apt-get install -y gcc spawn-fcgi libfcgi-dev; \
    useradd nginx; \
    chmod +x script.sh; \
    chown -R nginx:nginx /etc/nginx/nginx.conf; \
    chown -R nginx:nginx /var/cache/nginx; \
    chown -R nginx:nginx /home; \
    touch /var/run/nginx.pid; \
    chown -R nginx:nginx /var/run/nginx.pid; \
    rm -rf /var/lib/apt/lists/* 

USER nginx

HEALTHCHECK NONE

ENTRYPOINT [ "bash","script.sh" ]