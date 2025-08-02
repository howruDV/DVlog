---
title:  "Unreal ㅣ 1. 개발 환경 설정"
excerpt: "언리얼 개발 환경 설정 및 시작하기"

categories:
  - Unreal
tags:
  - Unreal
  - Setup
  - Log
last_modified_at: 2025-07-23

toc: true
toc_label: ""
toc_sticky: true
---



# 📌I. 개발 환경 설정
## [설치]
- Unreal Engine 설치 옵션
	- 디버깅을 위한 편집기 기호
		- 에픽게임즈  
		➡️ 언리얼 엔진 버전  
		➡️ 우클릭 - 부가메뉴(설치옵션)  
		➡️ 디버깅을 위한 편집기 기호(디버깅 심볼) 체크
		- ![[Pasted image 20250725012630.png]]

- 개발 환경
	- Visual Studio
	- Visual Studio Code
	- Rider

- Visual Studio 세팅
	- C++를 사용한 게임 개발
<br>
<br>

## [설정]
**a. 언리얼 환경설정**
- 에디터 설정
	- General ➡️ SourceCode  ➡️ Editor 버전 설정
	- 모든 언리얼 프로젝트 세팅에 적용됨
<br>
<br>

**b. Visual Studio 설정**  
🔗 [Unreal Doc: Visual Studio 구성 가이드](https://dev.epicgames.com/documentation/ko-kr/unreal-engine/setting-up-visual-studio-development-environment-for-cplusplus-projects-in-unreal-engine)  
🔗 [Microsoft Doc: Visual Studio의 언리얼 엔진용 도구](https://learn.microsoft.com/ko-kr/visualstudio/gamedev/unreal/get-started/vs-tools-unreal-overview)

- 확장도구 : Visual Commander의 UE4 Smarter Macro Indenting
	- 매크로 사용할 때, 엔터 입력 후 탭 들어가는 불편함 해결
<br>
<br>

# 📌II. 언리얼 시작하기
## [언리얼 코드 컴파일 방법]
> ⚠️ 절대 Visual Studio에서 수동으로 클래스를 추가하지 말 것!  

- Visual Studio에서 컴파일
	- 에디터 종료 & 엔진 재시작 필요
	- 헤더 파일이 변경된 경우: 라이브 코딩이 잘못 동작할 우려가 있음
	- 기본값이 변경된 경우: 기본값은 엔진이 켜지는 순간 초기화되므로, 엔진이 켜져 있을 때 변경되면 반영되지 않음
- 라이브 코딩으로 컴파일 (Ctrl+Alt+F11)
	- 소스 파일만 변경된 경우
	- 생성자 구현이 변경된 경우우
<br>
<br>

**a. 라이브코딩**  
🔗 [Unreal Doc: 라이브 코딩](https://dev.epicgames.com/documentation/ko-kr/unreal-engine/using-live-coding-to-recompile-unreal-engine-applications-at-runtime)

코드 변경 후, 엔진이 실행 중인 동안 (엔진을 재시작하지 않고) C++ 코드를 리빌드하고 바이너리를 패치할 수 있는 시스템
- **핫 리로드(Hot Reload)** 시스템을 대신 사용할 수 있기는 하지만, 라이브 코딩이 훨씬 빠르고 유연하다고 소개
	- 핫 리로드: 빌드 후 DLL 재로드
	- 라이브 코딩: 실시간 핫스왑 컴파일
<br>
<br>

##  [언리얼 클래스 구조]
- 언리얼 엔진은 자체적인 리플렉션 시스템과 매크로를 통해 클래스 구조 관리
- `UCLASS`, `GENERATED_BODY()` 등의 매크로를 활용해 언리얼 런타임에서 객체를 추적하고 직렬화하거나 에디터에서 다룰 수 있도록 만듬
<br>
<br>

**a. 클래스 선언 구조**  
언리얼의 리플렉션 및 메타 시스템을 위해 매크로를 추가로 사용

```C++
UCLASS() 
class MYGAME_API AMyActor : public AActor 
{
	GENERATED_BODY()
	
public:     
	AMyActor();     
	virtual void BeginPlay() override; 
};
```
- `UCLASS()` : 이 클래스가 언리얼의 UObject 시스템에 포함될 수 있도록 지정
- `GENERATED_BODY()` : 언리얼 빌드 툴이 자동으로 생성하는 코드가 들어가는 자리 (리플렉션, 직렬화 등)
- `MYGAME_API` : 모듈 외부에서 접근 가능한 API로 노출 (DLL Export 용도)
<br>
<br>

**b. Super 키워드**
```C++
void AMyActor::BeginPlay() 
{
	Super::BeginPlay();  // 부모 클래스의 BeginPlay() 호출
	// 여기에 자식 클래스만의 로직 추가 
}
```
- `Super`는 부모 클래스의 별칭
- 가상 함수를 오버라이드할 때, 부모 클래스의 기능을 유지한 채 덧붙이려면 `Super::Function()` 호출 필요
- Super의 동작 방식
	- `Super`는 실제로 언리얼 매크로 내부에서 `typedef`로 정의
	- ```C++
using Super = AParentClass;
	```
<br>

## [로그 남기기]
[🔗DOCS : 언리얼 엔진에서의 로깅](https://dev.epicgames.com/documentation/ko-kr/unreal-engine/logging-in-unreal-engine?application_version=5.3#%EC%9E%90%EC%B2%B4%EB%A1%9C%EA%B7%B8%EC%B9%B4%ED%85%8C%EA%B3%A0%EB%A6%AC%EC%A0%95%EC%9D%98)

`UE_LOG(LogCategory, Verbosity, Format, VaArgs)`
- 인자
	- LogCategory: 로그 카테고리
	- Verbosity: 로그 상세 (경고 수준)
	- Format: 출력 내용

- 출력창
	- Window ➡️ Output Log

> **0. Category**  
> 로그 카테고리
>- 카테고리 종류
>	- 언리얼 엔진에서는 90개 이상의 카테고리를 기본적으로 제공
>	- LogTemp : 특정한 카테고리에 속하지 않고 임의로 띄움
>- Custom 로그 카테고리 만들기
	- 프로젝트명.h
		```c++
		DECLARE_LOG_CATEGORY_EXTERN(MyLogCategory, Log, All);
		```
	- 프로젝트명.cpp
		```c++
		DEFINE_LOG_CATEGORY(MyLogCategory);
		```
	- 로그 사용 : 사용할 클래스 헤더에 ``#include "프로젝트명.h"``  헤더 포함 후 사용

>**1. Verbosity**  
>로그 상세 (경고수준)
>- **Fatal** 
>	- 콘솔 및 로그 파일 출력 O
>	- 가장 높은 중요도를 가지는 로그 수준
>	- 치명적인 오류 또는 상황을 나타내며, 프로그램이 종료될 수 있음
>
>- **Error** 
>	- 콘솔 및 로그 파일 출력 O (빨간색)
>	- 중요한 오류를 나타내는 로그 수준
>	- 프로그램의 실행에 큰 문제가 발생했음을 나타내며, 오류 해결에 필요한 정보 제공
>
>- **Warning**
>	- 콘솔 및 로그 파일 출력 O (노란색)
>	- 경고나 주의사항을 나타내는 로그 수준
>	- 프로그램이 정상적으로 실행되긴 하지만, 잠재적인 문제가 발생할 수 있음을 알림
>
>- **Display**
>	- 콘솔 및 로그 파일 출력 O
>	- 사용자에게 표시되는 정보를 나타내는 로그 수준
>	- 주로 게임 상에서 유용한 정보를 출력할 때 사용
>
>- **Log**
>	- 로그 파일 출력 O / 게임 내의 콘솔 출력 X
>	- 기본적인 로그 수준으로, 중요한 정보를 나타냄
>	- 프로그램의 상태, 이벤트, 데이터 등을 추적하는 데 사용
>
>- **Verbose**
>	- 로그 파일 출력 O, 게임내의 콘솔 출력 X
>	- 더 상세한 정보를 제공하는 로그 수준
>	- 디버깅 시에 자세한 내용을 확인하기 위해 사용
>
>- **VeryVerbose**
>	- 로그 파일 출력 O, 게임내의 콘솔 출력 X
>	- 가장 상세한 정보를 제공하는 로그 수준
>	- 매우 자세한 디버깅 정보를 출력할 때 사용

>**2. Formatting**  
>출력 내용
>
>- TEXT 매크로를 사용한 포맷팅 가능
>	- **FString**
>	```c++
UE_LOG(LogTemp, Warning, TEXT("An Actor's name is %s"), *Actor->GetName());
```
>
>	- **Bool**
>	```c++
	UE_LOG(LogTemp, Warning, TEXT("The boolean value is %s"), ( bExampleBool ? TEXT("true"): TEXT("false") ));
	```
>
>	- **Integer**
>	```c++
	UE_LOG(LogTemp, Warning, TEXT("The integer value is: %d"), ExampleInteger);
	```
>
>	- **Float**
>	```c++
	UE_LOG(LogTemp, Warning, TEXT("The float value is: %f"), ExampleFloat);
	```
>
>	- **FVector**
>	```c++
	UE_LOG(LogTemp, Warning, TEXT("The vector value is: %s"), *ExampleVector.ToString());
	```