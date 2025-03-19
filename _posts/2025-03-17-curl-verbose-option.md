---
title: "[cURL] curl -v 옵션"
description: "curl -v 옵션 설명명"
author: "youneedpython"
date: "2025-03-17 20:12:41:00 +0900" 
categories: [OS, cURL]
tags: [OS, cURL, -v, 옵션, 컬, 요청]
pin: true
math: true
mermaid: true
---

<br/><br/>


## `curl -v` 옵션 설명

`curl` 명령어에서 `-v` (또는 `--verbose`) 옵션은 **상세한(Verbose) 출력**을 활성화하는 역할  
이 옵션을 사용하면 `curl`이 수행하는 과정이 자세하게 출력되므로, 디버깅이나 HTTP 요청 및 응답을 분석할 때 유용  

### `-v` 옵션의 주요 기능:
- **요청(Request) 및 응답(Response) 헤더 출력**  
  HTTP 요청과 응답의 헤더 정보를 출력하여 서버와의 통신을 확인  
- **DNS 조회 및 연결 정보 표시**  
  서버의 IP 주소 조회 과정과 실제로 연결된 서버 정보를 출력  
- **TLS/SSL 핸드셰이크 과정 확인**  
  HTTPS 요청 시 SSL/TLS 인증서 검증 및 핸드셰이크 과정이 출력  

---

### 예제 1: 기본 HTTP 요청과 비교
```sh
curl https://www.example.com
```
위 명령어는 일반적으로 웹페이지의 HTML 내용만 출력  

```sh
curl -v https://www.example.com
```
위 명령어를 실행하면 요청 및 응답 헤더 정보, DNS 조회 과정, 연결 상태 등을 포함한 추가적인 정보가 출력   

---

### 예제 2: `-v` 옵션을 사용하여 HTTP 요청 디버깅
```sh
curl -v -X GET https://jsonplaceholder.typicode.com/posts/1
```
출력 예시:
```
*   Trying 104.21.47.226:443...
* Connected to jsonplaceholder.typicode.com (104.21.47.226) port 443 (#0)
> GET /posts/1 HTTP/1.1
> Host: jsonplaceholder.typicode.com
> User-Agent: curl/7.68.0
> Accept: */*
>
< HTTP/1.1 200 OK
< Content-Type: application/json; charset=utf-8
< Content-Length: 292
...
```
이처럼 `-v` 옵션을 사용하면 요청과 응답의 전체 흐름을 볼 수 있어 디버깅이 쉬워진다.  

---

### 추가 옵션
- **`--trace <파일>`** : 상세한 디버깅 로그를 지정한 파일에 저장
- **`--trace-ascii <파일>`** : ASCII 형식으로 간단한 디버깅 로그 저장
- **`-s` (또는 `--silent`)** : 진행률 표시를 숨김 (예: `-v -s` 조합 사용 가능)

#### 언제 사용하면 좋을까?
- **API 호출 디버깅**: 요청 헤더 및 응답을 확인할 때
- **서버 연결 문제 해결**: DNS 조회, SSL/TLS 문제 등을 점검할 때
- **리디렉션 확인**: 서버가 다른 URL로 리디렉트하는지 확인할 때

즉, `-v` 옵션은 `curl`을 사용할 때 서버와의 통신을 좀 더 깊이 들여다볼 수 있도록 도와주는 디버깅 도구  
