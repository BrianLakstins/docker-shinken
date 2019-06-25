FROM debian:jessie
MAINTAINER  Brian Lakstins

#from 10 minute install documents - https://shinken.readthedocs.io/en/latest/02_gettingstarted/installations/shinken-installation.html

# Install deps and create shinken user
RUN apt-get update && apt-get install -y python-pip \
    python-pycurl \
    python-setuptools \
    python-cherrypy3 \
    python-crypto \
    monitoring-plugins-standard && \
    easy_install pip && \
    apt-get -y autoremove && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

#create user for shinken
RUN useradd -m shinken 

# Install shinken
RUN pip install shinken==2.4 && \
    shinken --init && \
    rm -r /var/lib/shinken/doc/* && \
    update-rc.d -f shinken remove
