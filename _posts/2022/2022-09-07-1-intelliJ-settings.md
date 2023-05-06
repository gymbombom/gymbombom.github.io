---
layout: post
title: 인텔리제이(intelliJ) 설정
tags :
    - 인텔리제이
    - intelliJ
---

#### quarantine 속성 제거
* Mac OS의 경우, "확인되지 않은 개발자가 배포했기 때문에 열 수 없습니다" 문제로 인해 App 이 실행불가 하므로 quarantine 속성을 제거해야 한다.

```shell
$ xattr -d com.apple.quarantine ./IntelliJ IDEA.app
```


#### portable 설정
* ./IntelliJ IDEA.app/Contents/bin/idea.properties 파일 열기(Mac OS의 경우)
* config, system, plugins, log path를 원하는 디렉토리로 설정

```properties

# config path 변경
# ex) idea.config.path=변경을 원하는 config 디렉토리 위치
#---------------------------------------------------------------------
# 설정 디렉토리에 대한 경로를 사용자 정의하려면 이 옵션의 주석 처리를 제거하십시오.
# Uncomment this option if you want to customize a path to the settings directory.
#---------------------------------------------------------------------
idea.config.path=../../../config

# system path 변경 
# ex) idea.system.path=변경을 원하는 system 디렉토리 위치
#---------------------------------------------------------------------
# 캐시 디렉토리에 대한 경로를 사용자 정의하려면 이 옵션의 주석 처리를 제거하십시오.
# Uncomment this option if you want to customize a path to the caches directory.
#---------------------------------------------------------------------
idea.system.path=${idea.config.path}/system

# plugins path 변경
# ex) idea.plugins.path=변경을 원하는 plugins 디렉토리 위치
#---------------------------------------------------------------------
# 사용자 설치 플러그인 디렉토리에 대한 경로를 사용자 정의하려면 이 옵션의 주석 처리를 제거하십시오.
# Uncomment this option if you want to customize a path to the user-installed plugins directory.
#---------------------------------------------------------------------
idea.plugins.path=${idea.config.path}/plugins

# log path 변경
# ex) idea.log.path=변경을 원하는 log 디렉토리 위치
#---------------------------------------------------------------------
# 로그 디렉토리에 대한 경로를 사용자 정의하려면 이 옵션의 주석 처리를 제거하십시오.
# Uncomment this option if you want to customize a path to the logs directory.
#---------------------------------------------------------------------
idea.log.path=${idea.system.path}/log

```

---

#### MAVEN REPOSITORY 설정
* 내가 사용할 Local Maven Repository 디렉토리 생성
```shell
$ mkdir maven_repos
```

* Local Maven Repository 에 settings.xml 파일 생성 후 ```<localRepository>``` 항목의 path 변경

```xml
<!-- settings.xml -->


<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">

<!-- 해당 항목의 path 변경 -->
<localRepository>C:\Users\user\Desktop\dev\git\project\maven_local_repository</localRepository> 
<interactiveMode>true</interactiveMode>
<offline>false</offline>

  <profiles>
    <profile>
      <id>myprofile</id>
      <repositories>
        <repository>
          <releases>
            <enabled>true</enabled>
          </releases>
          <id>central</id>
          <url>https://repo.maven.apache.org/maven2</url>
        </repository>
      </repositories>

      <pluginRepositories>
        <pluginRepository>
          <releases>
            <enabled>true</enabled>
          </releases>
          <id>central</id>
          <url>https://repo.maven.apache.org/maven2</url>
        </pluginRepository>
      </pluginRepositories>

    </profile>
  </profiles>

  <activeProfiles>
    <activeProfile>myprofile</activeProfile>
  </activeProfiles>

</settings>
```

* IntelliJ IDEA > Settings > Preferences > Build, Execution, Deployment > Build Tools > Maven > User settings file 항목수정<br>
<img src="/images/posts/19.png" width='800' height='700'>


---

#### SDK 설정
* File > Project Structure > SDK's > Add JDK.. 클릭 후 JDK 추가<br>
<img src="/images/posts/5.png" width='800' height='700'>
* File > Project Structure > Project 에서 해당 Project에서 사용할 SDK 및 Language Level 설정<br>
<img src="/images/posts/20.png" width='800' height='700'>

---

#### Code Folding 해제
* Preferences > Editor > General > Code Folding 클릭
* Show code folding outline, Imports, One-line methods 체크해제
<img src="/images/posts/6.png" width='800' height='600'>

---

#### Links
* [인텔리제이 다운로드](https://www.jetbrains.com/ko-kr/idea/download/other.html){: target="_blank"}
---