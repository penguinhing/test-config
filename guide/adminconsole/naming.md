# 네이밍 컨벤션

## 문서 목적

이 문서는 Java 파일명, 패키지 경로, 메서드명, 변수명의 컨벤션 준수 여부를 비교하고 판정하기 위한 규칙 정의서이다.
각 규칙은 `규칙 ID`, `적용 범위`, `판정 기준`, `위반 예시`, `준수 예시`를 기준으로 해석한다.

## 공통 판정 원칙

- 파일명 접미사는 대소문자를 구분한다.
- `DTO`, `DAO`, `VO` 접미사는 모두 대문자로 작성해야 한다.
- 경로 예시는 프로젝트 소스 루트 기준의 상대 경로로 해석한다.
- `위반 예시`는 반드시 컨벤션 위반으로 판정한다.
- `준수 예시`는 컨벤션을 만족하는 기준 예시로 판정한다.

## NAMING-DTO-001: dto 패키지의 클래스 파일명은 DTO로 끝난다

### 적용 범위

- `/dto` 패키지 내부의 Java 클래스 파일
- 경로 패턴: `**/dto/*.java`

### 판정 기준

- `/dto` 패키지 내부의 Java 클래스 파일명은 반드시 `DTO.java`로 끝나야 한다.
- `Dto.java`, `DtO.java`, `dto.java`처럼 대소문자가 다른 접미사는 위반으로 판정한다.

### 위반 예시

- `dto/asdDto.java`
- `dto/asdDtO.java`
- `dto/asddto.java`

### 준수 예시

- `dto/kkDTO.java`
- `dto/asdasd3DTO.java`

## NAMING-DTO-002: DTO 클래스명은 행위와 요청/응답을 구분한다

### 적용 범위

- `/dto` 패키지 내부의 요청 DTO 클래스
- `/dto` 패키지 내부의 응답 DTO 클래스

### 판정 기준

- DTO 클래스명은 어떤 도메인의 어떤 행위에 대한 DTO인지 드러나야 한다.
- DTO 클래스명은 요청인지 응답인지 `RequestDTO` 또는 `ResponseDTO` 접미사로 구분해야 한다.
- DTO 클래스명은 `{도메인}{행위}{Request|Response}DTO` 형식을 따른다.
- 행위명은 `Create`, `Update`, `Delete`, `Get`, `Search` 등 실제 요청/응답 목적을 나타내는 단어를 사용한다.
- `UserDTO`, `UserRequestDTO`, `UserCreateDTO`처럼 행위 또는 요청/응답 구분이 누락된 이름은 위반으로 판정한다.

### 준수 예시

- `UserCreateRequestDTO`
- `UserCreateResponseDTO`
- `UserUpdateRequestDTO`
- `UserUpdateResponseDTO`
- `UserDeleteRequestDTO`
- `UserDeleteResponseDTO`
- `UserGetRequestDTO`
- `UserGetResponseDTO`
- `UserSearchRequestDTO`
- `UserSearchResponseDTO`

## NAMING-DAO-001: dao 패키지의 클래스 파일명은 DAO로 끝난다

### 적용 범위

- `/dao` 패키지 내부의 Java 클래스 파일
- 경로 패턴: `**/dao/*.java`

### 판정 기준

- `/dao` 패키지 내부의 Java 클래스 파일명은 반드시 `DAO.java`로 끝나야 한다.
- `Dao.java`, `DAo.java`, `DaO.java`, `dao.java`처럼 대소문자가 다른 접미사는 위반으로 판정한다.

### 위반 예시

- `dao/asdDao.java`
- `dao/asdDAo.java`
- `dao/asdDaO.java`
- `dao/asddao.java`

### 준수 예시

- `dao/kkDAO.java`
- `dao/asdasd3DAO.java`

## NAMING-VO-001: vo 패키지의 클래스 파일명은 VO로 끝난다

### 적용 범위

- `/vo` 패키지 내부의 Java 클래스 파일
- 경로 패턴: `**/vo/*.java`

### 판정 기준

- `/vo` 패키지 내부의 Java 클래스 파일명은 반드시 `VO.java`로 끝나야 한다.
- `Vo.java`, `vo.java`처럼 대소문자가 다른 접미사는 위반으로 판정한다.

### 위반 예시

- `vo/asdVo.java`
- `vo/asdvo.java`

### 준수 예시

- `vo/asdVO.java`

## NAMING-MODEL-VO-001: model 내부의 VO 클래스 파일명은 VO로 끝난다

### 적용 범위

- `/model` 경로 내부의 Java 클래스 파일 중 VO 클래스
- 경로 패턴: `**/model/*.java`

### 대상 판정 기준

- 파일명 또는 클래스명이 VO 개념을 나타내는 `/model` 내부 Java 클래스는 이 규칙의 적용 대상이다.
- 예: `model/asdVo.java`, `model/asdvo.java`, `model/asdVO.java`

### 제외 대상

- VO 클래스가 아닌 `/model` 내부 Java 클래스는 제외한다.
- 예: `model/ComplianceResultFw.java`
- 예: `model/ComplianceTargetLongRange.java`

### 준수 판정 기준

- 적용 대상 파일명은 반드시 `VO.java`로 끝나야 한다.
- `Vo.java`, `vo.java`처럼 대소문자가 다른 접미사는 위반으로 판정한다.

### 위반 예시

- `model/asdVo.java`
- `model/asdvo.java`

### 준수 예시

- `model/asdVO.java`

## NAMING-METHOD-001: CRUD 메서드명은 계층별 동사 규칙을 따른다

### 적용 범위

- Controller 클래스의 요청 처리 메서드
- Service 클래스의 CRUD 메서드
- DAO 클래스 또는 MyBatis Mapper 인터페이스의 CRUD 메서드

### 판정 기준

- DAO 메서드는 CRUD 행위에 대해 `select`, `insert`, `update`, `delete`를 사용한다.
- Service 메서드는 CRUD 행위에 대해 `get`, `add`, `edit`, `remove`를 사용한다.
- Controller 메서드는 `handle` 접두사를 붙이고, CRUD 행위에 대해 `Get`, `Add`, `Edit`, `Remove`를 사용한다.
- 같은 CRUD 행위는 Controller, Service, DAO 계층에서 대응되는 동사로 통일한다.

### 준수 예시

- Controller: `handleGetUserList`
- Service: `getUserList`
- DAO: `selectUserList`

## NAMING-COLLECTION-001: 복수형 대신 List를 사용한다

### 적용 범위

- 여러 값을 나타내는 변수명
- 여러 값을 반환하거나 처리하는 메서드명

### 판정 기준

- 변수명이나 메서드명에서 영어 복수형을 사용하지 않고 `List`를 사용한다.
- 목록을 나타내는 이름은 단수 명사와 `List`를 조합한다.

### 위반 예시

- `devices`
- `getUsers`

### 준수 예시

- `deviceList`
- `getUserList`
