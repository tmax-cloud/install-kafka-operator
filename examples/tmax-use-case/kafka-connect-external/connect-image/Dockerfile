FROM quay.io/strimzi/kafka:0.26.0-kafka-2.8.0
USER root:root
RUN mkdir -p /opt/kafka/plugins/debezium
RUN mkdir -p /opt/kafka/plugins/jdbc
COPY ./debezium-connector-mysql/ /opt/kafka/plugins/debezium/
COPY ./jdbc/ /opt/kafka/plugins/jdbc/
COPY ./avro/ /opt/kafka/plugins/avro/
USER 1001
