---
title: "[Linux] 리눅스 `/usr` 디렉토리"
description: "Linux 리눅스 `/usr` 디렉토리"
author: "youneedpython"
date: "2025-03-08 21:00:22:00 +0900" 
categories: [OS, Linux]
tags: [OS, Linux, 리눅스, usr, 디렉토리, 폴더]
pin: true
math: true
mermaid: true
---

<br/><br/>

# 리눅스 `/usr` 디렉토리

<br/><br/>

리눅스에서 `/usr` 디렉토리는 **"Unix System Resources"**의 약자로, 사용자가 실행하는 프로그램 및 관련 파일을 저장하는 디렉토리  
하지만 흔히 **"User"**와 관련 있다고 오해하는 경우가 많음

## 📂 `/usr` 디렉토리의 역할

`/usr` 디렉토리는 시스템이 부팅된 후에 접근 가능한 **공유 가능하고 읽기 전용인 사용자 애플리케이션 및 라이브러리**를 저장하는 공간  
예를 들어, 시스템에 기본적으로 설치된 유틸리티, 문서, 라이브러리 등이 저장  

## 📌 `/usr`의 주요 하위 디렉토리

- **`/usr/bin`** → 일반 사용자용 실행 파일 (예: `ls`, `grep`, `vim` 등)
- **`/usr/sbin`** → 시스템 관리자용 실행 파일 (예: `fdisk`, `iptables` 등)
- **`/usr/lib`** → 실행 파일이 사용하는 공유 라이브러리
- **`/usr/local`** → 사용자가 직접 설치한 프로그램 (시스템 기본 설치와 분리됨)
- **`/usr/share`** → 문서, 아이콘, 설정 파일 같은 공유 데이터 저장
- **`/usr/include`** → C/C++ 헤더 파일 저장

## 📌 `/usr` vs `/bin`, `/sbin`

- `/bin`, `/sbin` → **부팅에 필수적인 실행 파일** 저장 (부팅 시 마운트됨)
- `/usr/bin`, `/usr/sbin` → **부팅 이후에 사용하는 실행 파일** 저장 (네트워크 마운트 가능)

즉, `/usr`는 사용자 프로그램과 라이브러리를 저장하는 공간이지만, 시스템 필수 파일은 `/bin`과 `/sbin`에 있음  
