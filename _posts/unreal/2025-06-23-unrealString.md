---
title:  "Unreal ㅣ 3. 언리얼 C++ 기본 타입과 문자열"
excerpt: "기본 타입의 문제와 언리얼 문자열 타입"

categories:
  - Unreal
tags:
  - Unreal, String
last_modified_at: 2025-06-23

toc: true
toc_label: ""
toc_sticky: true
---



# 📌I. 기본 타입의 문제
## [플랫폼 파편화]
언리얼이 데이터 기본 타입을 별도로 지정하는 이유

**C++의 플랫폼 파편화 (Platform Fragmentation)**
 - 게임은 Window-32bit, Window-64bit, Mac, Console 등 다양한 플랫폼에서 가동
 - 그러나 플랫폼마다 동일한 작동을 보장하지 않음
 - 어떤 함수의 동작이 다르다거나, data type의 크기가 다르다거나 등
 - 예시) int
	 - 어떤 플랫폼에서는 32bit
	 - 어떤 플랫폼에서는 64bit
	 - 따라서 코드 작성자가 int 타입의 크기를 확신할 수 없음

**모호한 데이터 타입 크기가 야기하는 문제들**
- cash hit 등의 최적화 문제
	- 게임은 단일 컴퓨터에서 최대 퍼포먼스를 뽑아야 하므로,
	- 메모리 관리 최적화를 위해 cash size에 맞춰 데이터 크기를 면밀히 고려해야 하는데
	- 데이터 타입이 모호하면 문제가 될 수 있음
- 네트워크 상에서 전송하는 데이터 크기의 문제
	- 데이터 타입이 모호하면 전송하는 크기가 달라질 수 있음
- 예시) bool 타입
	- bool은 크기가 명확하지 않음
	- 헤더에는 가급적 bool 대신 uint8을 사용하되 bit field 연산자 사용
```C++
uint8 bMybool:1;
```

