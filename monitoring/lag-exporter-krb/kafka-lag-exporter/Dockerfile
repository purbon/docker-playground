FROM registry.access.redhat.com/ubi8/ubi-minimal
MAINTAINER pere.urbon@gmail.com
ENV container docker

ENV install_dir=/opt/docker
ENV SOURCE_URL=https://github.com/seglo/kafka-lag-exporter/releases/download/v0.6.8/kafka-lag-exporter-0.6.8.zip

USER root

RUN microdnf install wget unzip

RUN mkdir -p $install_dir && cd $install_dir \
    && wget $SOURCE_URL \
    && unzip kafka-lag-exporter-0.6.8.zip && mv kafka-lag-exporter-0.6.8 kle

#RUN microdnf clean all && microdnf update --nodocs

# 2. Install zookeeper and kafka
RUN microdnf install java-11-openjdk-headless \
  && microdnf install krb5-workstation krb5-libs \
  && microdnf clean all

COPY application.conf /opt/docker/application.conf
COPY logback.xml /opt/docker/logback.xml

EXPOSE 8000

CMD /opt/docker/kle/bin/kafka-lag-exporter -Dconfig.file=/opt/docker/application.conf -Dlogback.configurationFile=/opt/docker/logback.xml
