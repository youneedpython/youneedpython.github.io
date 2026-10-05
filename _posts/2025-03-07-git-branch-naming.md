---
title: "[Git] 브랜치 네이밍 컨벤션"
description: "git 브랜치 네이밍 컨벤션"
author: "youneedpython"
date: "2025-03-07 20:45:11:00 +0900" 
categories: [Tech, Git, branch]
tags: [Git, branch, name, naming, 깃, 깃허브, 브랜치 네이밍, 브랜치명]
pin: false
math: true
mermaid: true
---

<br/><br/>

# 브랜치 네이밍 컨벤션

---

## 🔹 1️⃣ 일반적인 브랜치 네이밍 컨벤션

### 📌 1. Git Flow 방식 (가장 널리 사용됨)

Git Flow는 기능 개발, 버그 수정, 배포 등을 체계적으로 관리하는 방식입니다.

| 브랜치 유형 | 접두사 예시 | 설명 |
|------------|-----------|------|
| 기능 개발 | `feature/` | 새로운 기능 추가 |
| 버그 수정 | `fix/` | 버그 수정 |
| 핫픽스 | `hotfix/` | 긴급 수정 (배포 후 발생한 문제 해결) |
| 릴리즈 준비 | `release/` | 배포를 준비하는 브랜치 |
| 실험적 테스트 | `experiment/` | 실험적인 기능 구현 |

📌 **예제**
```sh
git checkout -b feature/add-login-api
git checkout -b fix/fix-navbar-bug
git checkout -b hotfix/urgent-payment-issue
git checkout -b release/v1.2.0
```
👉 **스페이스 대신 `-` 또는 `_`를 사용**하여 가독성을 높임  
👉 **기능 이름을 명확하게 표현**(예: `feature/add-login-api` → "로그인 API 추가 기능 개발")

---

## 🔹 2️⃣ GitHub Flow 방식 (더 간단한 네이밍)

GitHub Flow는 보다 단순한 개발 방식으로, 보통 `main` 브랜치에서 새로운 브랜치를 만들어 작업 후 병합하는 방식입니다.

| 브랜치 유형 | 예시 | 설명 |
|------------|-----------|------|
| 기능 개발 | `add-login-api` | 로그인 API 추가 |
| 버그 수정 | `fix-navbar-bug` | 네비게이션 바 버그 수정 |
| 핫픽스 | `urgent-payment-issue` | 긴급 결제 오류 수정 |

📌 **예제**
```sh
git checkout -b add-login-api
git checkout -b fix-navbar-bug
git checkout -b urgent-payment-issue
```
👉 더 간결한 방식으로, `feature/`, `fix/` 등의 접두사를 생략하는 방식  
👉 작은 프로젝트나 개인 프로젝트에 적합

---

## 🔹 3️⃣ 네이밍 규칙 요약

| 네이밍 스타일 | 예제 | 언제 사용하면 좋을까? |
|--------------|------|----------------|
| `feature/add-login-api` | `feature/add-user-authentication` | Git Flow를 따르는 팀 프로젝트 |
| `fix/navbar-css-issue` | `fix/navbar-layout` | 버그 수정 시 |
| `hotfix/critical-payment-bug` | `hotfix/api-timeout` | 긴급 수정 시 |
| `release/v1.2.0` | `release/next-major-update` | 배포 준비 시 |
| `add-login-api` | `fix-navbar-bug` | 개인 프로젝트나 단순한 협업 |

---

## 🎯 정리

- **협업하는 프로젝트라면** `feature/기능명`, `fix/버그명` 같은 Git Flow 네이밍이 가장 좋음
- **작은 프로젝트라면** `add-login-api`, `fix-navbar-bug` 같은 간단한 네이밍도 가능
- **기능 브랜치를 생성할 때 추천하는 형식**:

```sh
git checkout -b feature/add-login-api
git checkout -b fix/navbar-css-issue
git checkout -b hotfix/urgent-payment-fix
```


