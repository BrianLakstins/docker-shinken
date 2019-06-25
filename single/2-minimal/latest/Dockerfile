FROM shinken-base:latest
MAINTAINER  Brian Lakstins

RUN apt-get update && \
    apt-get install -y supervisor  && \
    apt-get -y autoremove && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# Default docker process
CMD ["supervisord", "-c", "/etc/supervisor/supervisord.conf", "-n"]

