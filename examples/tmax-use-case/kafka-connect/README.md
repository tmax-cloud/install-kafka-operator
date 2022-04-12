# Kafka Connect
## 기본 Cluster 구축
* Kafka Cluster 배포
    ```bash
    kubectl apply -f 1_kafka-simple-connect.yaml
    ```

* Schema Registry 배포
    ```bash
    kubectl apply -f 2_schema-registry.yaml
    ```
  
* Kafka Connect 배포
    ```bash
    kubectl apply -f 3_kafka-connect.yaml
    ```

## Mysql to Postgres 예제
* DB 생성
    ```bash
    kubectl apply -f mysql-to-postgres/mysql.yaml
    kubectl apply -f mysql-to-postgres/postgres.yaml
    ```

* JDBC Mysql Source Connector, JDBC Postgres Sink connector 생성
    ```bash
    kubectl apply -f 4_kafka-mysql-source-connector.yaml
    kubectl apply -f 5_kafka-postgres-sink-connector.yaml
    ```

* Postgres에 Mysql과 똑같은 테이블 및 데이터가 생성되는 것 확인
* 비고
  * jdbc source connector는 로그 기반으로 동작하지 않기 때문에 delete는 반영되지 않음

## Postgres to Postgres 예제
* DB 생성
    ```bash
    kubectl apply -f postgres-to-postgres/postgres.yaml
    kubectl apply -f postgres-to-postgres/postgres-2.yaml
    ```

* Debezium Postgres Source Connector, JDBC Postgres Sink connector 생성
    ```bash
    kubectl apply -f 4_kafka-postgres-debezium-source-connector.yaml
    kubectl apply -f 5_kafka-postgres-jdbc-sink-connector.yaml
    ```

* postgres와 postgres-2에 똑같은 테이블 및 데이터가 생성되는 것 확인
* 비고
  * debezium source connector는 로그 기반으로 동작하기 때문에 delete도 반영 가능

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
