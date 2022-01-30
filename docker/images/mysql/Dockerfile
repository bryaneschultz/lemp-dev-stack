FROM mysql:8.0.28

# Set default Timezone
ARG TIMEZONE
RUN ln -snf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && \
    echo ${TIMEZONE} > /etc/timezone

RUN chown -RL mysql:mysql /var/run/mysqld/ && \
    chown -RL mysql:mysql /var/lib/mysql && \
    chmod 700 /var/run/mysqld && \
    chmod 700 /var/lib/mysql

# Clear cache
RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*
