---
title:  "GitBlog ㅣ 0. 왜 Git Blog인가"
excerpt: "GitHub Pages의 개념과 선택 이유"

categories:
  - GitBlog
tags:
  - GitBlog
last_modified_at: 2024-11-11

toc: true
toc_label: ""
toc_sticky: true
---



# 0. 왜 Git Blog인가
## Customizing
이전에는 Notion, Velog 등의 플랫폼을 사용했는데, 특정 기능에 불편함을 느껴도 이를 해결하기 어려웠다.

예를 들어 Notion은 인터페이스와 컨텐츠 구조가 블로그의 흐름과는 달라 직관적이지 않고 무겁게 느껴졌다. 게다가 서비스가 불안정할 때 보여주는 엄청난 로딩 속도나, 제한적인 검색 기능 등이 특히 불편했다. Velog는 레이아웃과 기능이 제한적이어서 자주 손이 가지 않았다.
이에 커스터마이징이 가능한 Git Blog를 고려했다.

## Markdown
이런 맥락으로 그동안 주로 Obsidian을 활용해 Markdown으로 공부한 내용을 정리했다.
비록 웹 호스팅은 어렵지만 상기 나열했던 단점들을 모두 극복할 수 있었기 때문이다. 특히 로컬 상에서 Markdown으로 빠르고 간편하게 글을 작성하고 확인할 수 있는 점이 좋았다.

Git Blog는 이런 장점을 그대로 가져가면서도 웹에 호스팅할 수 있다는 점이 긍정적으로 다가왔다. 특히 Markdown 문법을 활용하기 때문에, 필요 시 (Obsidian에서 Markdown으로 작성된) 기존 노트 내용을 별도의 작업 없이 빠르고 편리하게 옮길 수 있다는 점이 컸다.

## Git
Git의 버전 관리 시스템을 그대로 활용할 수 있어 편리하다.
또 글을 작성할 때마다 커밋 기록이 쌓인다!


# 1. GitHub Pages
GitHub Pages는 GitHub에서 제공하는 무료 웹 호스팅 서비스로, 주로 정적 웹사이트를 호스팅하는데 사용된다.

## 정적 사이트란
정적 사이트란 미리 만들어진 HTML, CSS, JavaScript 파일을 그대로 클라이언트에 전달해 보여주는 웹사이트를 의미한다. 데이터베이스와 서버의 별도 처리 없이 이미 페이지가 구성이 완료되어 있다. 이렇듯 모든 사용자에게 동일한 HTML을 제공하는 특징을 <mark>static 하다</mark>고 한다. 따라서 블로그, 포트폴리오 등과 같이 자주 변하지 않거나 사용자 입력을 많이 받지 않는 사이트에 적합하다.

이런 특징으로 많은 사람들이 GitHub Pages를 사용해 블로그를 게시해서 Git Blog라고도 부른다.

## GitHub Pages의 특징 : Jekyll 통합
GitHub Pages는 Jekyll이라는 정적 사이트 생성기와 통합되어 있어, 마크다운 파일을 사용해 블로그나 문서화 사이트를 쉽게 만들 수 있다.

# 2. Jekyll
Jekyll은 정적 사이트 생성기로, 마크다운이나 HTML 파일을 정적 웹사이트 형태로 변환해주는 툴이다. Ruby 언어로 작성되며, 데이터베이스나 백엔드 없이도 정적 웹사이트를 쉽게 구축할 수 있다.

## Git Blog와 Jekyll
Jekyll은 GitHub뿐만 아니라 다양한 서버에서 독립적으로 호스팅할 수 있다. 로컬 컴퓨터에 Ruby 환경을 설치한 후 Jekyll을 설치해 정적 파일을 빌드하고, 그 결과물을 어떤 서버든 올리기만 하면 된다.

GitHub Pages는 내부적으로 Jekyll를 지원하기 때문에, 별도의 서버 설정 없이도 Jekyll 사이트가 잘 작동하도록 최적화되어 있다. 따라서 마크다운 파일로 글을 작성해 깃에 푸시하기만 하면, GitHub Pages의 Jekyll이 이를 인식해 html로 변환해 자동으로 웹호스팅을 한다. 즉 GitHub Pages를 통해 사이트를 운영하는 것만이 목표라면 Jekyll을 꼭 설치할 필요는 없다. (로컬 컴퓨터에 설치하지 않아도 자동으로 작동한다)

그러나 개발 효율성, 미리보기 기능 등의 측면에서 Jekyll을 로컬에 설치해 사용하는 것을 추천한다.
Jekyll을 설치하면 로컬 환경에서 사이트를 미리 볼 수 있어, GitHub에 푸시하기 전 사이트가 어떻게 보일지 쉽게 확인하고 수정할 수 있다. 이를 통해 훨씬 수월하고 안정적으로 작업이 가능하다.

# References
- [🔗Blog : devinlife](https://devinlife.com/howto/)