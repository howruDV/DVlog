---
title:  "Unreal ㅣ 2. 언리얼 코딩 규칙"
excerpt: "코딩 표준과 언리얼 코딩 표준"

categories:
  - Unreal
tags:
  - Unreal, CodingConvenction
last_modified_at: 2025-06-23

toc: true
toc_label: ""
toc_sticky: true
---



# 📌1. 코딩 표준
## [코딩 표준]
**코딩 표준이란**
- 프로그래밍을 작성하는데 지켜야 하는 프로그래밍 이름 규칙, 작성 방법 등을 지정한 가이드라인
- 예시
	- [🔗Google Docs : C++ Style Guide](https://google.github.io/styleguide/cppguide.html)
	- [🔗Unreal Docs : C++ Coding Standard](https://dev.epicgames.com/documentation/ko-kr/unreal-engine/epic-cplusplus-coding-standard-for-unreal-engine)


**코딩 표준이 왜 필요한가**
- 유지보수 측면에서 소프트웨어의 가독성 향상
- 크로스 컴파일러 호환성에 필요
<br>
<br>

## [명명 스타일]
- Pascal Casing : 첫 글자를 대문자를 사용해 명명 `UnrealEngine`
- Camel Casing : 첫 글자는 소문자로, 나머지는 대문자로 명명 `unrealEngine`
- Snake Casing : 합성어 사이에 언더바를 사용해 명명 `unreal_engine`  

# 📌II. 언리얼 코딩 표준
## [명명 규칙]
- Pascal Casing
- 접두사
	- 클래스 상속
		- UObject 상속: U
		- AActor 상속: A 
		- SWidget 상속: S
		- Interface 개념: I
		- 템플릿 클래스: T
		- 이외 F
	- 자료형
		- 열거형 E
		- bool b
<br>

## [네이밍]
- 타입 및 변수: 명사
- 메서드: 동사
- 매크로: 접두사 UE, 모두 대문자 & 언더스코어(\_) 사용
- 파일명: 접두사 붙이지 않음
<br>

## [주석 규칙]
**a. 주석 스타일**  
- JavaDoc 자동 문서화 지원
	- JavaDoc 기반 시스템을 사용해
	- 코드에서 코멘트를 추출한 뒤 자동으로 문서를 만들기 때문에,
	- 특수한 코멘트 포맷 규칙을 권장
- 주석 스타일
	- Steep 스타일
	- Sweeten 스타일
<br>

>**Steep 스타일**
>- 여러 줄 스타일  
>
>```C++
/**
 * Applies a force to move the character forward.
 *
 * This method is used when the user presses the forward key. It handles velocity update
 * and animation blending.
 *
 * @param DeltaTime     Time since last tick (seconds). Must be > 0.0.
 * @param Direction     Forward direction as a unit vector. Must be normalized.
 * @param Speed         Desired movement speed in cm/s. Range: [0, 600].
 *
 * @return              True if movement succeeded, false if blocked or invalid input.
 *
 * @see StopMovement()
 * @warning             Will not apply force if character is airborne.
 */
bool MoveForward(float DeltaTime, const FVector& Direction, float Speed);
>```

>**Sweeten 스타일**
>- 간결한 통합 설명 스타일
>```C++
// Moves the character forward given a normalized direction and speed in cm/s.
// Returns false if blocked or input is invalid.
bool MoveForward(float DeltaTime, const FVector& Direction, float Speed);
>```  
  
  

**b. 주석 포함 내용**
- 주석 목적
	- 의도(intent)를 설명하는 것이 중요.
	- 코드가 구현을 설명한다면, 주석은 목적을 설명해야 함.

- 클래스 주석
	- 클래스 주석 포함 내용
		- 이 클래스가 무엇을 해결하려고 하는지
		- 이 클래스를 왜 만들었는지

- 메서드 주석
	- 작성 위치
		- 단 한 번, 메서드가 public하게 선언된 곳
		- 호출자(callers)에게 관련된 정보
		- 함수 구현 세부 사항이나 호출자와 무관한 오버라이드는 함수 본문 내부 주석으로 분리
	- 여러 줄 메서드 주석 포함 내용
		- 함수 목적
			- 이 함수가 해결하는 문제
		- 파라미터 주석
			- 각 파라미터에 대해 다음을 문서화해야 함:
			- **단위** (예: `초`, `픽셀`, `퍼센트` 등)
		    - **기대 값 범위** (예: `0 ~ 1`)
		    - **불가능한 값**
		    - **상태 코드/에러 코드의 의미**
		- 반환 값 주석
			- 반환 값을 별도 `@return`으로 문서화
		    - 단, 함수 목적이 **단순히 이 값을 반환하는 것**이라면 중복 피하기 위해 `@return`은 생략 가능
		- 추가정보
			- `@see`, `@warning`, `@note`, `@deprecated`