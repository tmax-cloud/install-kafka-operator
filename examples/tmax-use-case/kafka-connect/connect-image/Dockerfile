FROM confluentinc/cp-kafka-connect:5.5.0 as cp
FROM strimzi/kafka:0.20.1-kafka-2.5.0
USER root:root

COPY --from=cp /usr/share/java/kafka-connect-storage-common /opt/kafka/plugins/kafka-connect-storage-common
COPY --from=cp /usr/share/java/confluent-common /opt/kafka/plugins/confluent-common
COPY --from=cp /usr/share/java/kafka-connect-s3 /opt/kafka/plugins/kafka-connect-s3
COPY --from=cp /usr/share/java/kafka-connect-jdbc /opt/kafka/plugins/kafka-connect-jdbc
RUN cd /opt/kafka/plugins && for plugin in kafka-connect-s3 kafka-connect-jdbc; do cd $plugin; ln -s ../confluent-common; ln -s ../kafka-connect-storage-common; cd ..; done

RUN mkdir -p /opt/kafka/plugins/redis
COPY ./redis-sink-connector/ /opt/kafka/plugins/redis/

RUN mkdir -p /opt/kafka/plugins/confluentinc-jdbc-connector
COPY ./confluentinc-jdbc-connector/ /opt/kafka/plugins/confluentinc-jdbc-connector/

RUN mkdir -p /opt/kafka/plugins/confluentinc-connect-transforms
COPY ./confluentinc-connect-transforms/ /opt/kafka/plugins/confluentinc-connect-transforms/

RUN mkdir -p /opt/kafka/plugins/confluentinc-kafka-connect-json-schema-converter
COPY ./confluentinc-kafka-connect-json-schema-converter/ /opt/kafka/plugins/confluentinc-kafka-connect-json-schema-converter/

RUN mkdir -p /opt/kafka/plugins/streamthoughts-kafka-connect-file-pulse
COPY ./streamthoughts-kafka-connect-file-pulse/ /opt/kafka/plugins/streamthoughts-kafka-connect-file-pulse/

RUN mkdir -p /opt/kafka/plugins/jcustenborder-kafka-connect-json-schema
COPY ./jcustenborder-kafka-connect-json-schema/ /opt/kafka/plugins/jcustenborder-kafka-connect-json-schema/

USER 1001