## [다양한 캐릭터 인코딩]
🔗[Blog : The Absolute Minimum Every Software Developer Absolutely, Positively Must Know About Unicode and Character Sets](http://www.joelonsoftware.com/articles/Unicode.html)
[🔗Unreal Docs : Character Encoding](https://dev.epicgames.com/documentation/ko-kr/unreal-engine/character-encoding-in-unreal-engine)
언리얼이 문자열 처리 방식을 따로 규정하는 이유

**다양한 문자열 방식**
- Single byte (ANSI, ASCII) : 컴퓨터 초창기
- Multibyte (EUC-KR, CP949) : 컴퓨터 보급기 (1990년대 초중반 Windows)
- Unicode (UTF-8, UTF-16) : 국제 표준 정착기 (1990년대 후반)

**UTF 8과 16의 차이**
- UTF-8
	- 기본적으로 1byte
	- 필요할 때 가변적으로 2byte 사용
- UTF-16
	- 기본적으로로 2byte

**언리얼 문자열 처리**
- 문자열 처리: UTF-16 사용
- 소스 코드(.cpp): UTF-8 사용
	- Windows 디폴트인 Code Page 949를 사용하면 로그 등을 찍을 때 한글 깨짐
<br>

# 📌II. 언리얼 C++ 문자열 타입
🔗 [Docs: string handling in unreal engine](https://dev.epicgames.com/documentation/ko-kr/unreal-engine/string-handling-in-unreal-engine)

>언리얼 스트링 처리 클래스
>- FString
>- FName
>- FText
  
  

## [FString]
 [🔗Docs : Unreal FString](https://dev.epicgames.com/documentation/ko-kr/unreal-engine/fstring-in-unreal-engine)
<br>

**a. 언리얼 스트링 리터럴 처리**
- 통일된 문자열 처리 방식
	- UTF-16 (사이즈 균일한 타입)
	- `TCHAR`: 유니코드를 위한 언리얼 표준 캐릭터 타입
- 문자열 리터럴 생성
	- `TEXT 매크로`: TEXT매크로로 감싼 문자열은 TCHAR 배열로 지정됨
- 문자열 다루기
	- `FString`: TCHAR 배열을 포함하는 헬퍼 클래스
<br>

**b. FString**
`FString` 은 조작이 가능한 유일한 스트링 클래스
- 다양한 메서드 존재
- 비싼 비용

**FString 구조**
![[Pasted image 20241107132150.png]]
- 문자열 데이터 관리: `TArray` (TCHAR 동적 배열)
	- `FString`은 내부적으로 `TArray<TCHAR>`를 멤버로 갖고 있음
	- `FString` 에 `TCHAR` 배열 대입 가능
- 문자열 데이터 접근: 포인터 연산자
	- `FString`의 포인터 연산자(\*)는 `TArray<TCHAR>` 배열 시작주소 반환하도록 오버라이딩 됨
	- (`UE_LOG`나 `Printf` 같은 함수는 `const TCHAR*`을 받기 때문에 `operator*`을 써야 함)
- 문자열 처리: 내부 메소드 및 `FCString`
	- `FCString`: C 런타임 수준에서 문자열 처리하는 다양한 함수 제공

**FString 문자열 처리 예시**
- 다른 타입에서 `FString`으로 변환
	- `FString::Printf`
	- `FString::SanitizeFloat`
	- `FString::FromInt`
- `FString`에서 다른 타입으로 변환 (안전하지 않음)
	- `FCString::Atoi`
	- `FCString:Atof`

>FString과 UE_LOG
>`UE_LOG(LogTemp, Log, TEXT("%s"), *MyFString)`
>- UE_LOG의 3번째 인자는 배열만 받으므로, FString 객체를 그대로 전달하면 안 됨
>- 포맷으로 전달할 때, FString이 감싸고 있는 내용에 접근해야 하므로 * 연산자를 사용해야 함

## [FName]
[🔗Docs : Unreal FName](https://dev.epicgames.com/documentation/ko-kr/unreal-engine/fname-in-unreal-engine)
언리얼 초경량 문자열 사용 시스템으로,
주로 에셋 관리를 위해 사용

- 목적: 에셋 관리
	- 에셋을 String으로 네이밍해 저장하면 탐색이 오래 걸림 (String 비교 필요)
	- 빠른 탐색을 위해 데이터 테이블 사용
	- `FName`은 문자열을 해싱해 (데이터 테이블을 검색할) 키 생성하고 보관

- 내부적인 구조(해쉬)로 가지는 특징
	- 빌드시 해시값(Int)로 변환됨
	- 대소문자 구분 않음
	- 문자열 변경이나 조작 불가능
	- FName 접근 속도 빠름

- `FName`의 구조
	- Index (Name Index): 문자열이 저장된 전역 Pool에서의 인덱스
	- Number (Instance ID): 동일한 이름을 반복적으로 사용할 수 있게 해주는 정수

- `FName`과 글로벌 Pool
	- 언리얼은 FName과 관련된 글로벌 Pool 자료구조를 가지고 탐색에 활용
		- `FName`: 문자열을 해싱해 키 생성하고 보관
		- 자료 검색: `FName`에 저장된 키를 전역 Pool에서 검색해 자료 반환
	- `FName` 생성
		- 생성자에 문자열 정보를 넣으면 풀을 조사해 적당한 키로 변환
		- Find or Add
		- `FName`을 Tick처럼 자주 사용하는 함수 안에서 자주 호출하게 되면 오버헤드가 있음 (키생성 및 조사비용)

<div class="spacer"></div>

## [FText]
🔗[Docs: Unreal FText](https://dev.epicgames.com/documentation/ko-kr/unreal-engine/ftext-in-unreal-engine)  
Localization 지원을 위한 문자열 관리 체계로,  
문자열 테이블과 함께 사용
- 일종의 키로 작용
- 별도의 문자열 테이블 정보가 추가로 요구됨
- 게임 빌드 시 자동으로 다양한 국가별 언어로 변환됨