FROM java:8

ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install --no-install-recommends -y apt-transport-https software-properties-common supervisor curl wget

# Elastic repositories
RUN wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | apt-key add -
RUN echo "deb https://artifacts.elastic.co/packages/5.x/apt stable main" | tee -a /etc/apt/sources.list.d/elastic-5.x.list

# Elasticsearch
RUN apt-get update && apt-get install --no-install-recommends -y elasticsearch
RUN sed -i '/#cluster.name:.*/a cluster.name: logstash' /etc/elasticsearch/elasticsearch.yml
RUN sed -i '/#path.data: \/path\/to\/data/a path.data: /data' /etc/elasticsearch/elasticsearch.yml
ADD etc/supervisor/conf.d/elasticsearch.conf /etc/supervisor/conf.d/elasticsearch.conf

# Logstash
RUN apt-get install -y apt-transport-https
RUN apt-get install --no-install-recommends -y logstash
ADD etc/supervisor/conf.d/logstash.conf /etc/supervisor/conf.d/logstash.conf
#RUN bin/logstash-plugin install logstash-filter-translate

# Kibana
RUN apt-get install --no-install-recommends -y kibana
ADD etc/supervisor/conf.d/kibana.conf /etc/supervisor/conf.d/kibana.conf

RUN apt-get clean

EXPOSE 5601

ENV PATH /opt/logstash/bin:$PATH

CMD [ "/usr/bin/supervisord", "-n", "-c", "/etc/supervisor/supervisord.conf" ]
