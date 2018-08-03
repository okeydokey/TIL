# [시작하세요! 도커](http://wikibook.co.kr/docker/)

## Dockerfile
- 이미지를 생성하는 법
	- 컨테이너 생성(ubuntu, centos) > 애플리케이션 설치 > 컨테이너 커밋
	- **Dockerfile 빌드**

- Dockerfile 이란 ?
	- 컨테이너에서 수행해야 할 작업 명시

- Dockerfile 명령어
	- FROM
		- 생성할 이미지의 베이스가 될 이미지를 뜻함.
		- Dockerfile을 작성할 때 반드시 한 번 이상 입력
		- 이미지 이름의 포맷은 docker run 명령어에서 이미지 이름을 사용했을 때와 같음
		- 사용하려는 이미지가 도커에 없다면 자동으로 pull
	- MAINTAINER
		- 이미지를 생성한 개발자의 정보
		- Dockerfile을 작성한 사람의 연락처(email 등)
		- docker inspect 명령어로 확인 가능
	- LABEL
		- 이미지에 메타데이터 추가
		- ‘키:값’ 형태로 저장되며 여러 개의 메타데이터 저장 가능
		- docker inspect 명령어로 확인 가능
	- RUN
		- 컨테이너 내부 명령어 실행
		- 배열 형태로 사용 가능 RUN [“실행 가능한 파일”, “명령줄 인자 1”, “명령줄 인자 2”]
		- ex) RUN apt-get update
		- ex) RUN apt-get install apache2 -y
			- 선택인자 미리 적용
		- ex) RUN [“/bin/bash”, “-c”, “echo hello >> test2.html”]
	- ADD
		- 파일을 이미지에 추가
		- Dockerfile이 위치한 디렉터리인 컨텍스트에서 가져옴
	- WORKDIR
		- 명령어를 실행할 디렉터리
		- 배시 셸 cd 명령어와 유사
	- EXPOSE
		- Dockerfile의 빌드로 생성된 이미지에서 노출할 포트
		- docker run -P 옵션과 함께 사용
	- CMD
		- 컨테이너가 시작될 때마다 실행할 명령어를 설정
		- Dockerfile에서 한 번만 사용

- Dockerfile 빌드
	- ``` docker build -t mybuild:0.0 ./ ```
	- -t : 생성될 이미지의 이름
	- ./ : dockerfile 위치
	- 빌드 과정
		1. 빌드 컨텍스트를 읽어들임
			- 빌드 컨텍스트 : 이미지를 생성하는 데 필요한 각종 파일, 소스코드, 메타데이터 등을 담고 있는 디렉터리, Dockerfile이 위치한 디렉터리가 빌드 컨텍스트가 됨. 빌드될 이미지에 파일 추가시 사용
			- 컨텍스트에는 불필요한 파일이 포함되지 않도록 주의
			- .dockerignore 컨텍스트에서 제외할 파일 설정
		2.  Dockerfile에 기록된 대로 컨테이너 생성 및 이미지 커밋
			- ADD, RUN등의 명령어가 실행 될 때마다 새로운 컨테이너가 하나씩 생성 이를 이미지로 커밋
			- Dockerfile의 명령어 줄 수 만큼 레이어 존재
			- 한번 이미지 빌드를 마치고 다시 같은 빌드 실행 시 캐시 사용
			- 캐시를 사용하지 않으려면 --no-cache 옵션 추가
			- 캐시로 사용할 이미지 지정하려면 --cache-from 이미지명 을 사용