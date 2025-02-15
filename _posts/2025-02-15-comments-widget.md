---
title: "[블로그에 댓글 기능 추가] GitHub Blog에 Utterances 댓글 위젯 연동하기"
description: "쉽고 간단하게 깃허브 블로그에 댓글 기능 연동하기"
author: "youneedpython"
date: "2025-02-15 12:15:00 +0900" 
categories: [Github blog, comments widget]
tags: [Git, GitHub, leave fork, 포크 연결 끊기, fork repository 연결 끊기기]
pin: true
math: true
mermaid: true
---

<br><br>

![Utterances Comment](/assets/img/2025-02-15/utterances.png)

GitHub Pages를 사용하여 블로그를 운영하는 경우, 별도의 댓글 기능능을 추가하는 것이 고민될 수 있습니다. 일반적인 블로그 플랫폼(예: WordPress, Tistory)과 달리 GitHub Pages는 기본적으로 댓글 기능을 제공하지 않기 때문입니다.

이 글에서는 **[Utterances](https://utteranc.es/){: target="_blank" }**라는 오픈소스 댓글 위젯을 블로그에 연동하는 방법을 알아보겠습니다.



## 1. Utterances란?

Utterances는 **GitHub Issues**를 활용하여 블로그에 댓글 기능을 추가하는 오픈소스 위젯입니다. 즉, 블로그의 각 게시글에 대해 GitHub Issue가 생성되며, 해당 Issue에 댓글을 남기는 방식으로 작동합니다.

### Utterances의 특징
- **무료** 🎉 (광고 없음, 트래킹 없음)
- **GitHub Issues를 활용** 🔐 (데이터가 GitHub에 저장됨)
- **오픈소스** 🔓 (자유롭게 사용 가능)
- **다크 모드 지원** 🌑
- **가벼운 TypeScript 기반** (별도의 JS 프레임워크 필요 없음)

이제 본격적으로 설정 방법을 알아보겠습니다.

<br><br>

## 2. Utterances의 작동 방식 (How it works)
Utterances가 로드되면 **GitHub Issue 검색 API**를 사용하여,  
현재 페이지와 연결된 Issue를 찾습니다.  
이때 검색 기준은 다음과 같습니다:

- **url** (페이지 URL)
- **pathname** (경로명)
- **title** (제목)

👉 **만약 해당 페이지에 연결된 Issue가 없다면**,  
Utterances 봇(**utterances-bot**)이 자동으로 새로운 Issue를 생성합니다.

<br><br>

## 3. Utterances 설정하기

### 3.1 Utterances 앱을 블로그의 GitHub 저장소에 연결

Utterances가 연결될 **GitHub 저장소(Repository)를 선택**하는 방법을 설명합니다.

1. **저장소가 Public인지 확인**
- 저장소가 **Public(공개)** 되어 있어야 합니다.
- 그렇지 않으면 **독자들이 이슈 및 댓글을 볼 수 없습니다.**
- 비공개(private) 저장소에서는 댓글을 볼 수 없습니다.

1. **Utterances 앱 설치 확인**
- 저장소에 **[Utterances 앱](https://github.com/apps/utterances){: target="_blank" } 설치**가 필요합니다.
- **[Utterances 앱](https://github.com/apps/utterances){: target="_blank" } 설치 페이지**에서 설치하세요.
- 설치하지 않으면 **사용자들이 댓글을 작성할 수 없습니다.**

1. **Fork된 저장소라면 추가 설정 필요**
  - **Settings** 탭으로 이동합니다.
  - **Issues 기능이 활성화되어 있는지** 확인합니다.

    > Fork된 저장소는 Issues 메뉴가 비활성화 되어 있습니다.  
    >   Fork 연결 끊는 방법은 [여기](https://youneedpython.github.io/posts/github-fork-disconnect/){: target="_blank" }를 클릭하여 확인  
    {:.prompt-info}


1. **저장소 입력란 설정**
- 저장소의 `Settings` → `Issues` 활성화 확인
- **이 저장소가 블로그 포스트의 이슈 및 댓글이 게시될 위치입니다.**

<br><br>

## 3. 블로그에 Utterances 댓글 위젯 추가하기

### 3.1 스크립트 추가

아래 스크립트를 블로그의 원하는 위치(예: `post.html` 템플릿 파일)에 추가하세요.  
```html
<script src="https://utteranc.es/client.js"
        repo="YOUR_GITHUB_USERNAME/YOUR_REPO_NAME"
        issue-term="pathname"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>
```

**`_layouts/post.html`** 파일 하단에 붙여 넣기기
![Utterances 스크립트 추가](/assets/img/2025-02-15/add script tag.png)

#### 주요 설정 설명
- `repo="YOUR_GITHUB_USERNAME/YOUR_REPO_NAME"`  
  → 댓글을 저장할 GitHub 저장소 설정  
- `issue-term="pathname"`  
  → 댓글을 연결할 기준 (URL 경로를 기준으로 사용)  
- `theme="github-light"`  
  → 댓글 위젯의 테마 설정 (`github-dark` 사용 가능)  
- `async`  
  → 페이지 로딩 속도를 고려한 비동기 설정  

이제 블로그를 실행하면, Utterances가 정상적으로 로드되며 댓글 기능이 활성화됩니다!

<br><br>

## 4. Utterances 댓글 사용법

### 4.1 댓글 작성
- 사용자가 댓글을 남기려면 **GitHub 로그인**이 필요합니다.
- 댓글을 작성하면 자동으로 해당 게시글과 연결된 **GitHub Issue**가 생성됩니다.

### 4.2 댓글 관리
- 블로그 댓글은 GitHub Issues에서 확인할 수 있습니다.
- 댓글을 관리하려면 해당 Issue에서 직접 댓글을 삭제하거나 편집하면 됩니다.

<br><br>

## 5. 마무리

이제 GitHub Pages 블로그에서도 **무료**로 댓글 기능을 추가할 수 있습니다.  
Utterances는 별도 데이터베이스 없이 GitHub Issues를 활용하므로, 유지보수가 간편하며 GitHub 사용자들과 자연스럽게 소통할 수 있습니다.

