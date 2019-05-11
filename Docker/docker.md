# [시작하세요! 도커](http://wikibook.co.kr/docker/)

## 도커란 ?
- **리눅스 컨테이너**에 여러 기능을 추가함으로써 애플리케이션을 **컨테이너**로서 좀 더 쉽게 사용할 수 있게 만들어진 오픈소스 프로젝트
- 그렇다면 리눅스 컨테이너란 ?
	- [가장 빨리 만나는 Docker 1장 - 1. 가상 머신과 Docker](http://pyrasis.com/book/DockerForTheReallyImpatient/Chapter01/01)에 매우 쉽게 설명되어 있음
	- http://opennaru.tistory.com/105 요기도 참조
	- 리눅스는 chroot 명령(프로세서의 root 디렉토리 변경)을 제공했으나 완벽한 가상 환경이 아님
	- 리눅스 컨테이너는 cgroup, namespace 사용하여 프로세스 단위의 가상의 격리 환경 제공
		- cgroup : 프로세스 그룹의 리소스 (CPU, 메모리, 디스크 I / O 등)의 이용을 제한, 격리
		- namespace : 격리된 가상 공간
- 기존의 가상화 기술은 하이퍼바이저를 이용하는 방식이였으나 여러가지 단점이 존재
	- 각종 시스템 자원 가상화, 독립된 공간 생성하는 작업이 하이퍼바이저를 거치기때문에 성능 손실 발생
	- 게스트 운영체제가 필요하므로 이미지 크기가 커짐
- 도커 컨테이너
	- 프로세스 단위의 격리 환경으로 성능 손실이 거의 없음
	- 호스트의 커널 공유로 애플리케이션 구동을 위한 라이브러리, 실행 파일만 존재하므로 이미지 용량이 대폭 줄어듦

### 도커 설치 for Mac
- [Docker for Mac](https://www.docker.com/docker-mac)
- [Docker Toolbox](https://www.docker.com/products/docker-toolbox)

#### Docker for Mac과 Docker Toolbox
- Docker for Mac
	- 가상 머신을 생성하지 않고 자체 가상화 기술로 리눅스 환경을 만들어 컨테이너 생성
- Docker Toolbox
	- PC에 리눅스 가상 머신을 생성한 뒤 도커를 설치하므로 가상 네트워크가 2개 생성됨
	- 2중 포트 포워딩 필요

## 도커 엔진
- 도커 엔진에서 사용하는 기본 단위는 이미지와 컨테이너

### 도커 이미지
- 컨테이너를 생성할 때 필요한 요소, 여러 개의 계층으로 된 바이너리 파일로 존재하고, 컨테이너를 생성하고 실행할 때 읽기 전용으로 사용
- [저장소 이름]/[이미지 이름]:[태그] 형태로 구성

### 도커 컨테이너
- 이미지로 컨테이너를 생성하면 해당이미지의 목적에 맞는 파일이 들어 있는 파일시스템과 격리된 시스템 자원 및 네트워크를 사용할 수 있는 독립된 공간이 생김
- 이미지를 읽기 전용으로 사용하되 이미지에서 변경된 사항만 컨테이너 계층에 저장하므로 변경 내용이 이미지에 영향을 미치지 않음

#### 도커 엔진 버전 확인
``` docker -v``` > ``` docker version```

#### 도커 시스템 정보 확인

```docker system info```

#### 도커 이미지 내려받기
``` docker pull centos:7 ``` > ```docker image pull ```

##### Docker Content Trust

- Docker 이미지의 정당성 확보

- 유효화

  ```export DOCKER_CONTENT_TRUST=1```

- 무효화

  ```export DOCKER_CONTENT_TRUST=0```

#### 도커 로그
``` docker logs XXX ``` > ```docker container logs```

#### 사용자 정의 네트워크 작성

```docker network create -d bridge webap-net```

```docker container run --net=webap-net -it centos```