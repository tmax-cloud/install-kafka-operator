# Kafka Connect
## Mysql to Postgres 예제
* DB 생성
  * Mysql, Postgres, Redis 생성
    ```bash
    kubectl apply -f db-deploy/
    ```

* Kafka Broker, Schema Registry, Kafka Connect 생성
  * 사전에 Dockerfile을 통해 kafka connect 이미지를 생성해야 함
  * Kafka Broker
    ```bash
    kubectl apply -f 1_kafka-simple-connect.yaml
    kubectl apply -f 2_schema-registry.yaml
    kubectl apply -f 3_kafka-connect.yaml
    ```

* Mysql Source Connector, Postgres Sink connector 생성
  * 컨테이너 접속하여 아래 명령어 실행
    ```bash
    kubectl apply -f 4_kafka-mysql-source-connector.yaml
    kubectl apply -f 5_kafka-postgres-sink-connector.yaml
    ```

* Postgres에 Mysql과 똑같은 테이블 및 데이터가 생성되는 것 확인

## Redis Sink Connector 예제
* Redis Sink Connector
  * Redis에 저장될 수 있는 형식으로 Pub 해야함
    * --property "parse.key=true" --property "key.separator=:" 옵션 추가
  * e.g.,)
    ``` bash
    bin/kafka-console-producer.sh --broker-list my-cluster-kafka-bootstrap:9092 --topic dbserver1.{DbName}.{TableName} --property "parse.key=true" --property "key.separator=:"

    > key1:value1
    > key2:value2
    ...
    ```

## 참고  
  * kafka connect & connector 예제  
  https://strimzi.io/blog/2020/01/27/deploying-debezium-with-kafkaconnector-resource/
  * kafka (sink/source) connector 모음  
  https://www.confluent.io/hub/
