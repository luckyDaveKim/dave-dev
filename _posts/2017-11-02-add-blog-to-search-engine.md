---
layout: post
current: post
navigation: true
class: post-template
subclass: 'post tag-getting-started'

category: Jekyll
tags:
  - jekyll
  - search_engine
  - github
  - github_pages
  - google
  - naver

date: 2017-11-02 13:48:18 +09:00
title: 검색엔진에서 페이지 검색 가능하게 하기
cover: assets/posts/2017-11-02/add-blog-to-search-engine/cover.jpg
---

# 개요
이 포스팅에서는 검색엔진에서 페이지 검색이 가능하도록 등로갛는 방법에 대해 소개하고자 합니다.

# 왜 이런 일을 해야 하나요?
기본 옵션의 GitHub Pages는 일반 포털사이트의 블로그와 달리 검색엔진에서 검색이 되지 않는다. 즉, 검색엔진에 등록하지 않으면 혼자만 사용하는 홈페이지가 되는 것이다. 다양한 사람과 소통하기 위해 만든 홈페이지라면 검색엔진에 등록하는 것이 필수이다.

# 검색엔진에 등록하기에 앞서...
검색엔진에 등록하기 전 다음과 같은 사전 준비가 필요하다.
## **sitemap.xml** 추가
sitemap.xml이란 검색 로봇이 홈페이지에서 어느길로 가야할지 알려주는 이정표와 같은 역할을 한다.
1. [sitemap.xml 내용](https://github.com/luckyDaveKim/luckyDaveKim.github.io/blob/master/sitemap.xml)과 동일하게 **sitemap.xml** 파일을 추가한다.
2. `주의사항!!!` - 루트 디렉토리에 존재하는 _config.yml 파일 내부의 url에 자신의 홈페이지 url `url: https://luckydavekim.github.io`을 입력해야 sitemap.xml이 정상 동작한다.

## **robots.txt** 추가
robots.txt는 검색 로봇이 접근 범위를 지정해주는 역할을 한다.
1. [robots.txt 내용](https://github.com/luckyDaveKim/luckyDaveKim.github.io/blob/master/robots.txt)를 참고하여 **robots.txt** 파일을 추가한다.

# Google 검색엔진에 등록하기
Google 검색엔진에 등록은 Google 웹마스터 도구(Search Console)를 통해 등록할 수 있다.
1. [Google 웹마스터 도구](https://www.google.com/webmasters/tools/home?hl=ko)에 접속한다.
2. **속성추가**를 클릭하여 자신의 홈페이지 주소를 입력한다.
3. 좌측메뉴에서 크롤링 > **Sitemaps**를 클릭한다.
![google-goto-check-sitemap](/assets/posts/2017-11-02/add-blog-to-search-engine/google-goto-check-sitemap.png)
4. 우측 상단의 **SITEMAP 추가/테스트**를 클릭한다.
5. 자신의 홈페이지 sitemap.xml 주소`https://luckydavekim.github.io/sitemap.xml`를 입력한다.

# Naver 검색엔진에 등록하기
Naver 검색엔진에 등록은 Naver 웹마스터 도구를 통해 등록할 수 있다.
1. [Naver 웹마스터 도구](http://webmastertool.naver.com/board/main.naver)에 접속한다.
2. **사이트 간단 체크**를 통하여 자신의 홈페이지 등록 가능 여부를 확인한다.
![naver-check-site](/assets/posts/2017-11-02/add-blog-to-search-engine/naver-check-site.png)
3. 사이트 체크를 통과하였을 경우 하단의 **조회한 사이트 소유확인 하기**를 클릭한다.
![naver-goto-check-site-own](/assets/posts/2017-11-02/add-blog-to-search-engine/naver-goto-check-site-own.png)
4. HTML 파일 업로드, HTML 태그 중 원하는 인증 방식을 선택하여 인증한다.
![naver-check-site-own](/assets/posts/2017-11-02/add-blog-to-search-engine/naver-check-site-own.png)