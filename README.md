# 🌱 Spring MVC 웹 페이지 만들기 학습 프로젝트
> 인프런 김영한님의 **스프링 MVC 1편 - 백엔드 웹 개발 핵심 기술** 중 "스프링 MVC - 웹 페이지 만들기" 챕터 학습 저장소

## 📚 프로젝트 소개

이 프로젝트는 Spring MVC와 Thymeleaf를 활용하여 **상품 관리 웹 페이지**를 직접 개발하는 실습 내용을 담고 있습니다.  
요구사항 분석부터 상품 목록 조회, 등록, 수정까지 기본적인 CRUD 기능을 갖춘 웹 애플리케이션을 단계적으로 구현합니다.

## 🎯 학습 목표

- **요구사항 분석**: 실무처럼 기능 요구사항을 분석하고 도메인을 설계하는 방법 이해
- **상품 도메인 개발**: Item 도메인 모델과 ItemRepository 구현
- **Thymeleaf 템플릿 연동**: 상품 목록, 상세, 등록/수정 폼을 타임리프로 개발
- **@ModelAttribute 활용**: 상품 등록 시 폼 데이터를 객체에 바인딩하는 방법 학습
- **PRG 패턴 적용**: Post/Redirect/Get 패턴으로 새로고침 중복 등록 문제 해결
- **RedirectAttributes 활용**: 리다이렉트 시 데이터를 안전하게 전달하는 방법 이해

## 🛠 기술 스택

- **Java**: 25
- **Spring Boot**: 4.0.3
- **Build Tool**: Gradle
- **Template Engine**: Thymeleaf
- **기타**: Lombok

## 🗂 프로젝트 구조

```
item-service
├── domain
│   └── item
│       ├── Item.java            # 상품 도메인 모델
│       └── ItemRepository.java  # 상품 저장소 (메모리 기반)
└── web
    └── item
        └── basic
            └── BasicItemController.java  # 상품 관리 컨트롤러
```

## 📝 학습 노트

### 구현 기능 목록

| 기능 | URL | HTTP 메서드 | 설명 |
|------|-----|------------|------|
| 상품 목록 | `/basic/items` | GET | 전체 상품 조회 |
| 상품 상세 | `/basic/items/{itemId}` | GET | 상품 단건 조회 |
| 상품 등록 폼 | `/basic/items/add` | GET | 등록 폼 표시 |
| 상품 등록 처리 | `/basic/items/add` | POST | 상품 저장 |
| 상품 수정 폼 | `/basic/items/{itemId}/edit` | GET | 수정 폼 표시 |
| 상품 수정 처리 | `/basic/items/{itemId}/edit` | POST | 상품 수정 저장 |

### PRG (Post/Redirect/Get) 패턴

새로고침 시 발생하는 **중복 등록 문제**를 해결하기 위한 패턴입니다.

```
[문제 상황]
POST /add → 상품 저장 → 뷰 렌더링 → 새로고침 → POST 재요청 → 중복 저장 ❌

[PRG 패턴 적용]
POST /add → 상품 저장 → Redirect /items/{id} → GET 상품 상세 → 새로고침 → GET 재요청 ✅
```

### RedirectAttributes

리다이렉트 시 데이터 전달 + 저장 성공 메시지 표시에 활용합니다.

```java
// 컨트롤러
redirectAttributes.addAttribute("itemId", savedItem.getId());
redirectAttributes.addAttribute("status", true);
return "redirect:/basic/items/{itemId}";

// 타임리프
<h2 th:if="${param.status}" th:text="'저장 완료!'"></h2>
```

> `addAttribute`로 추가한 값은 URL 쿼리 파라미터로 자동 전달되어 `${param.status}`로 읽을 수 있습니다.

### 타임리프 핵심 문법 요약

| 문법 | 설명 |
|------|------|
| `th:href="@{/basic/items/{id}(id=${item.id})}"` | URL 경로 변수 + 파라미터 처리 |
| `th:each="item : ${items}"` | 반복 처리 |
| `th:text="${item.itemName}"` | 텍스트 출력 |
| `th:value="${item.id}"` | 속성 값 설정 |
| `th:if="${param.status}"` | 조건부 렌더링 |
| `th:action="@{/basic/items/add}"` | 폼 액션 URL |

## 🔗 참고 자료

- [강의 링크](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-1)
- [Thymeleaf 공식 문서](https://www.thymeleaf.org/documentation.html)
- [Spring MVC 공식 문서](https://docs.spring.io/spring-framework/docs/current/reference/html/web.html#mvc)

---

**강의**: 인프런 - 스프링 MVC 1편 (김영한) | **섹션**: 8. 스프링 MVC - 웹 페이지 만들기
