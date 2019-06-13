FROM docker.elastic.co/elasticsearch/elasticsearch:7.1.1
LABEL maintainer="mallor <mallor@163.com>"
RUN mkdir -p /usr/share/elasticsearch/plugins/analysis-ik
RUN cd /usr/share/elasticsearch/plugins/analysis-ik && wget https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v7.1.1/elasticsearch-analysis-ik-7.1.1.zip && unzip elasticsearch-analysis-ik-7.1.1.zip

EXPOSE 9200 9300