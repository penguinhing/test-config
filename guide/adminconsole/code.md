# 코드 컨벤션

## 문서 목적

이 문서는 Java 코드의 컨벤션 준수 여부를 비교하고 판정하기 위한 규칙 정의서이다.
각 규칙은 `규칙 ID`, `적용 범위`, `판정 기준`을 기준으로 해석하며, 필요한 경우 `위반 예시`, `준수 예시`를 보조 기준으로 사용한다.

## 공통 판정 원칙

- `위반 예시`가 있는 경우 반드시 컨벤션 위반으로 판정한다.
- `준수 예시`가 있는 경우 컨벤션을 만족하는 기준 예시로 판정한다.
- 예시와 동일하지 않은 코드라도 `판정 기준`의 의미를 만족하면 준수로 판정한다.
- 예시와 동일하지 않은 코드라도 `판정 기준`을 어기면 위반으로 판정한다.

## CONTROLLER-001: 컨트롤러는 Entity를 반환하지 않는다

### 적용 범위

- Spring MVC `@Controller`, `@RestController` 클래스의 요청 처리 메서드

### 판정 기준

- 컨트롤러는 Entity 또는 Model 객체를 직접 반환하지 않는다.
- 컨트롤러 응답은 반드시 DTO로 변환해 반환한다.
- Entity 또는 Model 컬렉션도 직접 반환하지 않는다.
- 응답 래핑은 `CONTROLLER-002` 기준을 함께 따른다.

## MODEL-001: model 하위 클래스와 VO에는 Setter 사용을 지양한다

### 적용 범위

- 패키지 경로가 `model` 하위인 Java 클래스
- VO(Value Object) 클래스

### 판정 기준

- MyBatis를 사용하므로 JPA의 `@Entity` 어노테이션 존재 여부로 Entity를 판단하지 않는다.
- Java 파일의 패키지 경로 또는 파일 경로가 `model` 하위이면 Entity 또는 Model로 판단한다.
- model 하위 클래스와 VO 클래스에는 공개 Setter를 선언하지 않는다.
- Lombok의 `@Setter`, `@Data`를 model 하위 클래스와 VO 클래스에 사용하지 않는다.
- 상태 변경이 필요한 경우 의미 있는 메서드명으로 변경 행위를 표현한다.
- 객체 생성 시 값 주입은 생성자, 정적 팩터리 메서드, 빌더를 사용한다.

### 위반 예시

```java
@Getter
@Setter
public class User {
    private String name;
}
```

### 준수 예시

```java
@Getter
public class User {
    private String name;

    public void changeName(String name) {
        this.name = name;
    }
}
```

## LOMBOK-001: Getter와 Setter는 Lombok 어노테이션으로 생성한다

### 적용 범위

- Java 클래스의 Getter, Setter 선언

### 판정 기준

- Getter 또는 Setter가 필요한 경우 직접 메서드를 작성하지 않고 Lombok의 `@Getter`, `@Setter`를 사용한다.
- 클래스 전체에 Setter를 열면 안 되는 경우 필요한 필드 또는 메서드 수준에만 제한적으로 `@Setter`를 적용한다.
- model 하위 클래스와 VO에는 `MODEL-001`을 우선 적용한다.

## CONTROLLER-002: 컨트롤러 응답은 RestResponse 또는 RestListResponse로 감싼다

### 적용 범위

- Spring MVC `@Controller`, `@RestController` 클래스의 요청 처리 메서드

### 판정 기준

- 컨트롤러에서 값을 반환할 때는 반드시 `RestResponse<T>` 또는 `RestListResponse<T>`로 감싸서 반환한다.
- 단건 응답은 `RestResponse<T>`를 사용한다.
- 목록 응답은 `RestListResponse<T>`를 사용한다.
- DTO를 반환하더라도 응답 래퍼 없이 직접 반환하면 위반으로 판정한다.

## RESPONSE-001: 컨트롤러 외부에서는 RestResponse 또는 RestListResponse를 반환하지 않는다

### 적용 범위

- 컨트롤러 요청 처리 메서드를 제외한 Java 메서드
- Service, Dao, 내부 Helper, 내부 Component 메서드

### 판정 기준

- 컨트롤러 외부의 메서드는 `RestResponse<T>` 또는 `RestListResponse<T>`를 반환하지 않는다.
- 내부 계층에서는 DTO, Model, VO, 기본 타입, 컬렉션 등 실제 처리 대상 타입을 반환한다.
- 응답 래핑은 컨트롤러 계층에서만 수행한다.
- Feign Client 등 외부 리소스를 요청하고 외부 API 응답 계약상 `RestResponse<T>` 또는 `RestListResponse<T>`를 받아야 하는 경우는 예외로 허용한다.

## DAO-001: Dao 메서드는 하나의 쿼리만 실행한다

### 적용 범위

- Dao 클래스 또는 Mapper 클래스의 메서드
- MyBatis Mapper 인터페이스 메서드

### 판정 기준

- 하나의 Dao 메서드에서는 하나의 쿼리만 실행한다.
- 여러 쿼리를 조합하는 흐름 제어는 Service 계층에서 처리한다.
- 하나의 비즈니스 동작에 여러 쿼리가 필요하더라도 Dao 메서드 하나에 묶지 않는다.
