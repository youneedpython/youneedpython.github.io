---
title: "[Spring Boot] 프로파일별 `yml` 파일을 다르게 적용하는 방법"
description: "[Spring Boot] 프로파일별 `yml` 파일을 다르게 적용하는 방법"
author: "youneedpython"
date: "2025-03-04 21:22:07:00 +0900" 
categories: [개발, SpringBoot, profile]
tags: [개발, SpringBoot, profile, yml, 설정, 야믈 파일]
pin: true
math: true
mermaid: true
---

<br/><br/>

# Spring Boot에서 프로파일별 `yml` 파일을 다르게 적용하는 방법

Spring Boot에서는 **환경별 설정을 분리**하여 관리할 수 있도록 프로파일(Profile) 기능을 제공한다.  
이를 활용하면 **개발 환경(dev), 운영 환경(prod)** 등 다양한 환경에서 각각 다른 설정을 적용할 수 있다.  
보통 `application.yml`(기본 설정)과 `application-{profile}.yml`(환경별 설정) 파일을 분리하여 관리한다.

---

## 1. Spring Boot의 프로파일별 설정 파일 적용 방식

Spring Boot는 실행 시 **활성화된 프로파일**(`spring.profiles.active`)을 기반으로 `application-{profile}.yml`을 자동으로 적용한다.  
파일의 기본적인 우선순위는 다음과 같다.

### **설정 파일 로드 우선순위**
1. **환경 변수 (`SPRING_PROFILES_ACTIVE`)**  
   - `SPRING_PROFILES_ACTIVE=dev`와 같이 설정하면 `application-dev.yml`이 적용됨.
2. **`application-{profile}.yml` 파일**  
   - `application-dev.yml`, `application-prod.yml` 등의 환경별 설정 파일.
3. **기본 `application.yml` 파일**  
   - 별도의 프로파일이 설정되지 않았을 때 적용됨.

즉, **기본적으로 `application.yml`을 먼저 로드하고**, 그 후 활성화된 프로파일에 따라 `application-{profile}.yml`을 덮어쓴다.

---

## 2. 환경별 YML 파일 구성

프로젝트의 `src/main/resources/` 디렉토리에 아래와 같이 `yml` 파일을 구성할 수 있다.

```
src/main/resources/
 ├── application.yml        # 기본 설정 (운영 환경용)
 ├── application-dev.yml    # 개발 환경 설정
 ├── application-prod.yml   # 운영 환경 설정
```

### **예제: `application.yml` (기본 설정 파일)**
```yaml
server:
  port: 8080
spring:
  profiles:
    active: dev  # 기본적으로 개발 환경을 사용하도록 설정
  datasource:
    url: jdbc:mysql://default-db:3306/mydb
    username: root
    password: secret
```

위 설정에서 `spring.profiles.active=dev`가 기본값으로 설정되어 있으므로,  
별도로 프로파일을 설정하지 않으면 **`application-dev.yml`이 적용됨.**

### **예제: `application-dev.yml` (개발 환경 설정)**
```yaml
server:
  port: 8081
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/devdb
    username: dev_user
    password: dev_password
```

### **예제: `application-prod.yml` (운영 환경 설정)**
```yaml
server:
  port: 9090
spring:
  datasource:
    url: jdbc:mysql://prod-db:3306/mydb
    username: prod_user
    password: prod_password
```

---

## 3. 프로파일별 YML 파일 적용 방법

### **1️⃣ 환경 변수로 설정 (추천)**
운영 환경에서 `SPRING_PROFILES_ACTIVE` 환경 변수를 설정하면, 해당 프로파일의 YML 파일이 적용된다.

#### **Linux / macOS (터미널에서 실행)**
```sh
SPRING_PROFILES_ACTIVE=prod ./gradlew bootRun
```

#### **Windows (PowerShell)**
```powershell
$env:SPRING_PROFILES_ACTIVE="prod"; ./gradlew bootRun
```

#### **Windows (CMD)**
```cmd
set SPRING_PROFILES_ACTIVE=prod
./gradlew bootRun
```

**적용 결과:**  
운영 환경에서는 `application-prod.yml`이 적용되고, 개발 환경에서는 `application-dev.yml`이 적용된다.

---

### **2️⃣ `application.properties`에서 설정**
`src/main/resources/application.properties` 파일에서 기본 프로파일을 설정할 수도 있다.

#### **예제: `application.properties`**
```properties
spring.profiles.active=dev
```

