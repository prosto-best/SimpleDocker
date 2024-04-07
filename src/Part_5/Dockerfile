FROM nginx:latest

WORKDIR /home/

COPY ./nginx.conf /etc/nginx/nginx.conf
COPY ./server.c .
COPY ./run.sh .

RUN apt-get update ; \
    apt-get install -y gcc vim spawn-fcgi libfcgi-dev ; \
    chmod +x run.sh ; \
    rm -rf /var/lib/apt/lists/* ; \
    chmod u-s /bin/mount ; \
    chmod g-s /usr/bin/expiry ; \
    chmod u-s /usr/bin/newgrp ; \
    chmod g-s /usr/bin/chage ; \
    chmod g-s /usr//sbin/unix_chkpwd ; \
    chmod u-s /usr//bin/mount ; \
    chmod g-s /usr/bin/wall ; \
    chmod u-s /usr//bin/su ; \
    chmod u-s /usr/bin/passwd ; \
    chmod u-s /usr/bin/chfn ; \
    chmod u-s /usr/bin/chsh ; \
    chmod u-s /usr/bin/gpasswd ; \
    chmod u-s /bin/umount 

USER nginx

HEALTHCHECK --interval=5m --timeout=3s \
  CMD curl -f http://localhost/ || exit
  
ENTRYPOINT ["sh", "./run.sh"]