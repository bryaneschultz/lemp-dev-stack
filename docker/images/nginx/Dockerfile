FROM nginx:latest

ARG DEBIAN_FRONTEND=noninteractive
ARG PRIMARY_DOMAIN
ARG PRIMARY_SITE_URL
ARG PMA_SITE_URL
ARG TIMEZONE
ARG USE_SSL
ARG VIRTUAL_HOST

# Install dependencies and update
RUN apt-get install apt-transport-https tzdata && \
    apt-get update && \
    apt-get upgrade -y

# Set default Timezone
RUN ln -snf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && \
    echo ${TIMEZONE} > /etc/timezone

# Clear cache and setup direcotries
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* \
           /tmp/* \
           /var/tmp/* \
           /usr/share/doc/* && \
    dpkg-reconfigure tzdata && \
    mkdir -p /etc/nginx/conf \
             /etc/nginx/sites-available

# Copy all necessary Nginx confs
COPY nginx/conf/nginx.conf /etc/nginx/
COPY nginx/conf/default.conf /etc/nginx/conf.d/
COPY nginx/conf/ssl.conf nginx/conf/default-nossl.conf nginx/conf/default-ssl.conf /tmp/
COPY nginx/conf/phpmyadmin-nossl.conf nginx/conf/phpmyadmin-ssl.conf /tmp/

# Determine which confs to load on build
RUN if [ "${USE_SSL}" != 'false' ]; then \
    sed /tmp/ssl.conf -e "s/{%DOMAIN%}/${PRIMARY_DOMAIN}/" > /etc/nginx/conf/ssl.conf && \
    sed /tmp/default-ssl.conf -e "s/{%SITE_URL%}/${PRIMARY_SITE_URL}/" \
        >  /etc/nginx/sites-available/${PRIMARY_SITE_URL}.conf && \
    sed /tmp/phpmyadmin-ssl.conf -e "s/{%SITE_URL%}/${PMA_SITE_URL}/" \
        >  /etc/nginx/sites-available/${PMA_SITE_URL}.conf; \
else \
    sed /tmp/default-nossl.conf -e "s/{%SITE_URL%}/${PRIMARY_SITE_URL}/" \
        >  /etc/nginx/sites-available/${PRIMARY_SITE_URL}.conf && \
    sed /tmp/phpmyadmin-nossl.conf -e "s/{%SITE_URL%}/${PMA_SITE_URL}/" \
        >  /etc/nginx/sites-available/${PMA_SITE_URL}.conf; \
fi

RUN cp /etc/nginx/sites-available/${PRIMARY_SITE_URL}.conf /etc/nginx/conf.d/ && \
    cp /etc/nginx/sites-available/${PMA_SITE_URL}.conf /etc/nginx/conf.d/

# Clear temp files
RUN rm -rf /tmp/*