이렇게 설정하면, **기본적으로 `application-dev.yml`이 적용됨.**  
만약 운영 환경에서 실행할 경우, `SPRING_PROFILES_ACTIVE=prod`를 설정하면 된다.

---

### **3️⃣ Gradle 빌드 시 프로파일 설정**
Gradle 실행 시 `-Dspring.profiles.active=prod` 옵션을 추가하여 원하는 프로파일을 적용할 수 있다.

```sh
./gradlew bootRun -Dspring.profiles.active=prod
```

JAR 파일을 실행할 때도 동일한 방식으로 적용 가능하다.

```sh
java -jar -Dspring.profiles.active=prod build/libs/demo-0.0.1-SNAPSHOT.jar
```

**적용 결과:**  
운영 환경에서는 `application-prod.yml`이 적용되고, 개발 환경에서는 `application-dev.yml`이 적용된다.

---

## 4. CI/CD 환경에서 프로파일 설정 (CI에 영향 없는 방법)

로컬에서 개발할 때는 `application-dev.yml`을 사용하고,  
CI/CD에서는 `application-prod.yml`을 적용하도록 설정할 수 있다.

### **GitHub Actions에서 환경 변수 설정**
```yaml
env:
  SPRING_PROFILES_ACTIVE: prod
```

### **GitLab CI/CD에서 환경 변수 설정**
```yaml
variables:
  SPRING_PROFILES_ACTIVE: prod
```

### **Jenkins에서 환경 변수 설정**
```sh
export SPRING_PROFILES_ACTIVE=prod
./gradlew build
```

이렇게 하면 **CI/CD에서는 무조건 `prod` 프로파일이 적용되므로, 로컬 개발 환경과 충돌하지 않음.**

---

## 5. `application.properties`와 `application.yml`이 동시에 존재하는 경우

Spring Boot는 `application.properties`와 `application.yml`이 함께 있어도 정상적으로 동작한다.  
하지만 같은 설정이 두 파일에 존재할 경우 **우선순위에 따라 적용됨.**

| 설정 파일 | 우선순위 |
|----------|---------|
| 환경 변수 (`SPRING_PROFILES_ACTIVE`) | 가장 우선 |
| `application-{profile}.yml` | 높은 우선순위 |
| 기본 `application.yml` | 기본값으로 사용 |
| `application.properties` | 가장 낮은 우선순위 |

즉, **`application.yml`이 `application.properties`보다 우선 적용되며**,  
`application-{profile}.yml`이 `application.yml`을 덮어쓴다.

**예제: `application.properties`**
```properties
server.port=8080
spring.profiles.active=dev
```

이 경우, `spring.profiles.active=dev`가 설정되어 `application-dev.yml`이 적용된다.  
하지만 `SPRING_PROFILES_ACTIVE=prod`로 설정하면 `application-prod.yml`이 적용된다.

---

## 6. 운영 환경에서는 `application-prod.yml`이 필수인가?

운영 환경에서 `prod` 프로파일을 사용하려면 **`application-prod.yml` 파일명을 사용하는 것이 권장된다.**  
Spring Boot는 `application-{profile}.yml` 형식으로 자동으로 인식하므로,  
CI/CD에서 `SPRING_PROFILES_ACTIVE=prod`를 설정하면 `application-prod.yml`이 자동으로 적용된다.

✅ **운영 환경에서 `prod` 프로파일을 강제 적용하려면?**  
```sh
SPRING_PROFILES_ACTIVE=prod ./gradlew build
```

✅ **JAR 실행 시 `prod` 프로파일 적용하려면?**  
```sh
java -jar -Dspring.profiles.active=prod build/libs/demo-0.0.1-SNAPSHOT.jar
```

✅ **CI/CD 환경에서 프로파일 적용하려면?**  
```yaml
env:
  SPRING_PROFILES_ACTIVE: prod
```

운영 환경에서는 **항상 `application-prod.yml`을 유지하는 것이 가장 안정적인 방식이다.**

---

## **결론**

- `application.yml`을 기본 설정으로 사용하고, `application-{profile}.yml`을 환경별로 관리할 수 있다.
- `spring.profiles.active`를 설정하여 원하는 프로파일을 활성화할 수 있다.
- CI/CD에서는 환경 변수를 통해 `prod` 프로파일을 강제 적용하는 것이 일반적이다.
- 운영 환경에서는 `application-prod.yml`을 유지하는 것이 가장 안정적인 방식이다.
