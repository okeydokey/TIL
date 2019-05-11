# [시작하세요! 도커](http://wikibook.co.kr/docker/)

## 도커 이미지
- 일반적으로 도커 허브(Docker Hub)라는 중앙 이미지 저장소에서 이미지를 내려 받음
- 도커 허브는 도커가 공식적으로 제공하고 있는 이미지 저장소
- Private를 사용하려면 비공개 저장소의 수에 따라 요금 지불
- 도커 사설 레지스트리를 직접 구축해 비공개로 사용 가능

### 도커 이미지 검색
``` docker search ubuntu ```

- --no-trunc : 결과를 모두 표시
- --limit : n건의 검색 결과를 표시
- --filter=stars=n : 즐겨찾기의 수(n 이상)를 지정

### 도커 이미지 생성
``` docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]] ```

``` docker run -i -t --name commit_test ubuntu:14.04 ```
``` root@acc5231241:/# echo test_first! >> first ```
``` docker commit -a "alicek106" -m "my first commit" commit_test commit_test:first ```

- -a : author
- -m : 커밋메시지
- 이미지를 커밋할 때 컨테이너에서 변경된 사항만 새로운 레이어로 저장하고, 그 레이어를 포함해 새로운 이미지를 생성
- 즉, ubuntu image가 188MB라고 해서 commit_test:first가  188MB로 생성되는 것이 아니고 변경된 사항만 저장됨
- 이미지를 삭제할 경우 해당 이미지를 기반으로  사용하는 이미지가 있을 경우 실제 이미지 파일을 삭제하지 않고 레이어에 부여된 이름만 삭제

### 도커 이미지 삭제

``` docker image rm [옵션] 이미지명 [이미지명]```

- --force, -f : 이미지를 강제로 삭제
- --no-prune : 중간 이미지를 삭제하지 않음

```docker image prune [옵션]```

- 사용하지 않은 이미지 삭제

- --all, -a : 사용하지 않은 이미지를 모두 삭제
- --force, -f : 이미지를 강제로 삭제

### 도커 이미지 태그

``` docker image tag ubuntu okey dokey/myubuntu:1.0```

- 이미지에 별명을 붙일 뿐 이미지 자체를 복사하거나 이름을 바꾼 것은 아님

### 이미지 구조
``` docker inspect ubuntu:14.04 ``` > ```docker image inspect ubuntu:14.04```

- 이미지 ID, 작성일, Docker 버전, CPU 아키텍처 등을 알 수 있음
- --format : 포멧 지정
  - ex) --format="{{.Os}}", --format="{{.ContainerConfig.Image}}"

### 이미지 추출
``` docker save -o ubuntu_14_04.tar ubuntu:14.04 ```

- 컨테이너의 커맨드, 이미지 이름과 태그 등 이미지의 모든 메타데이터를 포함해 하나의 파일로 추출
- -o : 추출될 파일명

``` docker load -i ubuntu_14_04.tar ```

- 추출된 이미지를 도커에 다시 로드

``` docker export -o rootFS.tar mycontainer ```
``` docker import rootFS.tar myimage:0.0 ```

- export 명령어는 컨테이너의 파일시스템을 tar파일로 추출하며 컨테이너 및 이미지에 대한 설정 정보를 저장하지 않음

### 이미지 배포
- save나 export와 같은 방법으로 이미지를 단일 파일로 추출해서 배포하는 것은 이미지 파일의 크기가 너무 크거나 도커 엔진의 수가 많다면 배포하기 어려우며 레이어 형태를 이용하지 않으므로 매우 비효율적

#### 도커 허브 이미지 저장소를 사용하여 배포
``` docker login ```
``` docker push ``` > ```docker image push 이미지명[:태그명]```
``` docker pull ```

- 도커 허브에서는 조직, 팀 구성을 제공
- 저장소에 이미지가 push됐을 때 URL로 http 요청을 전송하도록 설정 기능 제공(웹훅)

#### 도커 사설 레지스트리 사용
##### 사설 레지스트리 컨테이너 생성
- 도커 이미지로 제공
``` docker run -d --name myregistry -p 5000:5000 --restart=always registry:2.6 ```

- --restart : 컨테이너가 종료됐을 때 재시작에 대한 정책을 설정
	- always : 컨테이너가 정지 될 때마다 다시 시작하도록 설정(도커 호스트나 엔진을 재시작하면 컨테이너도 함께 재시작)
	- on-failure : 컨테이너 종료 코드가 0이 아닐 경우 컨테이너 재시작을 지정한 수만큼 시도
	- unless-stopped : 컨테이너를 stop명령어로 정지했다면 도커 호스트나 도커 엔진을 재시작해도 컨테이너가 시작되지 않도록 설정

##### 사설 레지스트리 컨테이너 생성
- 이미지 이름 추가
``` docker tag [기존의 이미지 이름][새롭게 생성될 이미지 이름] ```
``` docker tag myimagename:0.0 ${DOCKER_HOST_IP}:5000/myimagename:0.0 ```

- 이미지의 접두어를 레지스트리 컨테이너가 존재하는 호스트의 IP와 레지스트리 컨테이너의 5000번 포트와 연결된 호스트의 포트로 설정해야 함

- 이미지 push
``` docker push localhost:5000/myimagename:0.0 ```

> push 가 되지 않을경우
기본적으로 도커 데몬은 HTTPS를 사용하지 않은 레지스트리 컨테이너에 접근하지 못하도록 설정
DOCKER_OPTS=“--insecure-registry=XXX.XXX.XXX.XXX:5000” 을 추가한 뒤 도커 재시작
--insecure-registry 옵션은 HTTPS를 사용하지 않은 레지스트리 컨테이너에 이미지를 push, pull할 수 있게 설정

- 이미지 pull
``` docker pull localhost:5000/myimagename:0.0 ```


- 사설 레지스트리 컨테이너 삭제
	- 레지스트리 컨테이너는 생성됨과 동시에 컨테이너 내부에 마운트되는 도커 볼륨을 생성
	- push된 이미지 파일은 이 볼륨에 저장되며 레지스트리 컨테이너가 삭제돼도 볼륨은 남아있음
	- --volumes 옵션을 추가하면 볼륨도 함께 삭제
``` docker rm --volumes myregistry ```


##### Nginx 서버로 접근 권한 설정
- 미리 정의된 계정으로 로그인하도록 설정함으로써 접근을 제한
- Self-Signed 인증서와 키를 발급함으로써 TLS를 적용
	- Self-signed ROOT 인증서(CA) 파일 생성
```
mkdir certs
openssl genrsa -out ./certs/ca.key 2048
openssl req -x509 -new -key ./certs/ca.key -days 10000 -out ./certs/ca.crt
```

	- 레지스트리  컨테이너에 사용될 인증서 생성
```
openssl genrsa -out ./certs/domain.key 2048
openssl req -new -key ./certs/domain.key -subj /CN=localhost -out ./certs/domain.csr
echo subjectAltName = IP:localhost > extfile.cnf
openssl x509 -req -in ./certs/domain.csr -CA ./certs/ca.crt -CAkey ./certs/ca.key -CAcreateserial -out ./certs/domain.crt -days 10000 -extfile extfile.cnf
```


##### 사설 레지스트리 RESTful API


##### 사설 레지스트리에 옵션 설정