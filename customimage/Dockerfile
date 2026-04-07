FROM redhat/ubi10:latest

RUN yum install httpd nginx -y

COPY httpd.conf /etc/httpd/conf/httpd.conf 

RUN useradd webserver 
RUN chown -R webserver:webserver /var/log/httpd \
    /var/run \
    /var/lib/nginx \
    /var/log/nginx \
    /usr/share/nginx/html

RUN sed -i '/^user/d' /etc/nginx/nginx.conf
RUN sed -i 's#/run/nginx.pid#/tmp/nginx.pid#' /etc/nginx/nginx.conf


EXPOSE 82 
EXPOSE 80

USER webserver
ENTRYPOINT sh -c "/usr/sbin/httpd -D FOREGROUND & /usr/sbin/nginx -g 'daemon off;'"

CMD sh -c "/bin/bash sleep 3000"

 