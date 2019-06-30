FROM shinken-base:latest
MAINTAINER  Brian Lakstins

RUN apt-get update && \
    apt-get install -y supervisor \
    ntp \
    libsys-statistics-linux-perl \
    libssl-dev \
    python-crypto \   
    inotify-tools \ 
    nagios-plugins && \
    apt-get -y autoremove && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Install shinken modules from shinken.io
RUN su - shinken -c 'shinken --init' && \
    su - shinken -c 'shinken install booster-nrpe' 

# Install check_nrpe plugin
ADD         nrpe-2.15.tar.gz /usr/src/
RUN         cd /usr/src/nrpe-2.15/ && \
                ./configure --with-nagios-user=shinken --with-nagios-group=shinken --with-nrpe-user=shinken --with-nrpe-group=shinken --with-ssl=/usr/bin/openssl --with-ssl-lib=/usr/lib/x86_64-linux-gnu && \
                make all && \
                make install-plugin && \
                mv /usr/local/nagios/libexec/check_nrpe /usr/lib/nagios/plugins/check_nrpe && \
                cd / && \
                rm -rf /usr/src/nrpe-2.15

# Copy extra NRPE plugins and fix permissions
ADD         extra_plugins/* /usr/lib/nagios/plugins/
RUN         cd /usr/lib/nagios/plugins/ && \
                chmod a+x * && \
                chmod u+s check_apt restart_service check_ping check_icmp check_fping apt_update    

# Add shinken config watcher to restart arbiter, when changes happen
ADD         watch_shinken_config.sh /usr/bin/watch_shinken_config.sh
RUN         chmod 755 /usr/bin/watch_shinken_config.sh                

COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# Default docker process
CMD ["supervisord", "-c", "/etc/supervisor/supervisord.conf", "-n"]

