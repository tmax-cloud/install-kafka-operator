# install-kafka-operator
https://strimzi.io/ 참조


## Install-Guide
- kubectl create ns kafka
- kubectl apply -f manifests/cluster-operator/strimzi-cluster-operator-0.26.0.yaml -n kafka

## 폐쇄망 설치 가이드
* 설치를 진행하기 전 아래의 과정을 통해 필요한 이미지 및 yaml 파일을 준비한다.

1. 사용하는 image repository에 설치 시 필요한 이미지를 push한다. 

    * 작업 디렉토리 생성 및 환경 설정
    ```bash
    $ mkdir -p ~/kafka-install
    $ export KAFKA_HOME=~/kafka-install
    $ cd $KAFKA_HOME
    $ export STRIMZI_OPERATOR_VERSION=0.26.0
    $ export SCHEMA_REGISTRY_VERSION=7.0.1
    $ export KAFKA_CONNECT_VERSION=v1.0.3
    $ export KAFKA_BRIDGE_VERSION=0.20.3
    $ export REGISTRY={ImageRegistryIP:Port}
    ```
    * 외부 네트워크 통신이 가능한 환경에서 필요한 이미지를 다운받는다.
    ```bash
    $ sudo docker pull quay.io/strimzi/operator:${STRIMZI_OPERATOR_VERSION}
    $ sudo docker save quay.io/strimzi/operator:${STRIMZI_OPERATOR_VERSION} > strimzi_operator_${STRIMZI_OPERATOR_VERSION}.tar

    $ sudo docker pull quay.io/strimzi/kafka:0.26.0-kafka-3.0.0
    $ sudo docker save quay.io/strimzi/kafka:0.26.0-kafka-3.0.0 > kafka_3.0.0.tar

    $ sudo docker pull quay.io/strimzi/kafka:0.26.0-kafka-2.8.0
    $ sudo docker save quay.io/strimzi/kafka:0.26.0-kafka-2.8.0 > kafka_2.8.0.tar

    $ sudo docker pull docker.io/confluentinc/cp-schema-registry:${SCHEMA_REGISTRY_VERSION}
    $ sudo docker save docker.io/confluentinc/cp-schema-registry:${SCHEMA_REGISTRY_VERSION} > schema_registry_${SCHEMA_REGISTRY_VERSION}.tar

    $ sudo docker pull docker.io/tmaxcloudck/kafka-connect:${KAFKA_CONNECT_VERSION}
    $ sudo docker save docker.io/tmaxcloudck/kafka-connect:${KAFKA_CONNECT_VERSION} > kafka_connect_${KAFKA_CONNECT_VERSION}.tar

    $ sudo docker pull quay.io/strimzi/kafka-bridge:${KAFKA_BRIDGE_VERSION}
    $ sudo docker save quay.io/strimzi/kafka-bridge:${KAFKA_BRIDGE_VERSION} > kafka_bridge_${KAFKA_BRIDGE_VERSION}.tar
    ```
  
2. 위의 과정에서 생성한 tar 파일들을 폐쇄망 환경으로 이동시킨 뒤 사용하려는 registry에 이미지를 push한다.
    ```bash
    $ sudo docker load < strimzi_operator_${STRIMZI_OPERATOR_VERSION}.tar
    $ sudo docker load < kafka_3.0.0.tar
    $ sudo docker load < kafka_2.8.0.tar
    $ sudo docker load < schema_registry_${SCHEMA_REGISTRY_VERSION}.tar
    $ sudo docker load < kafka_connect_${KAFKA_CONNECT_VERSION}.tar
    $ sudo docker load < kafka_bridge_${KAFKA_BRIDGE_VERSION}.tar
    
    $ sudo docker tag quay.io/strimzi/operator:${STRIMZI_OPERATOR_VERSION} ${REGISTRY}/strimzi/operator:${STRIMZI_OPERATOR_VERSION}
    $ sudo docker tag quay.io/strimzi/kafka:0.26.0-kafka-3.0.0 ${REGISTRY}/strimzi/kafka:0.26.0-kafka-3.0.0
    $ sudo docker tag quay.io/strimzi/kafka:0.26.0-kafka-2.8.0 ${REGISTRY}/strimzi/kafka:0.26.0-kafka-2.8.0
    $ sudo docker tag docker.io/confluentinc/cp-schema-registry:${SCHEMA_REGISTRY_VERSION} ${REGISTRY}/confluentinc/cp-schema-registry:${SCHEMA_REGISTRY_VERSION}
    $ sudo docker tag docker.io/tmaxcloudck/kafka-connect:${KAFKA_CONNECT_VERSION} ${REGISTRY}/tmaxcloudck/kafka-connect:${KAFKA_CONNECT_VERSION}
    $ sudo docker tag quay.io/strimzi/kafka-bridge:${KAFKA_BRIDGE_VERSION} ${REGISTRY}/strimzi/kafka-bridge:${KAFKA_BRIDGE_VERSION}
    
    $ sudo docker push ${REGISTRY}/strimzi/operator:${STRIMZI_OPERATOR_VERSION}
    $ sudo docker push ${REGISTRY}/strimzi/kafka:0.26.0-kafka-3.0.0
    $ sudo docker push ${REGISTRY}/strimzi/kafka:0.26.0-kafka-2.8.0
    $ sudo docker push ${REGISTRY}/confluentinc/cp-schema-registry:${SCHEMA_REGISTRY_VERSION}
    $ sudo docker push ${REGISTRY}/tmaxcloudck/kafka-connect:${KAFKA_CONNECT_VERSION}
    $ sudo docker push ${REGISTRY}/strimzi/kafka-bridge:${KAFKA_BRIDGE_VERSION}
    ```
