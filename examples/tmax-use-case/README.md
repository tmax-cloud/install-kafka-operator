* 테스트 방법
    * 컨테이너 내부 접속 후 아래 명령어 실행

    * Producer
    ```bash
    bin/kafka-console-producer.sh --broker-list {bootstrap IP 혹은 DNS}:9092 --topic {TOPIC 이름}
    ```

    * Consumer
    ```bash
    bin/kafka-console-consumer.sh --bootstrap-server {bootstrap IP 혹은 DNS}:9092 --topic {TOPIC 이름} --from-beginning
    ```