---
title: "[git commit convention] 깃 커밋 컨벤션"
description: "깃 커밋 메시지 규칙을 이해하고 사용하자!"
author: "youneedpython"
date: "2025-02-14 19:15:00 +0900" 
categories: [Tech, Git, commit convention]
tags: [Git, GitHub, commit convention, 커밋, 커밋 문법, 커밋 컨벤션, 커밋 규칙]
pin: true
math: true
mermaid: true
image:
  path: /assets/img/2025-02-14/commitlint.png
  width: auto ## 원래 크기 유지
  height: auto ## 비율 유지
  alt: commitlint
---


# Git Commit Convention
- 깔끔한 커밋 메시지를 위한 가이드
- 참고: [commitlint config-conventional](https://github.com/conventional-changelog/commitlint/tree/master/@commitlint/config-conventional#commitlintconfig-conventional){: target="_blank" }을 한글 번역 및 정리한 글입니다.  
<br>

Git을 사용하다 보면 협업을 위해 **`가독성이 좋은 커밋 메시지`**를 작성하는 것이 중요합니다. 이때 많은 개발자들은 **`Git Commit Convention(깃 커밋 컨벤션)`** 을 따르는데, 이는 커밋 메시지를 일정한 형식으로 유지하기 위한 규칙입니다.

이 글에서는 **`@commitlint/config-conventional`** 을 기반으로 **`깃 커밋 컨벤션 규칙(git commit convention rules)`**을 소개합니다.

> [참고 사이트]
- [commitlint rules](https://commitlint.js.org/reference/rules.html){: target="_blank" }
- [commitlint config-conventional](https://github.com/conventional-changelog/commitlint/tree/master/@commitlint/config-conventional#commitlintconfig-conventional){: target="_blank" } 
{: .prompt-info}


---

## 1. Git 커밋 컨벤션이란?

Git 커밋 컨벤션은 커밋 메시지를 일정한 포맷으로 작성하여 코드 변경 내역을 명확하게 기록하는 방법입니다. 

이 컨벤션을 따르면 다음과 같은 이점이 있습니다:

- **가독성이 향상됨**: 변경 사항을 빠르게 파악할 수 있습니다.
- **일관된 스타일 유지**: 팀원 간의 코드 관리가 쉬워집니다.
- **자동화 가능**: 커밋 메시지를 기반으로 변경 로그를 생성할 수 있습니다.

---

## 2. @commitlint/config-conventional 설치하기

**commitlint**는 커밋 메시지가 규칙을 따르는지 검사하는 도구입니다. **@commitlint/config-conventional**을 사용하면 [Conventional Commits](https://www.conventionalcommits.org/) 규칙을 적용할 수 있습니다.

### 설치 방법
다음 명령어를 실행하여 commitlint와 설정 패키지를 설치합니다:

```sh
npm install --save-dev @commitlint/config-conventional @commitlint/cli
```

그리고 프로젝트 루트에 `commitlint.config.js` 파일을 생성하고 다음 내용을 추가합니다:

```sh
export default {extends: ['@commitlint/config-conventional']};" > commitlint.config.js
```

이제 커밋 메시지가 Conventional Commits 규칙을 따르지 않으면 오류가 발생합니다.

---

## 3. Commitlint 규칙 정리

`@commitlint/config-conventional`에서 사용하는 주요 규칙을 살펴보겠습니다.

### 3.1 type(커밋 유형) 검증

commitlint는 특정한 `type(유형)`만 허용합니다. 만약 아래의 유형을 벗어난 커밋 메시지를 작성하면 오류가 발생합니다.

#### 허용되는 커밋 유형 (type)

- `build` : 빌드 시스템 변경 (예: 빌드 파일 수정, 패키지 추가)
- `chore` : 코드 변경 없이 설정 수정 (예: 문서 업데이트, 코드 스타일 변경)
- `ci` : CI 관련 변경 (예: GitHub Actions 설정 변경)
- `docs` : 문서 수정 (예: README.md 업데이트)
- `feat` : 새로운 기능 추가
- `fix` : 버그 수정
- `perf` : 성능 개선
- `refactor` : 리팩토링 (기능 변경 없이 코드 개선)
- `revert` : 이전 커밋 되돌리기
- `style` : 코드 스타일 변경 (예: 공백, 세미콜론 변경 등)
- `test` : 테스트 코드 추가 또는 수정

commitlint는 위 유형 외의 다른 type이 포함된 커밋 메시지를 허용하지 않습니다.

#### 규칙 적용 방식

- **condition:** `type`이 리스트에 포함되어야 함
- **rule:** 항상 적용 (`always`)
- **level:** 오류로 간주 (`error`)

### 3.2 type-case (커밋 유형의 대소문자 규칙)

- `type`은 반드시 **소문자(lowerCase)** 여야 합니다.

예제:
```sh
FIX: some message # fails: FIX 대문자
fix: some message # passes: fix 소문자
```

### 3.3 type-empty (커밋 유형이 비어있으면 안 됨)

- `type`은 반드시 존재해야 합니다.

예제:
```sh
: some message # fails: 타입이 비어있음
fix: some message # passes: 타입이 fix로 입력되어 있음
```

### 3.4 subject-case (커밋 메시지 제목 규칙)

- `subject`는 `sentence-case`, `start-case`, `pascal-case`, `upper-case` 중 하나여야 합니다.
- [참고] Naming convention
  - 예시 문장: hello, my name is hong.
  - `sentence-case` : Hello, my name is hong. or hello, my name is hong.
    - *commitlint에서는 첫 글자가 소문자인 경우도 sentence-case로 허용*
  - `start-case` : Hello, My Name Is Hong.
  - `pascal-case` : Hello, MyNameIsHong.
  - `upper-case` : HELLO, MY NAME IS HONG.

> **Git 커밋 메시지의 구조 (commitlint style)**
> - `<type>(<scope>): <subject>`  
> 
> |---------|---------------------|------------------------|
> | 요소    | 의미                  | 예제                     |
> |---------|---------------------|------------------------|
> | `type`  | 변경 유형 (feat, fix, docs 등) | `fix`                   |
> | `scope` | 변경 범위 (선택적), **`무조건 소문자로 작성`**        | `auth`, `ui`, `database` 등 |
> | `subject` | 변경 내용 요약           | `resolve login issue` |
> |---------|---------------------|------------------------|
>
> - `type` vs `scope` 차이점
> 
> |----------|------------------------------------------|-----------------------------------------|
> | 항목     | 설명                                       | 예제                                     |
> |----------|------------------------------------------|-----------------------------------------|
> | `type`   | **필수**                                 | `feat`, `fix`, `docs`, `chore`, `test` |
> | `scope`  | **선택 사항** (추가 가능)               | `auth`, `ui`, `database`, `API`        |
> | `subject`| **필수** (첫 글자 스타일 규칙 적용)    | `fix login issue`, `add new button`    |
{: .prompt-info }
{: .custom-table }


예제:
```sh
fix(SCOPE): Some message # fails: fix의 SCOPE가 대문자로 되어 있음 (scope는 소문자로 써야 함)
fix(scope): Some message # passes: sentence-case
fix(scope): some message # passes: sentence-case
fix(scope): Some Message # passes: start-case
fix(scope): SomeMessage  # passes: pascal-case
fix(scope): SOMEMESSAGE  # passes: upper-case
fix(scope): SOME message # passes: upper-case + sentence-case 조합
```

### 3.5 subject-empty (커밋 메시지 제목이 비어있으면 안 됨)

- `subject`는 반드시 존재해야 합니다.

예제:
```sh
fix:  # fails: subject 없음
fix: some message # passes: subject 있음
```

### 3.6 subject-full-stop (커밋 메시지 제목의 마침표 사용 금지)

- `subject`는 마침표(`.`)로 끝나면 안 됩니다.

예제:
```sh
fix: some message. # fails: 마침표 사용
fix: some message # passes: 마침표 없음
```

### 3.7 header-max-length (커밋 메시지 길이 제한)

- `header`는 100자 이하로 작성해야 합니다.

예제:
```sh
fix: some message that is way too long and breaks the line max-length rule by several characters # fails
fix: some message # passes: 100글자 이하
```

### 3.8 body-leading-blank (본문 앞 공백 줄 필요)

- `body`는 앞에 공백 줄이 있어야 합니다.

예제:
```sh
fix: some message
body # warning: body 앞에 공백 줄 없음

fix: some message

body # passes: body 앞에 공백 줄 있음
```

### 3.9 body-max-line-length (본문 줄 길이 제한)

- `body`의 각 줄은 100자 이하로 작성해야 합니다.

예제:
```sh
fix: some message
body with multiple lines
has a message that is way too long and will break the line rule 'line-max-length' by several characters # fails

fix: some message
body with multiple lines
but still no line is too long # passes: body가 100글자 이하
```

### 3.10 footer-leading-blank (푸터 앞 공백 줄 필요)

- `footer`는 앞에 공백 줄이 있어야 합니다.

예제:
```sh
fix: some message
BREAKING CHANGE: It will be significant # warning: footer 앞에 공백 없음

fix: some message

BREAKING CHANGE: It will be significant # passes: footer 앞에 공백 있음
```

### 3.11 footer-max-line-length (푸터 줄 길이 제한)

- `footer`의 각 줄은 100자 이하로 작성해야 합니다.

예제:
```sh
fix: some message
BREAKING CHANGE: footer with multiple lines
has a message that is way too long and will break the line rule 'line-max-length' by several characters # fails

fix: some message
BREAKING CHANGE: footer with multiple lines
but still no line is too long # passes
```

---

## 4. 마무리

Git 커밋 컨벤션을 지키면 협업이 훨씬 원활해지고, 코드 변경 사항을 한눈에 파악할 수 있습니다.

