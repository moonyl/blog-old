---
title: Hexo 블로깅 환경 구축
date: 2019-09-21 10:52:54
categories:
  - writing
tags:
  - hexo
  - 환경 구축
last_modified_at: 2019-09-21T10:52:54-05:00
---
# Hexo 블로깅 환경 구축
우선 hexo를 설치한다.
```bash
npm install -g hexo-cli 
```
hexo를 통해 블로깅을 할 수 있는 환경을 만든다.
```bash
hexo init myblog
cd myblog
npm install
```
myblog라는 디렉토리에 hexo 환경을 만들게 된다. 이 디렉토리로 들어가서 개발에 필요한 패키지들을 설치하는 것이다.
새로운 글을 작성하기 위해서는 new 명령을 사용한다.
```bash
hexo new "first article"
```
이름이 first-article.md인 파일을 생성하게 된다. 위치는 _config.yml에서 설정한 내용에 따라 조금 다를 수는 있지만, _post 라는 디렉토리 아래의 하나가 된다.
생성된 .md파일에서 마크다운을 이용해서 글을 작성한다. 일반 텍스트 에디터를 이용해도 되나, 이왕이면 마크다운 에디터를 사용하면 좀더 쉽게 글쓰기를 할 수 있다. 지금 사용하는 에디터는 HackMD 이다.
_config.yml 파일을 수정해서, github.io에 설치할 수 있도록 한다.
```yaml
deploy:
    type: git
    repo: https://github.com/moonyl/moonyl.github.io.git
    branch: master
```
moonyl은 필자의 github 계정이다.
웹에 배포하기 위해서는 정적 파일로 만들어줘야 한다.
```bash
hexo generate
```
정적 파일은 public이라는 디렉토리에 생성된다.
생성한 정적파일을 실제 github.io로 설치하기 위해서는 개발용 패키지가 하나 필요하다.
```bash
npm install hexo-deployer-git --save
```
최종 결과파일을 설치한다.
```bash
hexo deploy
```