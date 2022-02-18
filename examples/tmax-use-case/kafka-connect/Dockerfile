FROM strimzi/kafka:0.20.1-kafka-2.5.0
USER root:root

RUN mkdir -p /opt/kafka/plugins/debezium
COPY ./debezium-connector-mysql/ /opt/kafka/plugins/debezium/

RUN mkdir -p /opt/kafka/plugins/redis
COPY ./redis-sink-connector/ /opt/kafka/plugins/redis/

USER 1001