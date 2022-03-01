FROM registry.access.redhat.com/ubi8/ubi-minimal
MAINTAINER pere.urbon@gmail.com
ENV container docker

ENV JAVA_MAJOR_VERSION=11 \
    JAVA_HOME=/usr/lib/jvm/jre-11


# 1. Adding Confluent repository
RUN rpm --import https://packages.confluent.io/rpm/7.0/archive.key
COPY confluent.repo /etc/yum.repos.d/confluent.repo
#RUN microdnf clean all

RUN microdnf clean all && microdnf update --nodocs

# 2. Install zookeeper and kafka
RUN microdnf install java-11-openjdk-headless \
  && microdnf clean all
RUN microdnf install confluent-server

# 3. Configure zookeeper for Kerberos
RUN microdnf install krb5-workstation krb5-libs
COPY zookeeper.properties /etc/kafka/zookeeper.properties
COPY zookeeper.sasl.jaas.config /etc/kafka/zookeeper_server_jaas.conf

EXPOSE 2181

ENV KAFKA_OPTS=-Djava.security.auth.login.config=/etc/kafka/zookeeper_server_jaas.conf

CMD zookeeper-server-start /etc/kafka/zookeeper.properties
