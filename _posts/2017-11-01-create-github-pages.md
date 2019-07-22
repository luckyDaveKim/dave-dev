---
layout: post
current: post
navigation: true
class: post-template
subclass: 'post tag-getting-started'

category: Git
tags:
  - github
  - github_pages

date: 2017-11-01 23:32:43 +09:00
title: GitHub Pages 생성
cover: assets/posts/2017-11-01/create-github-pages/cover.jpeg
---

# 개요
이 포스팅에서는 GitHub Pages를 생성하는 방법에 대해 소개하고자 합니다.

![github-pages-logo](/assets/posts/2017-11-01/create-github-pages/github-pages.jpg)

# GitHub 계정생성
[GitHub 홈페이지](https://github.com)에서 계정생성한다.

# GitHub Pages 생성
## Pages를 위한 Repository 생성
1. [GitHub Repository 생성](https://github.com/new)에서 repository를 새성한다.
2. **Repository name**에 자신의 github 계정명.github.io `luckyDaveKim.github.io`를 기입한다.
3. (선택) **Description**에 repository 설명 `Dave dev Blog`을 기입한다.
4. **Public**을 선택한다.
5. **Create repository**를 클릭한다.

![create-github-repository](/assets/posts/2017-11-01/create-github-pages/create-github-repository.png)

## HTML, Javascript, CSS 파일 추가
1. Bash 명령어를 통해 github 저장소를 clone 한다.
```bash
git clone https://github.com/luckyDaveKim/luckyDaveKim.github.io
```
2. HTML 소스를 작성한다.
```html
github 테스트 페이지
```
3. 작성한 소스를 commit 및 push 한다.
```bash
git add *
git commit - m "init blog"
git push
```

## 자신의 GitHub Pages에 접속
1. https://github 계정명.github.io `https://luckyDaveKim.github.io`에 접속하여 생성된 github pages를 확인한다.

# 테마 선택
1. **Settings**탭을 선택한다.
![repository-setting](/assets/posts/2017-11-01/create-github-pages/repository-setting.png)
2. **GitHub Pages**에서 **Choose a theme**를 클릭한다.
![github-pages-setting](/assets/posts/2017-11-01/create-github-pages/github-pages-setting.png)
3. 원하는 테마를 클릭 후 **Select theme**를 클릭한다.
![choose-repository-theme](/assets/posts/2017-11-01/create-github-pages/choose-repository-theme.png)