---
title:  "Unreal ㅣ 5. 언리얼 오브젝트 리플렉션 시스템"
excerpt: "리플렉션 시스템과 클래스 디폴트 오브젝트"

categories:
  - Unreal
tags:
  - Unreal
  - Reflection System
  - Class Default Object
last_modified_at: 2025-08-02

toc: true
toc_label: ""
toc_sticky: true
---



# 📌 I. 리플렉션 시스템
[🔗Unreal Blog : Unreal Property System](https://www.unrealengine.com/ko/blog/unreal-property-system-reflection)

## [리플렉션 시스템]
**언리얼 오브젝트를 에디터, 직렬화, 네트워크 등 다양한 엔진 시스템에서 자동으로 다루기 위해** 메타 정보를 저장하고 접근하는 시스템
- 에디터 등 다양한 시스템이 리플렉션 시스템을 기반으로 일어남
- 언리얼 자체 개발 "옵션"
	- 유형이나 프로퍼티는 리플렉션을 사용할 수도, 안 할 수도 있음
	- 그러나 프로퍼티가 아닌 멤버 변수는 언리얼 엔진의 관리를 받지 않으므로 직접 처리해줘야 함에 유의 (ex 에디터 확인 불가, 가비지 콜렉터 등)
<br>
<br>

**동작 원리**  
리플렉션 매크로를 달면 Unreal Header Tool 이 컴파일 할 때 정보 수집
- Unreal Build Tool에서 빌드 시, 리플렉션이 있는 모듈이 지난 컴파일 이후 변경되었다면
- Unreal Header Toool 실행시켜 리플렉션 데이터 수집 및 업데이트
<br>
<br>

**리플렉션 시스템 사용**
- 헤더: `#include "FileName.generated.h"`
	- 최상단 include: 현재 클래스 헤더
	- 최하단 include:  `.generated.h` 
	- 순서를 지키지 않으면 컴파일 에러
- 리플렉션 매크로: 언리얼 오브젝트에는 특별한 프로퍼티와 함수 지정 가능
	- `UENUM()`, `UCLASS()`, `USTRUCT()`, `UFUNCTION()`, `UPROPERTY()` 등
	- 관리되는 클래스 멤버 변수: UPROPERTY
	- 관리되는 클래스 멤버 함수: UFUNCTION
<br>
<br>

## [프로퍼티]
**Property**
```C++
UPROPERTY(EditAnywhere, Category=Pawn) // 에디터에서 편집 가능하고 'Pawn' 카테고리에 노출
int32 ResourcesToGather;
```
- 괄호 안에 메타데이터를 넣어, 편집 범위와 Category 등 지정 가능
- UPROPERTY가 선언되지 않은 일반 멤버 변수 혼합 사용 가능
<br>
<br>

**프로퍼티 계층 구조**  
<img width="713" height="607" alt="Image" src="https://github.com/user-attachments/assets/58d96463-a9a0-43f8-b678-31637fb73a2e" />
<br>
<br>  

## [활용 실습]
리플렉션 얻어오기
- `FProperty* UClass::FindPropertyByName()`
	- UFunction 등도 유사하게 구함
<br>
<br>

프로퍼티 set / gete
- `FProperty::GetValue_InContainer(UClass\*, void\* out)`
	- FProperty는 클래스에 대한 속성이고 (선언정보),
	- 특정 객체에 접근해 값을 받아오거나 지정하려면 인자로 객체 지정해야 함
	- 리플렉션 시스템을 사용해 접근 지시자와 무관하게 프로퍼티 값 설정 가능
<br>
<br>

Function 실행
- `void UClass::ProcessEvent(\*UFunction, void* Param)`
<br>
<br>

>Asserttion Func
>- `check`: 조건이 false라면 에디터 종료 (릴리즈 모드에서는 동작 X)
>- `ensure`: 조건이 false라면 에디터 종료 없이 오류 정보 남김
>- `ensureMsgf`: `ensure` + 메세지까지 작성

# 📌II. 언리얼 오브젝트
## [언리얼 오브젝트]
- 언리얼 오브젝트의 클래스 정보
	- 클래스를 사용해 자신이 가진 프로퍼티와 함수 정보를 컴파일 타임과 런타임에서 조회 가능
	- 클래스 메타정보 구하기
		- `StaticClass()`: compile Time
		- `GetClass()`: run Time
- 언리얼 오브젝트 생성
	- `new 키워드` ❌
	- `NewObject()` ✅
<br>
<br>

>GetClass와 StaticClass의 차이
>- `StaticClass()`는 **클래스 타입에서** 호출해 클래스의 메타 정보 반환
>- `GetClass()`는 **객체 인스턴스에서** 호출하여 객체가 속한 클래스의 메타 정보 반환
  
<br>

## [UClass]
언리얼에서 **클래스에 대한 메타데이터(리플렉션 정보)**를 담고 있는 객체

- 메타 정보
	- 클래스를 설명하는 정보를 담은 객체
	- 클래스를 나타내는 실제 인스턴스가 아니라, 클래스에 대한 정보

- 포함 정보
    - 어떤 `UProperty`(멤버 변수)들이 있는지
    - 어떤 `UFunction`(멤버 함수)들이 있는지
    - 이 클래스의 기본 오브젝트(CDO)는 무엇인지
    - 이 클래스의 부모 클래스는 무엇인지
    - 생성자에서 어떤 값들이 기본값으로 설정되었는지 등
<br>
<br>

## [클래스 기본 오브젝트 (Class Default Object, CDO)]
<img width="1829" height="340" alt="Image" src="https://github.com/user-attachments/assets/7f4932ff-44a0-4ed4-91f0-9cc41a851794" />  
언리얼 클래스 정보에는 클래스 기본 오브젝트(Class Default Object, CDO)가 함께 포함됨
- CDO는 인스턴스 생성 시 해당 객체의 초기 상태를 결정하는 **기본 복사본 역할**을 하며, 생성자 내에서 설정한 기본값을 보관
- 한 클래스로부터 다수의 객체를 생성해 게임 콘텐츠에 배치할 때 일관성 있게 기본 값 조정하는데 유용
- UClass 및 CDO는 엔진 초기화 과정에서 생성
	- 기본값은 엔진이 켜지는 순간 초기화되므로,
	- 기본값이 변경되는 경우 (생성자 등) 에디터 재시작
- 클래스에서 CDO 얻기: `UClass::GetDefaultObject()` 
<br>
<br>

## [언리얼 오브젝트 처리]
[🔗Unreal Docs : Unreal Object Handling](https://dev.epicgames.com/documentation/ko-kr/unreal-engine/unreal-object-handling?application_version=4.27)
 - **기본 초기화 값**: UObject는 기본적으로 0으로 초기화 후 ➡️ 생성자 호출
 - **자동 레퍼런스**: 언리얼 시스템이 자동 메모리 관리해서 자동 레퍼런스 가능
 - **Serialization**: 정보 저장/로드 지원
 - **CDO**: 기본 값 관리 가능
 - **메타데이터**: 편집 범위, Category 등 지정 가능
 - **런타임 접근**: 런타임에서 유형을 얻고 형변환 가능
 - **가비지 컬렉션**: 자동으로 언리얼이 메모리 회수해서 관리
 - **네트워크 리플리케이션**: UProperty에 태그를 붙혀 정보 전송/세팅 가능