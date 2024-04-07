FROM nginx:latest

WORKDIR /home/

COPY ./nginx.conf /etc/nginx/nginx.conf
COPY ./server.c .
COPY ./run.sh .

RUN apt-get update ; \
    apt-get install -y gcc vim spawn-fcgi libfcgi-dev ; \
    chmod +x run.sh

ENTRYPOINT ["sh", "./run.sh"]