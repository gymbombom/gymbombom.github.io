---
layout: post
title: kafka Cluster 설치
tags :
    - kafka
---

#### 환경

kafka version : kafka_2.13-3.4.0<br>
jdk : openjdk-17.0.2<br>
os : centos7<br>

설치정보<br>
총 5대의 cluster로 구성된 kafka cluster를 구성했다.<br>
기본적으로 kafka 설정시 1개의 서버에서 설치 및 setting을 완료하고, 전체서버로 배포한 다음 필요한 설정만 각 서버에서 변경한다.<br>
각 서버별 구성은 다음과 같다.<br>

서버 | 구성
---|---
kafka1 | `kafka`, `zookeeper`
kafka2 | `kafka`, `zookeeper`
kafka3 | `kafka`, `zookeeper`
kafka4 | `kafka`, `zookeeper`
kafka5 | `kafka`, `zookeeper`

---

#### 사전준비

* jdk가 설치되어 있어야한다.<br>
* kafka download<br>
[kafka archive download](https://archive.apache.org/dist/kafka/) 에서 kafka를 download 한다.

* /etc/hosts 파일수정<br>
각 서버별로 /etc/hosts 파일에 hostname을 등록한다.

```shell
# vi /etc/hosts
192.168.31.2 kafka1  
192.168.31.3 kafka2
192.168.31.4 kafka3
192.168.31.5 kafka4
192.168.31.6 kafka5
```

---


#### 설치

* zookeeper.properties 수정<br>
kafka에 내장되어 있는 zookeeper 설정파일을 수정한다.

```shell
# vi $KAFKA_HOME/config/zookeeper.properties

#zookeeper data directory로 사용할 위치로 설정한다.
dataDir=/home/kafka/data/kafka/zookeeper

# 클라이언트 연결을 수신할 포트
clientPort=2181 

# 하나의 클라이언트에서 동시접속하는 개수 제한. 0은 무제한
maxClientCnxns=0

# tick 단위 시간을 설정, milliseconds 단위
tickTime=2000

# 처음 주키퍼의 follower가 leader와의 연결 시도 시, tick 제한 횟수. tick 제한 횟수가 넘으면 timeout
initLimit=10

# follower가 leader와 연결된 후,leader와 동기화되기 위한 tick 제한 횟수
syncLimit=5

# zookeeper cluster server리스트를 작성한다.
server.1=kafka1:2888:3888
server.2=kafka2:2888:3888
server.3=kafka3:2888:3888
server.4=kafka4:2888:3888
server.5=kafka5:2888:3888
```


* server.properties 수정<br>
kafka server 설정파일을 수정한다.

```shell
# vi $KAFKA_HOME/config/server.properties

# 브로커ID
broker.id=1

# kafka topic log가 저장될 directory를 지정한다.
log.dirs=/home/kafka/data/kafka/kafka-logs

# zookeeper 서버 리스트를 적는다.
# kafka가 cluster 로 동작하기 위해서는 zookeeper 필수로 필요하다.
zookeeper.connect=kafka1:2181,kafka2:2181,kafka3:2181,kafka4:2181,kafka5:2181
```

* kafka1 서버에서 작성한 설정파일 및 kafka package 압축 및 배포

```shell
[kafka1@localhost]$ tar -cvf ./kafka.tar ./kafka

# 전체 서버로 배포
[kafka1@localhost]$ scp -r ./kafka.tar kafka@kafka1:/home/kafka;
...
[kafka1@localhost]$ scp -r ./kafka.tar kafka@kafka5:/home/kafka;
```


* 각 서버에서 패키지 압축 해제

```shell
[kafka1@localhost]$ tar -xvf ./kafka.tar;
...
[kafka5@localhost]$ tar -xvf ./kafka.tar;
```

* 각 서버에 myid 파일 작성
zookeeper.properties 에서 `dataDir`로 설정한 디렉토리 위치에 myid 파일을 생성하고,<br>
server list에서 `server.숫자` 와 동일하게 각각 myid 파일을 작성한다. 

```shell
[kafka1@localhost]$ echo 1 > /home/kafka/data/kafka/zookeeper/myid;
...
[kafka5@localhost]$ echo 5 > /home/kafka/data/kafka/zookeeper/myid;
```

* 각 서버의 broker.id 변경

```shell
# vi $KAFKA_HOME/config/server.properties

# 모든 서버의 broker.id를 각 서버별로 myid파일과 동일한 숫자로 변경한다.
broker.id=1
...
broker.id=5
```

* zookeeper 기동

```shell
# kafka1 ~ 5 서버의 zookeeper를 기동한다.
# kafka cluster 일 경우, kafka 기동 전 zookeeper 가 먼저 기동되어 있어야 한다.
[kafka1@localhost]$ zookeeper-server-start.sh  -daemon /home/kafka/kafka_2.11-0.10.1.1/config/zookeeper.properties
...
[kafka5@localhost]$ $KAFKA_HOME/bin/zookeeper-server-start.sh  -daemon /home/kafka/kafka_2.11-0.10.1.1/config/zookeeper.properties
```
* kafka 기동

```shell
[kafka1@localhost]$ $KAFKA_HOME/bin/kafka-server-start.sh -daemon /home/kafka/kafka_2.11-0.10.1.1/config/server.properties
```

---

#### 테스트

* Topic 생성

```shell
$ $KAFKA_HOME/bin/kafka-topics.sh --create --zookeeper kafka1:2181 kafka2:2181 kafka3:2181 kafka4:2181 kafka5:2181  --replication-factor 3 --partitions 50 --topic test;
```

* Topic 리스트 확인

```shell
 $ $KAFKA_HOME/bin/kafka-topics.sh --list --zookeeper kafka1:2181 kafka2:2181 kafka3:2181 kafka4:2181 kafka5:2181;
```

* producer 실행

```shell
 $ $KAFKA_HOME/bin/kafka-console-producer.sh --broker-list kafka1:9092 kafka2:9092 kafka3:9092 kafka4:9092 kafka5:9092 --topic test;
```

* consumer 실행

```shell
$  $KAFKA_HOME/bin/kafka-console-consumer.sh --from-beginning --new-consumer --bootstrap-server kafka1:9092 kafka2:9092 kafka3:9092 kafka4:9092 kafka5:9092 --topic test;
```

---

#### Experience

---

#### Links
[카프카(Kafka)를 설치(Install)해보자](https://yookeun.github.io/kafka/2018/07/01/kafka-install/){: target="_blank"}  
[zookeeper getting started guide](https://zookeeper.apache.org/doc/r3.1.2/zookeeperStarted.html){: target="_blank"}  
[kafka documentation](https://kafka.apache.org/documentation/){: target="_blank"}  

---
