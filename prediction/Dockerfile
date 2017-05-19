# copia de https://github.com/steveny2k/docker-predictionio.git
# actualizadas versiones, rutas de descarga y probado funcionamiento
# MAINTAINER Steven Yan

FROM ubuntu
MAINTAINER Alan Riveros <alan.gabriel.riveros@gmail.com>

ENV PIO_VERSION 0.10.0
ENV SPARK_VERSION 1.5.1
ENV ELASTICSEARCH_VERSION 1.7.3
ENV HBASE_VERSION 1.2.4

ENV PIO_HOME /PredictionIO-${PIO_VERSION}-incubating
ENV PATH=${PIO_HOME}/bin:$PATH
ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64

RUN apt-get update \
    && apt-get install -y --auto-remove --no-install-recommends curl openjdk-8-jdk libgfortran3 python-pip \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

RUN curl -O http://mirror.nexcess.net/apache/incubator/predictionio/${PIO_VERSION}-incubating/apache-predictionio-${PIO_VERSION}-incubating.tar.gz \
    && tar -xvzf apache-predictionio-${PIO_VERSION}-incubating.tar.gz -C / \
    && rm apache-predictionio-${PIO_VERSION}-incubating.tar.gz \
    && cd apache-predictionio-${PIO_VERSION}-incubating \
    && ./make-distribution.sh

RUN tar zxvf /apache-predictionio-${PIO_VERSION}-incubating/PredictionIO-${PIO_VERSION}-incubating.tar.gz -C /
RUN rm -r /apache-predictionio-${PIO_VERSION}-incubating
RUN mkdir /${PIO_HOME}/vendors
COPY files/pio-env.sh ${PIO_HOME}/conf/pio-env.sh

RUN curl -O http://d3kbcqa49mib13.cloudfront.net/spark-${SPARK_VERSION}-bin-hadoop2.6.tgz \
    && tar -xvzf spark-${SPARK_VERSION}-bin-hadoop2.6.tgz -C ${PIO_HOME}/vendors \
    && rm spark-${SPARK_VERSION}-bin-hadoop2.6.tgz

RUN curl -O https://download.elasticsearch.org/elasticsearch/elasticsearch/elasticsearch-${ELASTICSEARCH_VERSION}.tar.gz \
    && tar -xvzf elasticsearch-${ELASTICSEARCH_VERSION}.tar.gz -C ${PIO_HOME}/vendors \
    && rm elasticsearch-${ELASTICSEARCH_VERSION}.tar.gz \
    && echo 'cluster.name: predictionio' >> ${PIO_HOME}/vendors/elasticsearch-${ELASTICSEARCH_VERSION}/config/elasticsearch.yml \
    && echo 'network.host: 127.0.0.1' >> ${PIO_HOME}/vendors/elasticsearch-${ELASTICSEARCH_VERSION}/config/elasticsearch.yml

RUN curl -O http://archive.apache.org/dist/hbase/${HBASE_VERSION}/hbase-${HBASE_VERSION}-bin.tar.gz \
    && tar -xvzf hbase-${HBASE_VERSION}-bin.tar.gz -C ${PIO_HOME}/vendors \
    && rm hbase-${HBASE_VERSION}-bin.tar.gz
COPY files/hbase-site.xml ${PIO_HOME}/vendors/hbase-${HBASE_VERSION}/conf/hbase-site.xml
RUN sed -i "s|VAR_PIO_HOME|${PIO_HOME}|" ${PIO_HOME}/vendors/hbase-${HBASE_VERSION}/conf/hbase-site.xml \
    && sed -i "s|VAR_HBASE_VERSION|${HBASE_VERSION}|" ${PIO_HOME}/vendors/hbase-${HBASE_VERSION}/conf/hbase-site.xml
RUN cd ${PIO_HOME} 
RUN mkdir engines
#RUN pio-start-all 
#RUN jps -l
#RUN apt-get update
#RUN apt-get install -y git 
#RUN git clone https://github.com/PredictionIO/template-scala-parallel-universal-recommendation.git myUniversalRecommender
#RUN pio template get PredictionIO/template-scala-parallel-universal-recommendation myUniversalRecommender
#RUN cd myUniversalRecommender
#RUN pio app new MyApp1

EXPOSE 8000
EXPOSE 7070
VOLUME ["${PIO_HOME}/vendors", "${PIO_HOME}/engines"]
#prepare example: Similar Product Engine Template
#(http://predictionio.incubator.apache.org/templates/similarproduct/quickstart/)
#
#RUN pip install -U setuptools
#RUN pip install predictionio


#prepare example: Demo-Tapster
#RUN apt-get install git ruby build-essential make
#RUN gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
#RUN curl -L https://get.rvm.io | bash -s stable
#RUN touch ~/.bash_profile
#RUN export PATH=$PATH:/usr/local/rvm/bin:/usr/local/rvm/sbin
#RUN source ~/.bash_profile
