FROM postgres:9.4

RUN apt-get update \
  && apt-get install -y supervisor \
  && apt-get install -y make libxslt-dev libxml2-dev libpam-dev libedit-dev \
  && apt-get install -y postgresql-server-dev-9.4 \
  && rm -rf /var/lib/apt/lists/*

ADD http://repmgr.org/download/repmgr-3.0.1.tar.gz /tmp/
ADD http://repmgr.org/download/repmgr-3.0.1.tar.gz.asc /tmp/

RUN gpg --keyserver pool.sks-keyservers.net --recv-keys 297F1DCC \
  && gpg --verify /tmp/repmgr-3.0.1.tar.gz.asc \
  && rm /tmp/repmgr-3.0.1.tar.gz.asc

WORKDIR /tmp
RUN tar xvf /tmp/repmgr-3.0.1.tar.gz

WORKDIR /tmp/repmgr-3.0.1
RUN make USE_PGXS=1 \
  && make USE_PGXS=1 install

WORKDIR /
RUN mkdir -p /etc/repmgr \
  && cp /tmp/repmgr-3.0.1/repmgr.conf.sample /etc/repmgr/ \
  && rm -Rf /tmp/repmgr-3.0.1*

COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

CMD ["/usr/bin/supervisord"]
