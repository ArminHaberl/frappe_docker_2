FROM frappe/bench:latest

USER frappe

ARG MARIADB_HOST
ARG MYSQL_ROOT_PASSWORD
ARG SITE_NAME
ARG ADMIN_PASSWORD

ENV MARIADB_HOST ${MARIADB_HOST}
ENV MYSQL_ROOT_PASSWORD ${MYSQL_ROOT_PASSWORD}
ENV SITE_NAME ${SITE_NAME}
ENV ADMIN_PASSWORD ${ADMIN_PASSWORD}

RUN bench init --skip-redis-config-generation --frappe-branch version-13 frappe-bench

WORKDIR /home/frappe/frappe-bench

RUN bench set-redis-cache-host redis-cache:6379 && bench set-redis-queue-host redis-queue:6379 && bench set-redis-socketio-host redis-socketio:6379 && bench set-config -g db_host ${MARIADB_HOST}

RUN echo "#!/bin/bash" >> create_site.sh
RUN echo "cd /home/frappe/frappe-bench" >> create_site.sh
RUN echo "bench new-site ${SITE_NAME} --mariadb-root-password ${MYSQL_ROOT_PASSWORD} --admin-password ${ADMIN_PASSWORD} --no-mariadb-socket" >> create_site.sh
RUN echo "bench get-app bench_manager" >> create_site.sh
RUN echo "bench --site ${SITE_NAME} install-app bench_manager" >> create_site.sh
RUN echo "bench start" >> create_site.sh

RUN echo frappe | sudo -S cp create_site.sh /create_site
RUN echo frappe | sudo -S chmod +x /create_site
