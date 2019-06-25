FROM shinken-mongodb:latest
MAINTAINER  Brian Lakstins

#install nginx
RUN apt-get update && \
    apt-get install -y nginx && \
    apt-get -y autoremove && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*


# Configure nginx
COPY         nginx.conf /etc/nginx/sites-available/shinken_nginx.conf
RUN         mkdir -p /var/log/nginx && \
                rm -f /etc/nginx/sites-enabled/default && \
                ln -sf /etc/nginx/sites-available/shinken_nginx.conf /etc/nginx/sites-enabled/shinken_nginx.conf && \
                update-rc.d -f nginx remove && \
                echo "daemon off;" >> /etc/nginx/nginx.conf

#add nginx to startup
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# Default docker process
CMD ["supervisord", "-c", "/etc/supervisor/supervisord.conf", "-n"]