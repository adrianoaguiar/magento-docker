# web root at /var/www/html/magento
# port 88
FROM nginx:1.9.12

# prepare magento dir
RUN mkdir -p /var/www/html/magento/var/logs
RUN mkdir -p /usr/local/var/run
RUN mkdir -p /usr/local/var/log
RUN mkdir -p /usr/local/var/log/nginx

RUN apt-get update && apt-get install -y vim
RUN apt-get install telnet

ENV USR_LOCAL /usr/local
ENV ETC_NGX /etc/nginx
RUN mkdir -p $ETC_NGX/sites-available/ && mkdir -p $ETC_NGX/sites-enabled/ && mkdir -p $USR_LOCAL/var/log/nginx
COPY ./nginx.conf /etc/nginx/

# make dummy default for magento site config
# will be mapped drive later on docker compose yml
RUN touch $ETC_NGX/sites-available/default
COPY ./site/default $ETC_NGX/sites-available/
RUN ln -s $ETC_NGX/sites-available/default $ETC_NGX/sites-enabled/default

EXPOSE 88
# nginx -g deamon off;
CMD ["nginx", "-g", "daemon off;"]
