FROM ubuntu
ENV DEBIAN_FRONTEND noninteractive
ENV DEBCONF_NONINTERACTIVE_SEEN true
RUN echo 'tzdata tzdata/Areas select Etc' | debconf-set-selections; 
RUN echo 'tzdata tzdata/Zones/Etc select UTC' | debconf-set-selections; 
RUN apt update && apt upgrade -y
RUN apt-get install postgresql postgresql-contrib -y
EXPOSE 5432
USER postgres
RUN  /etc/init.d/postgresql start && psql --command "CREATE USER drupal WITH SUPERUSER PASSWORD 'drupal';" && createdb -O drupal drupal
RUN echo "host all  all    0.0.0.0/0  md5" >> /etc/postgresql/12/main/pg_hba.conf
RUN echo "listen_addresses='*'" >> /etc/postgresql/12/main/postgresql.conf
CMD ["/usr/lib/postgresql/12/bin/postgres", "-D", "/var/lib/postgresql/12/main", "-c", "config_file=/etc/postgresql/12/main/postgresql.conf"]

 