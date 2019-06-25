FROM shinken-minimal:latest
MAINTAINER  Brian Lakstins

# from: https://github.com/shinken-monitoring/mod-webui/wiki/Installation

# Install shinken modules from shinken.io
RUN su - shinken -c 'shinken --init' && \
    su - shinken -c 'shinken install webui2' 

# https://github.com/shinken-monitoring/mod-webui/blob/develop/requirements.txt

RUN pip install pymongo>=3.0.3 && \
    pip install requests  && \
    pip install arrow  && \
    pip install bottle==0.12.8 && \
    pip install passlib

# /etc/shinken should be in a volume, so this copy will only take place when building.
# if these files need to be updated, then they should be updated on the volume
COPY broker-master.cfg /etc/shinken/brokers/broker-master.cfg
COPY webui2.cfg /etc/shinken/modules/webui2.cfg

# Default docker process
CMD ["supervisord", "-c", "/etc/supervisor/supervisord.conf", "-n"]