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
``` docker -v```

#### 도커 이미지 내려받기
``` docker pull centos:7 ```

#### 컨테이너 생성
``` docker run -i -t ubuntu:14.04```
- run : 컨테이너를 생성하고 실행
- ubuntu:14.04 : 컨테이너를 생성하기 위한 이미지 이름 (이미지가 로컬 도커 엔진에 존재하지 않을 경우 도커 중앙 이미지 저장소인 도커 허브에서 자동으로 이미지를 내려 받음)
- -i : 상호 입출력
- -t : tty 활성화해서 배시 셸 사용

``` docekr create -i -t --name mycentos centos:7```
- create : 컨테이너를 생성하기만 할 뿐 컨테이너로 들어가지 않음
- --name : 컨테이너의 이름 설정

#### 컨테이너 시작 및 내부로 들어가기
``` docker start mycentos```
``` docker attach mycentos ```

#### 컨테이너 내부에서 빠져나오기
- exit를 셸에 입력 또는 Ctrl + D 입력 : 컨테이너 내부에서 빠져나오면서 동시에 컨테이너를 정지 시킴
- Ctrl + P, Q : 컨테이너의 셸에서만 빠져나옴

#### 도커 이미지 목록
``` docker images ```

#### 컨테이너 목록
``` docker ps ```
- 실행 중인 컨테이너 목록
- -a : 정지된 컨테이너를 포함한 모든 컨테이너 출력

``` docker ps --format "table {{.ID}}\t{{.Status}}\t{{.Image}}\t{{.Names}}" ```
- 원하는 정보만 출력하도록 --format 옵션 사용

#### 컨테이너 정지
``` docker stop mycentos ```

#### 컨테이너 삭제
``` docker rm mycentos ```
``` docker rm -f mycentos ```
- -f : 실행 중인 컨테이너 삭제

``` docker container prune ```
- 모든 컨테이너 삭제

``` docker rm $(docker ps -a -q) ```
- 컨테이너 상태와 관계없는(-a) 모든 컨테이너의 ID(-q) 리스트를 변수로하여 삭제

#### 컨테이너 외부 노출
``` docker run -i -t name mywebserver -p 80:80 ubuntu:14.04 ```
- -p : 컨테이너의 포트를 호스트의 포트와 바인딩해 연결할 수 있게 설정 [호스트의 포트]:[컨테이너의 포트]
- [이미 컨테이너가 존재할 경우 설정 방법](https://stackoverflow.com/questions/19335444/how-do-i-assign-a-port-mapping-to-an-existing-docker-container)

#### 도커 로그
``` docker logs XXX ```