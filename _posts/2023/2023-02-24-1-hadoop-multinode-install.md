---
layout: post
title: Hadoop MultiNode 설치
tags:
    - hadoop
---

#### 준비
MultiNode를 설치를 시작하기 전에 [Hadoop SingleNode 설치](/2023/02/11/1-hadoop-singlenode-install/){: target="_blank"} 을 참고하여 먼저 SingleNode 세팅 후 정상동작을 확인하고 MultiNode를 설정하도록 한다.  

MultiNode이므로  hadoop1 서버는 namenode 서버이고, 나머지 노드는 datanode 서버로 설정한다.  
1번 node는 namenode 와 datanode 모두 사용할 것이다.  


서버 | 구성
---|---
hadoop1 | `namenode`, `datanode`
hadoop2 | `datanode`
hadoop3 | `datanode`
hadoop4 | `datanode`
hadoop5 | `datanode`


각각  node 에 /etc/hosts파일에 hostname을 등록한다. 
```shell
#file : /etc/hosts

192.168.0.1 hadoop1
192.168.0.2 hadoop2
192.168.0.3 hadoop3
192.168.0.4 hadoop4
192.168.0.5 hadoop5
```

/etc/hosts 에  hostname이 잘 등록되었는지 확인한다.  

```shell
    [hadoop1]$ ping hadoop1;
    [hadoop1]$ ping hadoop2;
    [hadoop1]$ ping hadoop3;
    [hadoop1]$ ping hadoop4;
    [hadoop1]$ ping hadoop5;
```


#### 설치 및 환경파일 setting
singleNode 구성 후, multiNode로 변경하는 경우이므로 singleNode 설치에 관한 내용은
[Hadoop SingleNode 설치](/2023/02/11/1-hadoop-singlenode-install/){: target="_blank"} 참고  

ssh공개키 인증을 통하여, 패스워드 없이 namenode(hadoop1)에서 각 node로 ssh 접속이 가능한지 확인한다.  

```shell
[hadoop1]$ ssh hadoop@hadoop1;
[hadoop1]$ ssh hadoop@hadoop2;
[hadoop1]$ ssh hadoop@hadoop3;
[hadoop1]$ ssh hadoop@hadoop4;
[hadoop1]$ ssh hadoop@hadoop5;
```

패스워드 없이 ssh 접속이 불가능할 경우 각 node에서 ssh key 생성, namenode(hadoop1)의 ssh키를 각 node 에 배포해야 한다.  

* 각 node에서 ssh-key생성 및 authorized_keys 파일 생성

 ```shell
 # ssh-key 생성
 [hadoop1]$ ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa;
 ....
 [hadoop5]$ ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa;

 # authorized_keys파일생성
 [hadoop1]$ touch ~/.ssh/authorized_keys;
 [hadoop1]$ chmod 0600 ~/.ssh/authorized_keys;
 ....
 [hadoop5]$ touch ~/.ssh/authorized_keys;
 [hadoop5]$ chmod 0600 ~/.ssh/authorized_keys;
 ```

* namenode의 ssh 공개키를 각 노드에 배포 

```shell
[hadoop1]$ ssh-copy-id -i ~/.ssh/id_rsa.pub hadoop@hadoop1;
[hadoop1]$ ssh-copy-id -i ~/.ssh/id_rsa.pub hadoop@hadoop2;
[hadoop1]$ ssh-copy-id -i ~/.ssh/id_rsa.pub hadoop@hadoop3;
[hadoop1]$ ssh-copy-id -i ~/.ssh/id_rsa.pub hadoop@hadoop4;
[hadoop1]$ ssh-copy-id -i ~/.ssh/id_rsa.pub hadoop@hadoop5;
```

* namenode에서 각 cluster node에 password 없이 접속 가능한지 확인
```shell
$ ssh hadoop@hadoop1;
...
$ ssh hadoop@hadoop5;
```


namenode(hadoop1) 에서 설정파일을 아래와 같이 설정한다.
[Hadoop SingleNode 설치](/2023/02/11/1-hadoop-singlenode-install/){: target="_blank"} 에서 설정한 내용을 바탕으로 없는   
항목은 추가하여야 하고, 이미 설정된 항목에 대해서는 수정하여야 한다.  

* $(HADOOP_HOME)/etc/hadoop/core-site.xml  

``` xml
<configuration>
    <!-- fs.default.name 은 MultiNode Cluster 의 경우, namenode URI로 설정한다. -->  
   <property> 
      <name>fs.default.name</name> 
      <value>hdfs://hadoop1:9000/</value> 
   </property>

    <!-- 
        dfs.replication 설정은 node 갯수에 맞춰서 홀수로 설정한다. 
        파일이 생성될 때 실제 복제 수를 지정할 수 있습니다. 
        생성 시 복제가 지정되지 않은 경우 기본값이 사용됩니다.
    -->
    <property>
      <name>dfs.replication</name>
      <value>3</value>
    </property>

</configuration>
```

masters File을 생성하고, masternode(namenode) 의 hostname을 등록한다. 

```shell
[hadoop1]$ touch $HADOOP_HOME/etc/hadoop/masters;
```

* $HADOOP_HOME/etc/hadoop/masters  

```shell
hadoop1
```

slaves File 을 생성하고, slavenode(datanode) 의 hostname을 등록한다.  

```shell
[hadoop1]$ touch $HADOOP_HOME/etc/hadoop/slaves;
```

* $HADOOP_HOME/etc/hadoop/slaves  

```shell
hadoop1
hadoop2
hadoop3
hadoop4
hadoop5
```

설정이 완료되었으면, 설정파일을 수정한 masternode(hadoop1)에서  $HADOOP_HOME 디렉토리를 압축하여 Cluster 의 모든 Node에 배포한다.  

```shell
[hadoop1]$ tar -cvf hadoop.tar $HADOOP_HOME;

[hadoop1]$ scp ./hadoop.tar hadoop@hadoop2:~/hadoop.tar;
....
[hadoop1]$ scp ./hadoop.tar hadoop@hadoop5:~/hadoop.tar;
```

masternode(hadoop1) 에서 전송받은 압축파일을 각 node에서 압축해제 한다.  

```shell
[hadoop2]$ tar -cvf ~/hadoop.tar;
....
[hadoop5]$ tar -cvf ~/hadoop.tar;
```

#### 실행 
실행 전 hadoop 을 설치하고 namenode 를 최초 기동하는 경우라면, 기동전에 namenode format 을 해줘야 한다.  


```shell
[hadoop1]$ $HADOOP_HOME/bin/hdfs namenode -format;
```

모든 Node에서 hadoop을 실행한다. 

```shell
# hadoop.sh 에서 masters, slaves 파일의 설정을 바탕으로 각 node 마다 알아서 namenode, datanode 가 실행된다.
[hadoop1]$ $HADOOP_HOME/sbin/hadoop.sh start;
....
[hadoop5]$ $HADOOP_HOME/sbin/hadoop.sh start;
```

#### Experience

---

#### Links
[hadoop2,3 버전 port List 정리](https://vnvn31.tistory.com/entry/Hadoop-23%EB%B2%84%EC%A0%84-port-list-%EC%A0%95%EB%A6%AC){: target="_blank"}<br>
[core-default.xml 설정값](https://hadoop.apache.org/docs/r3.3.4/hadoop-project-dist/hadoop-common/core-default.xml){: target="_blank"}<br>
[hdfs-default.xml 설정값](https://hadoop.apache.org/docs/r3.3.4/hadoop-project-dist/hadoop-hdfs/hdfs-default.xml){: target="_blank"}<br>

---