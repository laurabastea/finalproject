FROM ubuntu
RUN apt update && apt upgrade -y

RUN apt-get -y --no-install-recommends install php-fpm nginx php-xml php-gd php-pdo php-mbstring php-pgsql php-mysql wget \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*
RUN usermod -u 1000 www-data
EXPOSE 80
RUN wget --no-check-certificate https://ftp.drupal.org/files/projects/drupal-9.2.4.tar.gz
RUN tar -zxvf drupal-9.2.4.tar.gz -C /var/www/html
COPY ./drupal.conf /etc/nginx/sites-available/default
COPY ./settings.php /var/www/html/drupal-9.2.4/sites/default/settings.php
RUN php-fpm7.4
RUN mkdir /var/www/html/drupal-9.2.4/sites/default/files 
RUN chmod a+rwx /var/www/html/drupal-9.2.4/sites/default/files 
COPY ./run.sh ./
CMD ["./run.sh"]
