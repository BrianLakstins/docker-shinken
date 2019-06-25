FROM shinken-webui2:latest
MAINTAINER  Brian Lakstins

#https://github.com/shinken-monitoring/mod-webui/wiki/Installing-WebUI-storage-modules

#https://docs.mongodb.com/manual/tutorial/install-mongodb-on-debian/

RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 9DA31620334BD75D9DCB49F368818C72E52529D4 && \
    echo "deb http://repo.mongodb.org/apt/debian jessie/mongodb-org/4.0 main" | tee /etc/apt/sources.list.d/mongodb-org-4.0.list
    
RUN apt-get update && \
    apt-get install -y mongodb-org python-pymongo && \
    apt-get -y autoremove && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

#create data directory for mongodb
RUN mkdir /data && mkdir /data/db   

# Install mongodb modules
RUN /usr/bin/shinken install mod-mongodb && \
    /usr/bin/shinken install snapshot-mongodb && \
    /usr/bin/shinken install retention-mongodb

#copy configuration files
COPY mongodb.cfg /etc/shinken/modules/mongodb.cfg
COPY webui2.cfg /etc/shinken/modules/webui2.cfg
COPY arbiter-master.cfg /etc/shinken/arbiters/arbiter-master.cfg
COPY retention-mongodb.cfg /etc/shinken/modules/retention-mongodb.cfg
COPY scheduler-master.cfg /etc/shinken/schedulers/scheduler-master.cfg

#add mongod to startup
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# Default docker process
CMD ["supervisord", "-c", "/etc/supervisor/supervisord.conf", "-n"]