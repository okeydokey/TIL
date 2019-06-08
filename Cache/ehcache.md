# [Ehcache](https://www.ehcache.org/documentation/)

### 특징
#### 빠르고 가벼움
- 빠르고, 단순하고, 작은 foot print, 적은 의존성

#### 확장성(Scalable)
- 테라바이트 단위의 확장성 제공
- 수백개의 캐시 확장
- 동시성
- Terracotta를 이용해 분산 배치 아키텍처에서 클러스터 된 캐시 가능

#### 유연성
- Expiration 정책
- Eviction 정책
- Storage engines (on-heap, off-heap, disk)
- Static/Dynamic/Runtime 캐시 구성

#### 표준 기반
- JSR107 JCACHE API 전체 구현

#### 확장 가능(Extensible)
- Listener interfaces
- Decorator interfaces
- Cache Loaders, Cache Writers
- Exception Handlers

#### Listeners
- CacheManager listeners
  - notifyCacheAdded()
  - notifyCacheRemoved()
- Cache event listeners
  - notifyElementRemoved
  - notifyElementPut
  - notifyElementUpdated
  - notifyElementExpired

#### 분산 캐싱
- 고성능 분산 캐싱 제공(with Terracotta)

#### Enterprise Java and Applied Caching
- 동시 작업에 대한 중복 처리를 피하기 위해 캐시 차단
- SelfPopulating Cache(SelfPopulating Cache for pull through caching of expensive operations)
- Enterprise Java Gzipping Servlet Filter
  - CachingFilter
  - SimplePageCachingFilter
  - SimplePageFragmentCachingFilter

#### Cacheable Commands
- 비동기 동작, 내결함성 및 캐싱과 같은 신뢰할 수있는 오래된 명령 패턴, 명령을 생성하고 캐시 한 다음 실행을 시도

### 용어
#### Cache
- 미래에 필요하고 빠르게 검색 될 수 있는 저장소
- 다른 곳에있는 데이터를 복제하거나 계산 결과 인 임시 데이터의 모음. 이미 캐시에 있는 데이터는 시간과 자원을 최소한의 비용으로 반복적으로 액세스 가능

#### Cache Entry
- Cache Entry는 캐시내에 키와 키에 매핑 된 데이터 값으로 구성

#### Cache Hit
- 요청된 데이터가 캐시에 있을 캐시 히트라고 함

#### Cache Miss
- 요청된 데이터가 캐시에 없을 경우 캐시 미스라고 함

#### [System-of-Record (SoR)](../DataManagement/system-of-record.md)
- SOR은 데이터에 대한 신뢰할 수 있는 출처로서 캐시는 SOR에서 검색되고 저장됨
- SOR은 특수한 파일 시스템이거나 다른 신뢰할 수 있는 long-term 저장소 일 수 있지만, 대부분 전통적인 database 임
- 비용이 있는 계산과 같은 개념적 구성 요소 일 수 있도 있음

#### Eviction
- 더 새로운 항목을위한 공간을 만들기 위해 캐시에서 항목을 제거 (일반적으로 캐시의 데이터 저장소 용량이 부족한 경우).

#### Expiration
- 일정 시간이 지난 후에 캐시에서 항목을 제거. 일반적으로 캐시의 부실 데이터를 방지하기위한 전략

#### Hot Data
- 최근에 애플리케이션에서 사용한 데이터는 곧 다시 액세스 될 가능성이 큼. 이러한 데이터를 Hot Data라 함. 캐시는 가장 인기있는(hottest) 데이터를 가장 빨리 사용할 수 있도록 유지하면서 퇴출을 위해 가장 인기 없는 데이터(least hot)를 선택하려고 시도 함







