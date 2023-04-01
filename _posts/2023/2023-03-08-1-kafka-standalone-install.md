---
layout: post
title: kafka standalone 설치
tags :
    - kafka
---

#### 환경

kafka version : kafka_2.13-3.4.0<br>
jdk : openjdk-11.0.18<br>
os : centos7<br>
<br>
설치정보<br>

서버 | 구성
---|---
kafka1 | `kafka`, `zookeeper`

---

#### 사전준비

* jdk가 설치되어 있어야한다.<br>
* kafka download<br>
[kafka archive download](https://archive.apache.org/dist/kafka/){: target="_blank"} 에서 kafka를 download 한다.

* /etc/hosts 파일수정<br>
각 서버별로 /etc/hosts 파일에 hostname을 등록한다.

```shell
# vi /etc/hosts
192.168.31.2 kafka1
```

---


#### 설치

* 압축해제

```shell
$ tar -xvf kafka_2.13-3.4.0.tgz;
```

* zookeeper.properties 수정<br>
kafka에 내장되어 있는 zookeeper 설정파일을 수정한다.

```shell
# vi $KAFKA_HOME/config/zookeeper.properties

#zookeeper data directory로 사용할 위치로 설정한다.
dataDir=/home/kafka/data/kafka/zookeeper
```

---

#### 테스트

* zookeeper 또는 kraft를 사용하여 kafka 실행(아래 둘중 하나 선택하여 실행)


```shell
# zookeeper 기동
$ $KAFKA_HOME/bin/zookeeper-server-start.sh  -daemon $KAFKA_HOME/config/zookeeper.properties;

# kafka 기동
$ $KAFKA_HOME/bin/kafka-server-start.sh -daemon $KAFKA_HOME/config/server.properties;
```


```shell
# kraft 와 함께 기동
[kafka1@localhost]$ $KAFKA_HOME/bin/kafka-server-start.sh -daemon $KAFKA_HOME/config/server.properties
...
[kafka5@localhost]$ $KAFKA_HOME/bin/kafka-server-start.sh -daemon $KAFKA_HOME/config/server.properties
```


* Topic 생성

```shell
$ $KAFKA_HOME/bin/kafka-topics.sh --create --bootstrap-server kafka1:2181 kafka2:2181 kafka3:2181 kafka4:2181 kafka5:2181  --replication-factor 3 --partitions 50 --topic test;
```

* Topic 리스트 확인

```shell
 $ $KAFKA_HOME/bin/kafka-topics.sh --list --bootstrap-server kafka1:2181 kafka2:2181 kafka3:2181 kafka4:2181 kafka5:2181;
```

* producer 실행

```shell
 $ $KAFKA_HOME/bin/kafka-console-producer.sh --broker-list kafka1:9092 kafka2:9092 kafka3:9092 kafka4:9092 kafka5:9092 --topic test;
```

* consumer 실행

```shell
$  $KAFKA_HOME/bin/kafka-console-consumer.sh --from-beginning --bootstrap-server kafka1:9092 kafka2:9092 kafka3:9092 kafka4:9092 kafka5:9092 --topic test;
```

* kafka 종료

```shell
[kafka1@localhost]$ $KAFKA_HOME/bin/kafka-server-stop.sh;
...
[kafka5@localhost]$ $KAFKA_HOME/bin/kafka-server-stop.sh;
```

* zookeeper 종료

```shell
[kafka1@localhost]$ $KAFKA_HOME/bin/zookeeper-server-stop.sh;
...
[kafka5@localhost]$ $KAFKA_HOME/bin/zookeeper-server-stop.sh;
```

---

#### Experience

---

#### Links
[카프카(Kafka)를 설치(Install)해보자](https://yookeun.github.io/kafka/2018/07/01/kafka-install/){: target="_blank"}  
[zookeeper getting started guide](https://zookeeper.apache.org/doc/r3.1.2/zookeeperStarted.html){: target="_blank"}  
[kafka documentation](https://kafka.apache.org/documentation/){: target="_blank"}  

---
