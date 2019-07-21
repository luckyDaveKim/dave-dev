---
layout: post
current: post
navigation: true
class: post-template
subclass: 'post tag-getting-started'

category: Web-Server
tags:
  - jdk
  - jdk8
  - windows

date: 2018-01-28 15:53:25 +09:00
title: "윈도우에서 JDK 8 설치 및 환경설정"
cover: assets/posts/2018-01-28/install-jdk-with-setting-at-windows/cover.jpg
---

# 개요
이 포스팅에서는 JDK 8 설치 및 환경설정하는 방법에 대해 소개하고자 합니다.

![jdk-logo](/assets/posts/2018-01-28/install-jdk-with-setting-at-windows/jdk-java-development-kit.jpg)

# JDK 다운로드
[JDK 다운로드](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)를 클릭하여 JDK를 다운로드 하거나, 혹은 다음 절차를 따라 다운로드 한다.
1. [Oracle 홈페이지](https://www.oracle.com)에 접속한다.
2. 상단 메뉴를 클릭 후 **Java > Java SE**를 클릭한다.
![Java SE 다운로드 메뉴](/assets/posts/2018-01-28/install-jdk-with-setting-at-windows/java-se-download-menu.png)
3. 하단의 **Get Started**에서 **Download Java SE for Developers**를 클릭한다.
![Java SE 다운로드 메뉴](/assets/posts/2018-01-28/install-jdk-with-setting-at-windows/java-se-get-started.png)
4. JDK 8버전을 받을 것이므로 해당 버튼을 클릭한다.
![Java SE 다운로드 메뉴](/assets/posts/2018-01-28/install-jdk-with-setting-at-windows/jdk-download.png)
5. **Accept License Agreement**를 클릭 후, 자신의 윈도우 비트에 맞는 JDK를 클릭하여 다운로드한다.
![Java SE 다운로드 메뉴](/assets/posts/2018-01-28/install-jdk-with-setting-at-windows/jdk-windows-download.png)

# JDK 설치
위에서 다운로드받은 JDK 파일을 다음 절차에 따라 설치한다.
1. **Next** 클릭한다.
![JDK 설치 동의](/assets/posts/2018-01-28/install-jdk-with-setting-at-windows/jdk-install-agreement.png)
2. 원하는 JDK 설치 경로를 지정한다.
![JDK 설치 경로 설정](/assets/posts/2018-01-28/install-jdk-with-setting-at-windows/jdk-install-path-setting.png)
3. JDK 설치완료!
![JDK 설치](/assets/posts/2018-01-28/install-jdk-with-setting-at-windows/jdk-install.png)
4. 원하는 JRE 설치 경로를 지정한다.
![JRD 설치 경로 설정](/assets/posts/2018-01-28/install-jdk-with-setting-at-windows/jre-install-path-setting.png)
5. JRD 설치완료!
![JDK 설치](/assets/posts/2018-01-28/install-jdk-with-setting-at-windows/jre-install.png)
6. Java SE 설치완료!
![Java SE 설치](/assets/posts/2018-01-28/install-jdk-with-setting-at-windows/java-se-installed.png)

# JDK 환경변수 설정
다음 절차에 따라 JDK 환경변수를 설정한다.
1. 키보드에서 **Windows + Pause Break**를 눌러서 시스템창을 띄운후, 좌측의 **고급 시스템 설정**을 클릭한다.
![윈도우 시스템 화면](/assets/posts/2018-01-28/install-jdk-with-setting-at-windows/system.png)
2. 하단의 **환경 변수**를 클릭한다.
![시스템 속성](/assets/posts/2018-01-28/install-jdk-with-setting-at-windows/system-properties.png)
3. **시스템 변수**에서 **새로 만들기**를 클릭한다.
![새로운 변수 추가](/assets/posts/2018-01-28/install-jdk-with-setting-at-windows/local-path-setting.png)
4. **변수 이름**에
`JAVA_HOME`을 입력 하고, **변수 값**에 JDK 설치 경로 `C:\Program Files\Java\jdk1.8.0_161`를 입력후 **확인**버튼을 클릭한다.
![JavaHome 변수 추가](/assets/posts/2018-01-28/install-jdk-with-setting-at-windows/new-java-home-local-path.png)
5. **시스템 변수**에 **Path**를 찾아 선택 후 **편집**을 클릭한다.
![Path 변수 편집](/assets/posts/2018-01-28/install-jdk-with-setting-at-windows/local-path-edit.png)
6. **시스템 변수**에 **Path**를 찾아 선택 후 **편집**을 클릭하여 `%JAVA_HOME%\bin`을 입려하고 확인을 클릭한다.
![Path 변수 추가](/assets/posts/2018-01-28/install-jdk-with-setting-at-windows/local-path-add.png)

# JDK 설치 확인
JDK 설치 및 환경변수 설정이 완료되었는지 다음 절차를 따라 확인한다.
1. 키보드에서 **Windows + R**을 눌러 **실행창**을 띄운후, `cmd`를 입력하고 **확인**버튼을 클릭한다.
![실행창](/assets/posts/2018-01-28/install-jdk-with-setting-at-windows/running.png)
2. **cmd**창이 뜨면 `java -version` 및 `javac -version`을 입력하여 설치한 JDK 버전이 뜨는지 확인한다.
![커맨드창](/assets/posts/2018-01-28/install-jdk-with-setting-at-windows/cmd.png)