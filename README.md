# 📝 Task02 - Board CRUD Project

Spring Boot와 Spring Data JPA를 활용한 **회원-게시판 기능 구현 프로젝트**<br>
✅ RESTful API 설계 → 계층 분리 → 연관관계 매핑 → JPA 활용 구현

---

## 🔧 사용 기술 스택

- Java 17  
- Spring Boot 3.x  
- Spring Web  
- Spring Data JPA  
- Lombok  
- MySQL 8.x  
- IntelliJ IDEA  
- Postman (API 테스트)

---

## 🧩 아키텍처 구성

- `Controller` : HTTP 요청 수신 및 응답 처리  
- `Service` : 비즈니스 로직 처리  
- `Repository` : JPA 기반 DB 접근  
- `Entity/DTO` : 도메인 모델과 요청/응답 모델 분리

> 📌 모든 계층은 Spring Bean으로 등록되며, 생성자 주입 방식 사용  
> 📌 `@Transactional`을 통한 변경 감지(Dirty Checking) 활용

---

## 📌 주요 기능

### ✅ 회원 기능

| 기능 | Method | URL | 설명 |
|------|--------|-----|------|
| 회원 가입 | POST | `/members/signup` | 회원 정보 등록 |
| 회원 조회 | GET | `/members/{id}` | 특정 회원 정보 조회 |
| 비밀번호 수정 | PATCH | `/members/{id}` | 기존 비밀번호 검증 후 수정 |

### ✅ 게시글 기능

| 기능 | Method | URL | 설명 |
|------|--------|-----|------|
| 게시글 작성 | POST | `/boards` | 게시글 등록 (작성자 포함) |
| 게시글 전체 조회 | GET | `/boards` | 전체 게시글 조회 |
| 게시글 단건 조회 | GET | `/boards/{id}` | 게시글 + 작성자 나이 포함 |
| 게시글 삭제 | DELETE | `/boards/{id}` | 게시글 삭제 |

- 모든 요청/응답은 **JSON** 형식으로 처리
- 상태 코드: `201 Created`, `200 OK`, `404 Not Found`, `401 Unauthorized` 등

---

## 🔗 Entity 관계

- `Member (1) : Board (N)` → **단방향 연관 관계**  
- `Board` 엔티티에서 `@ManyToOne` 방식으로 `Member` 참조

---

## 📂 디렉터리 구조 예시

```bash
com.example.board
├── controller
│   ├── MemberController
│   └── BoardController
├── dto
│   ├── BoardResponseDto
│   └── BoardWithAgeResponseDto
│   └── CreateBoardRequestDto
│   └── MemberResponseDto
│   └── SignUpRequestDto
│   └── SignUpResponseDto
│   └── UpdatePasswordRequestDto
├── entity
│   ├── Member
│   └── Board
│   └── BaseEntity
├── service
│   ├── MemberService
│   └── BoardService
├── repository
│   ├── MemberRepository
│   └── BoardRepository
├── BoardApplication

```

---

## 🧪 테스트 및 결과

- Postman을 활용한 API 테스트 
- 다음과 같은 JSON 예시로 요청 가능:

**✅ 회원 관련 테스트**
  
**🔸 회원 가입 요청**<br>
**POST** `/members/signup`
```json
{
  "username": "test",
  "password": "1234",
  "age": 20
}
```
**응답 (201 Created)**
```json
{
  "id": 1,
  "username": "test",
  "age": 20
}
```
<br>

**🔸 회원 조회 요청**<br>
**GET** `/members/1`<br>
**응답 (200 OK)**
```json
{
  "username": "test",
  "age": 20
}
```
<br>

**✅ 게시글 관련 테스트**

**🔸 게시글 작성 요청**<br>
**POST** `/boards`
```json
{
  "title": "JPA 게시판 구현",
  "contents": "Spring Data JPA로 게시판을 만들어 봅니다.",
  "username": "test"
}
```
**응답 (201 Created)**
```json
{
  "id": 1,
  "title": "JPA 게시판 구현",
  "contents": "Spring Data JPA로 게시판을 만들어 봅니다."
}

```

---

## ✔️ 학습 및 구현 포인트

- `Spring Data JPA`의 `JpaRepository` 활용
- `@Entity`, `@Table`, `@ManyToOne` 등 JPA 어노테이션 활용
- `@Transactional`을 통한 변경 감지(비밀번호 수정 등)
- DTO를 통한 **요청/응답 분리 및 캡슐화**
- `default` 메서드를 이용한 예외 처리 공통화 (`findByIdOrElseThrow()` 등)

---

## ⚠️ 한계점 및 개선 방향

- ❌ 인증(Authentication) 기능 미구현 → JWT, Session 기반 로그인 적용 가능
- ❌ 입력 값 검증 미흡 → `@Valid`, `@Validated` 등 도입 예정
- ❌ 예외 처리 분산 → `@ControllerAdvice` 활용해 공통 처리 리팩토링 예정

