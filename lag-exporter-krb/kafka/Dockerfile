FROM registry.access.redhat.com/ubi8/ubi-minimal
MAINTAINER pere.urbon@gmail.com
ENV container docker

ENV JAVA_MAJOR_VERSION=11 \
    JAVA_HOME=/usr/lib/jvm/jre-11

# 1. Adding Confluent repository
RUN rpm --import https://packages.confluent.io/rpm/7.0/archive.key
COPY confluent.repo /etc/yum.repos.d/confluent.repo
RUN microdnf clean all && microdnf update --nodocs

# 2. Install zookeeper and kafka
RUN microdnf install java-11-openjdk-headless \
  && microdnf clean all
RUN microdnf install confluent-server
RUN microdnf install confluent-control-center


# 3. Configure Kafka for Kerberos
RUN microdnf install krb5-workstation krb5-libs
COPY server.properties /etc/kafka/server.properties
COPY command.properties /etc/kafka/command.properties
COPY kafka.sasl.jaas.config /etc/kafka/kafka_server_jaas.conf

EXPOSE 9093

ENV KAFKA_OPTS="-Djava.security.auth.login.config=/etc/kafka/kafka_server_jaas.conf -Dzookeeper.sasl.client.username=zkservice"

CMD kafka-server-start /etc/kafka/server.properties
