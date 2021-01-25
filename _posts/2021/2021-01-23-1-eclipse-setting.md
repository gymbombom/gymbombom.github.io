---
layout: post
title: 이클립스(eclipse) 최초설정
tags :
    - eclipse
    - 이클립스
---

## 개요
* eclipse 최초설정방법 기록

---

## 설정

1. HeapMemory 사이즈 설정

```
$ECLIPSE_HOME\eclipse.ini
-Xms1024m
-Xmx1024m
로 설정
```


2. Virtual Machine 설정

```
$ECLIPSE_HOME\eclipse.ini
-vm
D:\JAVA\bin\javaw.exe
설정
-vmargs 이전 라인에 있어야한다.
```


3. maven Repository 설정

```
내가 사용할 Local Maven Repository 디렉토리 생성
Local Maven Repository 에 settings.xml 파일 생성
settings.xml 의 내용은 하단 xml 을 참조하여 작성한다.
Window - Preference - Maven - User Settings - User Settings의 항목을 내가 사용할 Repository의 settings.xml로 변경
Update Settings 클릭하여 update
```

```xml
<!-- settings.xml -->
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">

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


4. Heap Status 표시

```
Window-Preferences-General-Show heap status 항목체크
```

5. Import Folding 해제

```
Window-Preferences-Java-Editor-Folding에서 Imports 체크해제
```


---

## Optional

1. download 된 Maven 연동

```
  1-1. Maven 다운로드
  1-2. Window-Preference-Maven-Installations
```

---


---

## 경험

---

## Links

---












