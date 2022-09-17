---
layout: post
title: 인텔리제이(intelliJ) 설정
tags :
    - 인텔리제이
    - intelliJ
---

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

#### SDK 설정
* File > Project Structure > SDK's > Add JDK.. 클릭 후 JDK 추가<br>
<img src="/images/posts/5.png" width='800' height='700'>

---

#### Code Folding 해제
* Preferences > Editor > General > Code Folding 클릭
* Show code folding outline, Imports, One-line methods 체크해제
<img src="/images/posts/6.png" width='800' height='600'>

---

#### Links
* [인텔리제이 다운로드](https://www.jetbrains.com/ko-kr/idea/download/other.html){: target="_blank"}
---