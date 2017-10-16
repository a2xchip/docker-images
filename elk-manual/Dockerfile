FROM java:8

ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install --no-install-recommends -y apt-transport-https software-properties-common supervisor curl wget

# Elasticsearch
RUN curl -L https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.6.2.tar.gz | tar xz -C /opt
RUN ln -s /opt/elasticsearch-5.6.2 /opt/elasticsearch
ADD etc/supervisor/conf.d/elasticsearch.conf /etc/supervisor/conf.d/elasticsearch.conf

# Logstash
RUN curl -L https://artifacts.elastic.co/downloads/logstash/logstash-5.6.2.tar.gz | tar xz -C /opt
RUN ln -s /opt/logstash-5.6.2 /opt/logstash
ADD etc/supervisor/conf.d/logstash.conf /etc/supervisor/conf.d/logstash.conf
RUN /opt/logstash/bin/logstash-plugin install logstash-filter-translate

# Kibana
RUN curl -L https://artifacts.elastic.co/downloads/kibana/kibana-5.6.2-linux-x86_64.tar.gz | tar xz -C /opt
RUN ln -s /opt/kibana-5.6.2-linux-x86_64 /opt/kibana
ADD etc/supervisor/conf.d/kibana.conf /etc/supervisor/conf.d/kibana.conf

EXPOSE 5601
EXPOSE 9200

ENV PATH /opt/logstash/bin:$PATH

CMD [ "/usr/bin/supervisord", "-n", "-c", "/etc/supervisor/supervisord.conf" ]
