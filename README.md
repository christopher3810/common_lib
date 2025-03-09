# Common Library

Common Lib 은 여러 모듈에서 공통으로 사용되는 유틸리티, 예외 처리, 오류 메시지 등을 제공하는 순수 Kotlin 라이브러리입니다.

## 주의사항

1. Common Lib 은 순수 Kotlin 라이브러리로, Spring과 같은 프레임워크에 의존하지 않습니다.
2. 이 모듈에 추가되는 모든 코드는 재사용성과 경량성을 고려하여 작성되어야 합니다.

## 주요 기능

1. **오류 메시지 관리**: 표준화된 오류 메시지 정의 및 관리
2. **예외 처리 유틸리티**: 일관된 예외 응답 형식 제공
3. **유틸리티 클래스**: 다양한 유틸리티 기능 (ID 생성, 패턴 검증, 개인정보 마스킹 등)

## 사용 방법

### 1. 설치 방법

### Gradle

```groovy
repositories {
    mavenCentral()
    maven { url 'https://jitpack.io' }
}

dependencies {
    implementation 'com.github.christopher3810:common_lib:Tag'
}
```

### Maven

```xml
<repositories>
    <repository>
        <id>jitpack.io</id>
        <url>https://jitpack.io</url>
    </repository>
</repositories>

<dependency>
    <groupId>com.github.christopher3810</groupId>
    <artifactId>common_lib</artifactId>
    <version>v0.1.0</version>
</dependency>
```

### 요구사항

- Java 21 이상
- Kotlin 2.0.0 이상

### 버저닝 규칙 (Semantic Versioning)

이 라이브러리는 [Semantic Versioning 2.0.0](https://semver.org/) 표준을 준수합니다.

버전 번호는 `MAJOR.MINOR.PATCH` 형식을 따릅니다:

1. **MAJOR** 버전: 이전 버전과 호환되지 않는 API 변경이 있을 때 증가
2. **MINOR** 버전: 이전 버전과 호환되는 방식으로 기능이 추가될 때 증가
3. **PATCH** 버전: 이전 버전과 호환되는 버그 수정이 있을 때 증가

### 릴리스 태그 규칙

GitHub 릴리스 태그는 항상 `v` 접두사를 붙여 사용합니다:

- `v0.1.0`
- `v1.0.0`
- `v1.1.0`

### 2. 오류 메시지 사용

표준화된 오류 메시지를 사용하여 일관된 오류 처리를 구현할 수 있습니다:

```kotlin
import com.tars.common.error.ErrorMessage

// 오류 메시지 사용
val errorMessage = ErrorMessage.USER_NOT_FOUND.format("user@example.com")
```

### 3. 오류 응답 생성

API 오류 응답을 생성할 때 표준 형식을 사용할 수 있습니다:

```kotlin
import com.tars.common.exception.ErrorResponse
import com.tars.common.error.ErrorMessage
import java.time.LocalDateTime

// 오류 응답 생성
val errorResponse = ErrorResponse(
    timestamp = LocalDateTime.now(),
    status = 404,
    error = "Not Found",
    code = ErrorMessage.USER_NOT_FOUND.code,
    message = ErrorMessage.USER_NOT_FOUND.format("user@example.com"),
    path = "/api/users/email/user@example.com"
)
```

### 4. 사용자 ID 생성

고유한 사용자 ID를 생성할 때 UserIdGenerator를 사용할 수 있습니다:

sync 인 타입과 cas를 일일히 하는 타입 두가지

1. UserIdGeneratorCas
2. UserIdGeneratorSync

상황에 맞게 사용하면 됩니다.

```kotlin
import com.tars.common.util.idGenerator.UserIdGeneratorSync
import com.tars.common.util.idGenerator.UserIdGeneratorCas

// 동기화 방식 ID 생성기 사용
val syncGenerator = UserIdGeneratorSync()
val userId = syncGenerator.generateUserId()

// CAS(Compare-And-Swap) 방식 ID 생성기 사용
val casGenerator = UserIdGeneratorCas()
val userId = casGenerator.generateUserId()
```

### 5. 패턴 검증 (Pattern Validation)

이메일, 전화번호, 주민등록번호, 비밀번호, 카드번호 등의 형식을 검증할 때 PatternValidator를 사용할 수 있습니다:

```kotlin
import com.tars.common.util.patternValidator.PatternValidator
import com.tars.common.util.patternValidator.PatternValidator.PatternType

// 이메일 검증
val isValidEmail = PatternValidator.isValidEmail("user@example.com")

// 전화번호 검증
val isValidPhone = PatternValidator.isValidPhoneNumber("010-1234-5678")

// 주민등록번호 검증
val isValidSSN = PatternValidator.isValidSSN("123456-1234567")

// 비밀번호 검증
val isValidPassword = PatternValidator.isValidPassword("password123")

// 카드번호 검증
val isValidCard = PatternValidator.isValidCardNumber("1234-5678-9012-3456")

// 범용 검증 메서드 사용
val isValid = PatternValidator.isValid("user@example.com", PatternType.EMAIL)
```

### 6. 개인정보 마스킹 (Personal Information Masking)

개인정보를 마스킹할 때 MaskingUtil을 사용할 수 있습니다:

```kotlin
import com.tars.common.util.masking.MaskingUtil
import com.tars.common.util.masking.MaskingUtil.MaskingStrategy

// 주민등록번호 마스킹
val maskedSSN = MaskingUtil.maskSSN("123456-1234567", true) // "123456-1******"

// 전화번호 마스킹
val maskedPhone = MaskingUtil.maskPhone("010-1234-5678", true) // "010-1234-****"

// 이름 마스킹
val maskedName = MaskingUtil.maskName("홍길동", MaskingStrategy.SHOW_FIRST_LAST) // "홍*동"

// 이메일 마스킹
val maskedEmail = MaskingUtil.maskEmail("example@domain.com") // "exa****@domain.com"

// 카드번호 마스킹
val maskedCard = MaskingUtil.maskCardNumber("1234-5678-9012-3456", true) // "1234-****-****-3456"
```

