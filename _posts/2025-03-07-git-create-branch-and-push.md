---
title: "[Git] 새로운 브랜치 생성과 Push"
description: "깃 새로운 브랜치 생성과 Push"
author: "youneedpython"
date: "2025-03-07 21:15:02:00 +0900" 
categories: [Tech, Git, branch]
tags: [Git, branch, create, 깃, 깃허브, 브랜치 생성, 브랜치 추가, 브랜치 만들기]
pin: false
math: true
mermaid: true
---

<br/><br/>

# 새로운 브랜치 생성 및 Push 방법

<br/><br/>

## 🔹 1️⃣ 새로운 브랜치 생성

### 📌 새로운 브랜치 만들기

```sh
git branch feature-new-function
```

👉 **`feature-new-function`이라는 새 브랜치를 생성** (하지만 아직 체크아웃하지 않음)

---

## 🔹 2️⃣ 생성한 브랜치로 이동 (`checkout`)

```sh
git checkout feature-new-function
```

또는 단축 명령어:

```sh
git switch feature-new-function
```

📌 이제 **현재 작업 브랜치가 `feature-new-function`으로 변경됨**

👉 `git branch`를 실행하면 `* feature-new-function`으로 표시됨

---

## 🔹 3️⃣ 새로운 브랜치를 만들면서 바로 이동 (`-b` 옵션)

위 과정을 한 번에 실행하는 명령어:

```sh
git checkout -b feature-new-function
```

또는 단축 명령어:

```sh
git switch -c feature-new-function
```

📌 **새 브랜치를 만들고 바로 전환**  
📌 기존 브랜치에서 새로운 브랜치를 생성하고 바로 작업할 때 유용

---

## 🔹 4️⃣ 기능 추가 후 변경 사항 확인

파일을 수정하고 변경 사항을 확인:

```sh
git status
```

📌 **출력 예시**

```sh
On branch feature-new-function
Changes not staged for commit:
  modified: src/main/java/com/example/App.java
```

👉 현재 **`feature-new-function` 브랜치에서 코드 변경이 감지됨**

---

## 🔹 5️⃣ 변경 사항을 커밋

```sh
git add .
git commit -m "feat: add new function"
```

📌 **변경된 파일을 스테이징하고 커밋**

---

## 🔹 6️⃣ 새 브랜치를 원격 저장소로 push

```sh
git push --set-upstream origin feature-new-function
```

또는 단축 명령어:

```sh
git push -u origin feature-new-function
```

📌 **이 명령어가 하는 일**

- 원격 저장소(`origin`)에 `feature-new-function` 브랜치를 push
- `--set-upstream` 옵션을 사용하여 이후 `git push`만 실행해도 자동으로 원격 `feature-new-function` 브랜치에 push됨

---

## 🎯 최종 정리

| 단계 | 명령어 | 설명 |
| --- | --- | --- |
| **새 브랜치 생성** | `git branch feature-new-function` | 새 브랜치 생성 (이동은 안 됨) |
| **새 브랜치로 이동** | `git checkout feature-new-function` 또는 `git switch feature-new-function` | 해당 브랜치로 전환 |
| **새 브랜치 생성 + 이동 (한 번에)** | `git checkout -b feature-new-function` 또는 `git switch -c feature-new-function` | 브랜치를 만들면서 바로 이동 |
| **파일 수정 후 커밋** | `git add . && git commit -m "feat: add new function"` | 변경 사항을 저장 |
| **원격 저장소에 push** | `git push -u origin feature-new-function` | 원격에 새 브랜치를 push 및 연결 |

---

## 🚀 완료 후 브랜치 확인

### 📌 로컬 브랜치 목록 확인

```sh
git branch
```

📌 현재 있는 모든 브랜치를 확인 가능  

### 📌 원격 브랜치 목록 확인  

```sh
git branch -r
```

📌 원격 저장소(`origin`)에 있는 브랜치 목록 확인  

---

## 🎯 결론

새로운 기능을 추가하기 위해 브랜치를 만들고 push하려면:

```sh
git checkout -b feature-new-function
# (파일 수정)
git add .
git commit -m "feat: add new function"
git push -u origin feature-new-function
```


