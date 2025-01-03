---
title:  "GitBlog ㅣ 1. 블로그 시작하기"
excerpt: "jekyll 설치하고 테마 적용해 새 블로그 호스팅하기"

categories:
  - GitBlog
tags:
  - GitBlog
last_modified_at: 2024-11-12

toc: true
toc_label: ""
toc_sticky: true
---



# 📒 1. jekyll 설치
RubyInstaller 다운로드 + jekyll 설치

## 설치과정
1. RubyInstaller-2.4 이상 버전 다운로드 (Ruby+DevKit, 디폴폴트 옵션)
	- <https://rubyinstaller.org/downloads/>


2. 설치마법사 : 설치 시 PATH 추가 옵션 선택


3. 설치마법사 : 마지막에 ridk install 옵션 선택
    ![Pasted image 20241111025959](https://github.com/user-attachments/assets/2f301cfa-a2fd-4191-b3b8-e5da6129563f)


4. Installer 창 : 3번 (MSYS2 and MIGW development toolchain) 선택
    ![Pasted image 20241111030201](https://github.com/user-attachments/assets/064f6baf-4eda-4cc7-81f5-d72694c184d6)


5. cmd 창 : jekyll 설치
    - cmd창에 `gem install jekyll bundler` 입력
	- 설치 항목이 주르륵 뜨다가
	![image](https://github.com/user-attachments/assets/10c1fde3-3a59-4d99-9f1e-762fb6b3e23b)
	

    - 완료표시가 나온다
	
		![Pasted image 20241111030628](https://github.com/user-attachments/assets/befef9e9-97c9-4bb7-8ad2-d756e6628806)
		(난 업데이트까지 그냥 해줬다)


6. cmd 창 : 설치 확인
    - cmd창에 `jekyll -v` 입력

		![image](https://github.com/user-attachments/assets/172b8bb6-b7c2-4b54-bd4f-3dfc6b64dd93)


## References
- [🔗Docs : jekyll 윈도우 설치 문서](https://jekyllrb.com/docs/installation/windows/)


# 📒 2. 테마 다운로드

## 설치과정
1. 테마 선택 : minimal-mistakes
	- 디자인이 깔끔하고 커스텀하기 용이
	- 가장 많은 사람들이 사용하므로 자료가 많음
	- 업데이트가 꾸준함 (현재진행형)


2. Git fork
	- 불필요한 파일 삭제

		![Pasted image 20241111041423](https://github.com/user-attachments/assets/344fe5a6-6c54-4896-b439-913001920120)


3. theme 설치 : Remote theme method
	1. 폴더 내 Gemfile 콘텐츠 변경
		```
		source "https://rubygems.org"
		
		gem "github-pages", group: :jekyll_plugins
		gem "jekyll-include-cache", group: :jekyll_plugins
		```
	2. 폴더 내 \_config.yml 파일 내용 수정
		- plugins 항목에 jekyll-include-cache 포함되어있는지 확인

	3. gems Fetch & update
        - cmd창에 `bundle` 입력

	4. 폴더 내 \_config.yml 파일 내용 수정
		- `remote_theme: "mmistakes/minimal-mistakes@4.26.2"` 추가
		- 기존  `theme:` or `remote_theme:` 항목은 삭제


4. 로컬 호스팅 테스트
    - cmd창에 `bundle exec jekyll serve`
    - 출력되는 서버 주소로 접속해서 로컬 호스팅 테스트


## References
- [🔗Github : mmistakes/minimal-mistakes](https://github.com/mmistakes/minimal-mistakes)
- [🔗Docs : minimal-mistakes 설치 가이드](https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/#remote-theme-method)


# 📒 3. 온라인 배포
1. GitHub Repo 세팅
	- Github Repo - Setting - Pages
	- 세팅 변경
		- Source - Deploy from a brach
		- Branch - master


2. 깃허브 상단에 호스팅 안내메세지 확인
	![Pasted image 20241111041208](https://github.com/user-attachments/assets/738827ea-568a-4776-9943-6e6ee46b9fd5)


3. 온라인 호스팅 완료
	![Pasted image 20241111041235](https://github.com/user-attachments/assets/e489b14f-a5ed-4ae3-ad2d-4035a402187a)