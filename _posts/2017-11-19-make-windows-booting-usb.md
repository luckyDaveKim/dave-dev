---
layout: post
current: post
navigation: true
class: post-template
subclass: 'post tag-getting-started'

category: Windows
tags:
  - windows
  - windows10
  - booting_usb

date: 2017-11-19 02:06:00 +09:00
title: "윈도우10 부팅 USB 만들기"
cover: assets/posts/2017-11-19/make-windows-booting-usb/cover.jpg
---

# 개요
이 포스팅에서는 윈도우10 부팅 USB 만드는 방법에 대해 소개하고자 합니다.

# 준비물
- USB (최소 8GB)
- MediaCreationTool

# USB 포멧
1. USB를 삽입한다.
2. 내컴퓨터에서 삽입된 USB항목을 [오른쪽 클릭] -> [포멧]을 클릭한다.
3. **시작**버튼을 클릭한다.

# MediaCreationTool 설치
1. [Microsoft 홈페이지](https://www.microsoft.com/ko-kr/software-download/windows10)에서 MediaCreationTool을 다운로드 한다.
![media-creation-tool-download](/assets/posts/2017-11-19/make-windows-booting-usb/media-creation-tool-download.png)
2. 다운로드한 MediaCreationTool을 실행한다.
![create-media-init](/assets/posts/2017-11-19/make-windows-booting-usb/create-media-init.png)
3. 원하는 작업을 선택한다.
(다른 PC용 설치 미디어(USB 플래시 드라이브, DVD 또는 ISO 파일) 만들기)
![create-media-choose-work](/assets/posts/2017-11-19/make-windows-booting-usb/create-media-choose-work.png)
4. 부팅 USB로 만들 Windows 정보를 선택한다.
![create-media-choose-version](/assets/posts/2017-11-19/make-windows-booting-usb/create-media-choose-version.png)
5. USB 플래시 드라이브를 선택하여, 부팅 USB 만들 준비를 한다.
![create-media-choose-media-type](/assets/posts/2017-11-19/make-windows-booting-usb/create-media-choose-media-type.png)
6. 부팅 USB로 만들 드라이브를 선택한다.
![create-media-choose-drive](/assets/posts/2017-11-19/make-windows-booting-usb/create-media-choose-drive.png)
7. Windows10 다운로드를 진행한다.
![create-media-install](/assets/posts/2017-11-19/make-windows-booting-usb/create-media-install.png)

# 부팅 USB 생성 완료
 SB를 열어보면 다음과 같이 부팅 Windows USB가 생성되어있다.
![booting-usb](/assets/posts/2017-11-19/make-windows-booting-usb/booting-usb.png)