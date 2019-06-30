FROM shinken-apache:latest
MAINTAINER  Brian Lakstins

RUN apt-get update && \
    apt-get install -y python-dev \
    libcairo2-dev \
    curl \
    libffi-dev \
    build-essential && \
    apt-get -y autoremove && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Install shinken modules from shinken.io
RUN         chown -R shinken:shinken /etc/shinken/ && \
                su - shinken -c 'shinken --init' && \
                su - shinken -c 'shinken install livestatus' && \
                su - shinken -c 'shinken install graphite' && \
                su - shinken -c 'shinken install ui-graphite'

# Install Graphite
RUN         pip install whisper==0.9.12 && \
                pip install carbon==0.9.12 && \
                pip install graphite-web==0.9.12 && \
                pip install django==1.5.10 && \
                pip install django-tagging==0.3.2 
                
# Install and configure thruk
RUN             curl -s "https://labs.consol.de/repo/stable/RPM-GPG-KEY" | apt-key add - && \ 
                echo "deb http://labs.consol.de/repo/stable/debian jessie main" > /etc/apt/sources.list.d/labs-consol-stable.list && \
                apt-get update && \
                apt-get install -y thruk && \
                apt-get clean
COPY         thruk_local.conf /etc/thruk/thruk_local.conf


# Configure apache
COPY  apache.conf /etc/apache2/sites-available/shinken_apache.conf

# Configure graphite
COPY         graphite/carbon.conf /opt/graphite/conf/
COPY         graphite/storage-schemas.conf /opt/graphite/conf/
COPY         graphite/storage-aggregation.conf /opt/graphite/conf/
COPY         graphite/local_settings.py /opt/graphite/webapp/graphite/local_settings.py
RUN         mkdir -p /var/log/graphite && \
                cd /opt/graphite/webapp/graphite/ && \
                python manage.py syncdb --noinput

#add apache to startup
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# Default docker process
CMD ["supervisord", "-c", "/etc/supervisor/supervisord.conf", "-n"]