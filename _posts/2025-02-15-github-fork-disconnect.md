---
title: "간단하게 GitHub 저장소 Fork 연결 해제"
description: "간단하게 포크로 만든 저장소를 원본 저장소와 연결 끊자!"
author: "youneedpython"
date: "2025-02-15 07:21:00 +0900" 
categories: [Tech, Git, leave fork]
tags: [Git, GitHub, leave fork, 포크 연결 끊기, fork repository 연결 끊기]
pin: true
math: true
mermaid: true
# image:
#   path: /assets/img/2025-02-14/commitlint.png
#   width: auto ## 원래 크기 유지
#   height: auto ## 비율 유지
#   alt: commitlint
---

<br><br>

GitHub에서 저장소를 포크(Fork)하면 원본 저장소(upstream)와 연결된 상태가 됩니다.  
그러나 특정한 이유로 이 관계를 끊고 **`독립적인 저장소`**로 만들고 싶다면,  
GitHub의 **"Leave fork network"** 기능을 사용하면 됩니다.  


> `Fork 연결 해제 순서`
>
> | 순서 | 설명 |
> |----|---------------------------------------------|
> | 1  | GitHub 저장소에서 **Settings > General** 이동 |
> | 2  | **Leave Fork Network** 버튼 클릭 |
> | 3  | Fork 해제 시 발생하는 영향 확인 |
> | 4  | 저장소 이름 입력 후 **Leave Fork Network** 선택 |
> | 5  | Fork 해제 완료, 독립 저장소로 사용 가능 |
{: .prompt-info .custom-table }


<br>

아래에서 **Fork 연결을 끊는 방법**을 단계별로 알아보겠습니다.  

---

## 1. Fork된 저장소의 Settings로 이동
1. GitHub에서 **Fork한 저장소** 클릭  
2. **Settings** 탭을 클릭  
   - Issues 탭이 보이지 않는다면, Settings에서 활성화 가능!  

<br>

## 2. Leave Fork Network 버튼 찾기
1. **Settings > General** 탭으로 이동(기본으로 선택되어 있음)  
2. 아래로 스크롤하여 **Fork 네트워크 설정** 이동(페이지 맨 아래로 이동)  
3. **"Leave fork network"** 버튼을 클릭합니다.  

![Leave Fork Network 버튼](/assets/img/2025-02-15/leave fork network.png)  

<br>

## 3. Fork 연결을 끊기 전에 확인 사항
Leave fork network 창이 뜨면, 하단에 있는 **`I have read and understand these effects`** 버튼 클릭  

![Leave Fork Warning](/assets/img/2025-02-15/leave%20fork%20network%20check.png)  

> 확인 사항
> - Fork 연결을 끊으면 **되돌릴 수 없습니다.**
> - **Fork가 해제되면 원본 저장소(upstream)와 완전히 분리됩니다.**  
> - 이후 **원본 저장소의 변경 사항을 가져올 수 없습니다.**  
> - **한 번 나가면 다시 Fork 네트워크에 합류할 수 없습니다.**  
{: .prompt-danger }


<br>

## 4. 확인 입력 후 Fork 연결 해제
GitHub는 확인을 위해 저장소 이름을 입력하고, 아래에 있는 **`Leave fork network`** 버튼 클릭  
1. 입력창에 **저장소 이름을 복사 후 붙여넣기**  
2. **"Leave fork network"** 버튼을 클릭하면 연결 해제  

![Fork 제거 확인 입력](/assets/img/2025-02-15/leave%20fork%20network%20confirm.png)  

<br>

## 5. Fork 연결 해제 완료!
간단하게 포크 연결 해제!  
이제 저장소는 더 이상 원본 저장소(upstream)와 연결되지 않음!  
- 독립적인 **Standalone Repository**임  
- 원본 저장소의 변경 사항을 더 이상 가져올 수 없음  

✅ **Fork 연결이 정상적으로 해제되었습니다!**  

<br>

> **repository 메뉴에서 **`Issues`** 추가**
> 1. Settings 탭 클릭  
> 1. 왼쪽 사이드바에서 General 선택 (기본적으로 General 선택되어 있음)
> 1. Features 섹션으로 이동
> 1. Issues 옵션이 체크되어 있는지 확인
>   - 만약 체크되어 있지 않다면 ✔ Enable issues 체크
> 1. 페이지를 새로고침하면 Issues 탭이 표시됨  
> ![Issues 메뉴 추가](/assets/img/2025-02-15/after%20add%20Issues%20menu.png)
{: .prompt-info }

<br><br>


