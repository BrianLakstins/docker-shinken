FROM shinken-webui2:latest
MAINTAINER  Brian Lakstins

#install apache
RUN apt-get update && \
    apt-get install -y apache2 \
    libapache2-mod-proxy-html && \
    apt-get -y autoremove && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*     

# Configure apache
COPY  apache.conf /etc/apache2/sites-available/shinken_apache.conf
RUN   a2ensite shinken_apache && \
      ln -sf /etc/apache2/mods-available/proxy* /etc/apache2/mods-enabled/ && \
      ln -sf /etc/apache2/mods-available/slotmem* /etc/apache2/mods-enabled/ && \
      update-rc.d -f apache2 remove

#add apache to startup
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# Default docker process
CMD ["supervisord", "-c", "/etc/supervisor/supervisord.conf", "-n"]