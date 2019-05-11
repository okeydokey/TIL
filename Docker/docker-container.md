# [시작하세요! 도커](http://wikibook.co.kr/docker/)

## 도커 컨테이너

#### 컨테이너 생성

- 이미지로부터 컨테이너 생성

``` docker run -i -t ubuntu:14.04```  > ```docker container run -i -t ubuntu:14.04```

- run : 컨테이너를 생성하고 실행
- ubuntu:14.04 : 컨테이너를 생성하기 위한 이미지 이름 (이미지가 로컬 도커 엔진에 존재하지 않을 경우 도커 중앙 이미지 저장소인 도커 허브에서 자동으로 이미지를 내려 받음)
- -i : 상호 입출력
- -t : tty 활성화해서 배시 셸 사용
- --attache, -a : 표준 입력, 표준 출력, 표준 오류 출력에 attache 함
- --cidfile : 컨테이너 ID를 파일로 출력
- --detach, -d : 컴테이너를 생성하고 백그라운드에서 실행
- --interactive. -I : 컨테이너 표준 입력
- --tty, -t : 단말기 디바이스 사용
- --uesr, -u : 사용자명 지정
- --restart=[no|on-failure|on-failure:횟수n|always|unless-stopped] : 명령 실행 결과에 따라 재시작을 하는 옵션
  - no : 재시작하지 않음
  - on-failure : 종료 스테이터스가 0이 아닐 때 재시작
  - on-failure:횟수n : 종료 스테이터스가 0이 아닐 때 n번 재시작
  - always : 항상 재시작
  - unless-stopped : 최근 컨테이너가 정지 상태가 아니라면 항상 재시작 ?
- --rm : 명령 실행 완료 후에 컨테이너를 자동으로 삭제
- --add-host=[호스트명:IP주소] : 컨테이너의 /etc/hosts에 호스트명과 IP주소를 정의
- --dns=[IP주소] : 컨테이너용 DNS서버의 IP 주소 지정
- --expose : 지정한 범위의 포트 번호 할당
- --max-address=[MAC  주소] : 컨테이너의 MAC 주소를 지정
- --net=[bridge|none|container:<name|id>|host|NETWORK] : 컨테이너의 네트워크를 지정
- --hostname, -h : 컨테이너 자신의 호스트명을 지정
- --publish, -p[호스트의 포트 번호]:[컨테이너의 포트 번호] : 호스트와 컨테이너의 포트 매핑
- --publish-all, -p : 호스트의 임의의 포트를 컨테이너에 할당
- --env=[환경변수], -e : 환경변수 설정
- --env-file=[파일명] : 환경변수를 파일로부터 설정

``` docekr create -i -t --name mycentos centos:7```>```docekr container create -i -t --name mycentos centos:7```

- 이미지에 포함될 Linux의 디렉토리와 파일들의 스냅샷을 취함
  - 스냅샷이란 ? 스토리지 안에 존재하는 파일과 디렉토리를 특정 타이밍에 추출 한 것

- create : 컨테이너를 생성하기만 할 뿐 컨테이너로 들어가지 않음
- --name : 컨테이너의 이름 설정

#### 컨테이너 시작 및 내부로 들어가기

``` docker start mycentos```
``` docker attach mycentos ```

#### 컨테이너 내부에서 빠져나오기

- exit를 셸에 입력 또는 Ctrl + D 입력 : 컨테이너 내부에서 빠져나오면서 동시에 컨테이너를 정지 시킴
- Ctrl + P, Q : 컨테이너의 셸에서만 빠져나옴

#### 도커 이미지 목록

``` docker images ``` > ```docekr image ls```

- -all, -a : 모든 이미지 표시
- --digests : 다이제스트를 표시할지 말지
- --no-trunc : 결과를 모두 표시
- --quiet, -q : Docker 이미지 ID만 표시

#### 컨테이너 목록

``` docker ps ``` > ```docker container ps``` > ```docker container ls```

- 실행 중인 컨테이너 목록
- -a : 정지된 컨테이너를 포함한 모든 컨테이너 출력

``` docker ps --format "table {{.ID}}\t{{.Status}}\t{{.Image}}\t{{.Names}}" ```

- 원하는 정보만 출력하도록 --format 옵션 사용

#### 컨테이너 가동 확인

```docekr container stats mycentos```

#### 컨테이너에서 사용 중인 프로세스 확인

```docker container top mycentos```

#### 컨테이너 정지

``` docker stop mycentos ```

#### 컨테이너 강제 종료

```docker container kill```

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

#### 