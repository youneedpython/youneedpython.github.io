---
title: "[CI/CD] 기업들의 CI/CD 배포 전략"
description: "[CI/CD] 기업들의 CI/CD 배포 전략"
author: "youneedpython"
date: "2025-03-05 20:56:13:00 +0900" 
categories: [개발, CI/CD]
tags: [CI/CD, 배포, 배포 전략략]
pin: true
math: true
mermaid: true
---

<br><br>


# 기업들의 CI/CD 배포 전략

CI/CD 자동 배포를 설정할 때, 너무 자주 배포되지 않도록 **배포 전략**을 잘 설계해야 한다.    
각 기업마다 **배포 빈도와 방식**이 다르지만, 일반적으로 몇 가지 전략을 사용한다.  

---

## ✅ **기업들의 CI/CD 배포 전략**
### 🔹 **1. 특정 브랜치에만 배포 (보편적인 방법)**
현재 **모든 push마다 배포**되도록 설정했다면, **특정 브랜치에 push될 때만 배포**하도록 조정할 수 있다.

💡 **예시: `main` 또는 `release` 브랜치에서만 배포**  
- `feature/*`, `dev` 브랜치에서는 테스트만 실행  
- `main` 또는 `release` 브랜치에 병합될 때만 배포  

```yaml
stages:
  - test
  - deploy

test:
  script:
    - echo "Running tests..."
  only:
    - dev  # 개발 브랜치에서 테스트만 실행

deploy:
  script:
    - echo "Deploying..."
  only:
    - main  # 배포는 main 브랜치에서만 실행
```
📌 **대부분의 기업에서 사용하는 기본적인 CI/CD 전략**

---

### 🔹 **2. 태그 기반 배포 (버전 태그만 배포)**
Git 태그를 생성할 때만 배포되도록 설정하면, **"원하는 시점에만 배포"**가 가능하다.

💡 **예시: `v1.0.0`, `v1.1.0` 같은 태그가 push될 때만 배포**
```yaml
deploy:
  script:
    - echo "Deploying..."
  only:
    - tags  # 태그가 push될 때만 배포
```
📌 **대규모 서비스에서는 이 방식을 많이 사용**  
📌 GitHub, GitLab, AWS, Google Cloud 등의 CI/CD에서 지원된다.

---

### 🔹 **3. 일정 시간 간격으로 배포 (배치 배포 / 스케줄 배포)**
배포를 **하루에 한 번 또는 특정 시간마다 실행**하는 방법

💡 **예시: 매일 밤 2시에 배포**
```yaml
deploy:
  script:
    - echo "Deploying..."
  only:
    - schedules
```
📌 **예: 쿠팡, 배달의민족 같은 대형 서비스들은 새벽 시간대에 배포하는 경우가 많다.**

---

### 🔹 **4. 수동 배포 (승인 후 배포)**
배포 전에 **개발팀에서 직접 승인**해야 배포가 이루어지도록 설정할 수도 있다.  
GitLab의 **"Manual Job"**, GitHub Actions의 **"workflow_dispatch"** 같은 기능을 사용된된다.

💡 **GitLab 예시 (수동 배포 설정)**
```yaml
deploy:
  script:
    - echo "Deploying..."
  when: manual  # 수동으로 실행해야 배포됨
  only:
    - main
```
📌 **금융권, 공공기관, 대기업에서는 보통 이 방식을 많이 사용한다.**

---

### 🔹 **5. 배포 빈도를 줄이기 위한 Canary / Blue-Green 배포**
**대규모 트래픽 서비스**에서는 **모든 사용자가 동시에 새로운 버전을 쓰는 걸 방지**하기 위해 **점진적 배포**를 한다.  
예를 들어, **네이버, 카카오, 쿠팡 같은 기업**에서는 **Canary 배포** 또는 **Blue-Green 배포**를 많이 사용한다.

- **Canary 배포**: 일부 사용자에게만 새 버전을 배포하고, 문제가 없으면 점진적으로 확대  
- **Blue-Green 배포**: 기존 서버(Blue)와 새 서버(Green)를 동시에 운영하고, 점진적으로 트래픽을 이동  

📌 **이 방식은 Kubernetes, AWS ECS, Google Cloud 등의 클라우드 환경에서 많이 사용된다.**

---

## 🚀 **결론: 어떤 배포 전략을 사용할까?**
현재 **"push할 때마다 배포"**되도록 설정했다면,  
기업에서는 **보통 아래처럼 설정**합니다.

✅ **중소형 프로젝트** → `main` 브랜치에 merge될 때만 배포  
✅ **스타트업 / 빠른 배포가 필요한 서비스** → 하루에 여러 번 자동 배포  
✅ **대기업 / 안정성이 중요한 서비스** → Canary 배포 or 태그 기반 배포  
✅ **금융 / 공공기관 / 민감한 서비스** → 수동 승인 후 배포  

📌 **추천:**  
**"main 브랜치에 push될 때만 배포"** or **"태그를 만들었을 때만 배포"**하는 방식이 가장 적절할 수 있다! 😊  

---

## 🎯 **실제 적용 예시 (배포 방식 변경)**
✔ **방법 1: `main` 브랜치에서만 배포**
```yaml
deploy:
  script:
    - echo "Deploying..."
  only:
    - main  # main 브랜치에 push될 때만 배포
```

✔ **방법 2: 태그(`v1.0.0`)가 push될 때만 배포**
```yaml
deploy:
  script:
    - echo "Deploying..."
  only:
    - tags
```

✔ **방법 3: 하루 한 번 새벽에 배포 (스케줄 배포)**
```yaml
deploy:
  script:
    - echo "Deploying..."
  only:
    - schedules
```

---

## 🎯 **마무리**
- **매번 배포되면 부담이 크므로 `main` 브랜치나 태그 기반 배포를 고려하세요!**  
- **대기업들은 주로 Canary 배포, Blue-Green 배포, 태그 기반 배포 등을 활용합니다.**  
- **GitLab CI/CD, GitHub Actions에서 특정 브랜치/태그에서만 배포되도록 설정 가능.**  


