---
template: post
title: Jekyll 설치하기
image: img/cover.jpg
author: [Ghost]
date: 2017-11-02 13:45:30 +09:00
tags:
  - 'jekyll'
  - 'github'
  - 'github_pages'
---

# 개요
이 포스팅에서는 Jekyll을 설치하고 GitHub에 적용하는 방법에 대해 소개하고자 합니다.

![github-jekyll-logo](img/github-jekyll-logo.png)

# 필요한 Software
- Ruby
-- Ruby
-- RubyDevKit
-- Jekyll
-- Rouge
- Python
-- Pygements

# Ruby 설치하기
## Ruby 설치
1. [Ruby 홈페이지](https://rubyinstaller.org/downloads)에서 PC환경에 맞는 Ruby 설치파일을 다운로드한다.
![ruby-download](img/ruby-download.png)
2. **I accept the License**를 선택 후 **Next**를 클릭하여 설치한다.
![ruby-install](img/ruby-install.png)

## RubyDevKit 설치
1. [Ruby 홈페이지](https://rubyinstaller.org/downloads)에서 PC환경에 맞는 RubyDevKit 설치파일을 다운로드한다.
![rubydevkit-download](img/rubydevkit-download.png)
2. RubyDevKit을 `C:\RubyDevKit`에 설치한다.
3. CMD창에서 RubyDevKit을 초기화 한다.
```bash
$ cd C:\RubyDevKit
$ ruby dk.rb init
$ ruby dk.rb install
```

## Jekyll 설치
1. CMD창에서 gem을 이용하여 jekyll을 설치한다.
```bash
$ gem install jekyll
```

## Rouge 설치
1. CMD창에서 gem을 이용하여 rouge를 설치한다.
```bash
$ gem install rouge
```

# Python 설치하기
## Python 설치
1. [Python 홈페이지](https://www.python.org/downloads)에서 PC환경에 맞는 Python 설치파일을 다운로드한다.
![python-download](img/python-download.png)

## Pygments 설치
1. CMD창에서 pip를 이용하여 pygments를 설치한다.
```bash
$ pip install Pygments
```

# Jekyll 실행하기
## Jekyll 사이트 생성
1. CMD창에서 Jekyll 사이트를 생성한다.
```bash
$ jekyll new C:\jekyll
```
2. 다음 내용을 `C:\jekyll\config.yml`파일 끝에 추가하여 Jekyll 사이트 환경설을 한다.
```
encoding:utf-8
highlight:rouge
highlighter:Pygments
```
3. Jekyll을 실행한다.
```bash
$ jekyll serve
```
4. Browser에서 [실행한 Jekyll 홈페이지](http://127.0.0.1:4000/)로 접속한다.
