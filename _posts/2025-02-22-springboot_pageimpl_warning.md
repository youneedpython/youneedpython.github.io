---
title: "Spring Boot `PageImpl` 직렬화 관련 경고 발생 원인 및 해결"
description: "Spring Boot `PageImpl` 직렬화 관련 경고 발생 원인 및 해결"
author: "youneedpython"
date: "2025-02-22 19:02:07:00 +0900" 
categories: [개발, SpringBoot, 에러]
tags: [SpringBoot, 스프링 부트, 직렬화, json, PageImpl]
pin: true
math: true
mermaid: true
---

<br/>

# [Spring Boot] `PageImpl` 직렬화 관련 경고 발생 원인 및 해결  

## 🚨 경고 메시지

Spring Boot에서 API를 요청했을 때 다음과 같은 경고가 발생할 수 있다:

```
2025-02-23T05:03:59.070+09:00  WARN 24684 --- [demo] [nio-8080-exec-5] ration$PageModule$WarningLoggingModifier : Serializing PageImpl instances as-is is not supported, meaning that there is no guarantee about the stability of the resulting JSON structure!
        For a stable JSON structure, please use Spring Data's PagedModel (globally via @EnableSpringDataWebSupport(pageSerializationMode = VIA_DTO))
        or Spring HATEOAS and Spring Data's PagedResourcesAssembler as documented in https://docs.spring.io/spring-data/commons/reference/repositories/core-extensions.html#core.web.pageables.
```

## ⚠️ 왜 경고가 발생하는가?

Spring Data JPA에서 **페이징된 데이터를 JSON으로 직렬화할 때**, `PageImpl<T>` 객체를 그대로 변환하면 안정적인 JSON 구조를 보장할 수 없다.

### **📌 주요 원인**
1. **Spring Boot 버전에 따라 `PageImpl<T>`의 직렬화 방식이 변경될 수 있음**
   - Spring Boot 3.x에서는 `PageImpl<T>` 직렬화 방식이 더 엄격해졌고, 이전 버전과 다르게 동작할 가능성이 커짐.

2. **JSON 변환 결과가 불필요하게 복잡해질 수 있음**
   - `PageImpl<T>`을 JSON으로 변환할 때 내부 구조(`pageable`, `sort` 등)가 포함됨.
   - API 응답의 JSON 구조가 불필요하게 복잡해지고, 클라이언트에서 처리해야 할 정보가 많아짐.

3. **Spring 공식 문서에서도 `PageImpl<T>`의 직접 직렬화를 권장하지 않음**
   - 대신 `PagedModel`(Spring HATEOAS) 또는 DTO 변환을 사용할 것을 권장.

## 📌 `PageImpl<T>`를 JSON으로 변환할 때 발생하는 문제

예를 들어, `Page<ApartmentTrade>`를 그대로 반환하면 다음과 같은 복잡한 JSON 구조가 생성될 수 있다:

```json
{
    "content": [
        { "id": 1, "aptName": "아파트1", "dealAmount": 50000 },
        { "id": 2, "aptName": "아파트2", "dealAmount": 60000 }
    ],
    "pageable": {
        "sort": {
            "sorted": false,
            "unsorted": true,
            "empty": true
        },
        "offset": 0,
        "pageNumber": 0,
        "pageSize": 10,
        "unpaged": false,
        "paged": true
    },
    "totalPages": 10,
    "totalElements": 100,
    "last": false,
    "size": 10,
    "number": 0,
    "sort": {
        "sorted": false,
        "unsorted": true,
        "empty": true
    },
    "numberOfElements": 10,
    "first": true,
    "empty": false
}
```

### **🚨 문제점**
1. **불필요한 `pageable`, `sort` 같은 내부 필드가 포함됨**
2. **`sort` 정보가 중복됨**
3. **API 응답 JSON 구조가 복잡해짐 → 클라이언트에서 처리하기 어려움**

---

# ✅ 해결 방법

## **✔ 방법 1: `PageDTO`로 변환하여 반환 (Spring HATEOAS 없이 해결)**

### **📌 `PageDTO` 생성**
```java
public class PageDTO<T> {
    private List<T> content;
    private int totalPages;
    private long totalElements;
    private int size;
    private int number;

    public PageDTO(Page<T> page) {
        this.content = page.getContent();
        this.totalPages = page.getTotalPages();
        this.totalElements = page.getTotalElements();
        this.size = page.getSize();
        this.number = page.getNumber();
    }
}
```

### **📌 컨트롤러에서 DTO 변환 후 반환**
```java
@GetMapping("/apartments")
public ResponseEntity<PageDTO<ApartmentTrade>> getApartments(Pageable pageable) {
    Page<ApartmentTrade> page = apartmentTradeRepository.findAll(pageable);
    return ResponseEntity.ok(new PageDTO<>(page));
}
```

✅ **이제 불필요한 `pageable`, `sort` 필드가 제거되고, JSON 응답이 간결해짐!**

---

## **✔ 방법 2: `PagedModel` 사용 (Spring HATEOAS 활용)**

### **📌 Gradle에서 HATEOAS 의존성 추가**
```gradle
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-hateoas'
}
```

### **📌 컨트롤러에서 `PagedResourcesAssembler` 사용**
```java
import org.springframework.hateoas.PagedModel;
import org.springframework.hateoas.server.mvc.WebMvcLinkBuilder;
import org.springframework.hateoas.server.reactive.PagedResourcesAssembler;

@RestController
@RequestMapping("/api/apartments")
public class ApartmentTradeController {
    private final ApartmentTradeService apartmentTradeService;
    private final PagedResourcesAssembler<ApartmentTrade> pagedResourcesAssembler;

    @GetMapping
    public ResponseEntity<PagedModel<?>> getApartments(Pageable pageable) {
        Page<ApartmentTrade> page = apartmentTradeService.getApartments(pageable);
        PagedModel<?> pagedModel = pagedResourcesAssembler.toModel(page);
        return ResponseEntity.ok(pagedModel);
    }
}
```

✅ **이제 `PageImpl<T>` 직렬화 경고 없이 안정적인 JSON 응답을 받을 수 있음!**

---

# **🎯 결론**
| 방법 | 장점 | 단점 |
|------|------|------|
| **`PageDTO` 변환 (방법 1)** | 추가 라이브러리 없이 간단하게 해결 | DTO를 직접 만들어야 함 |
| **`PagedModel` 사용 (방법 2)** | Spring 공식 권장 방식, 확장성이 뛰어남 | HATEOAS 추가 필요 |

✅ **가장 간단한 해결 방법:** `PageDTO<T>`를 만들어 변환하여 반환 (방법 1)  
✅ **Spring 공식 권장 방법:** `PagedModel<?>`을 사용하여 반환 (방법 2)  


