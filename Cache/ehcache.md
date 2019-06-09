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
- 더 새로운 항목을위한 공간을 만들기 위해 캐시에서 항목을 제거 (일반적으로 캐시의 데이터 저장소 용량이 부족한 경우)

#### Expiration
- 일정 시간이 지난 후에 캐시에서 항목을 제거. 일반적으로 캐시의 부실 데이터를 방지하기위한 전략

#### Hot Data
- 최근에 애플리케이션에서 사용한 데이터는 곧 다시 액세스 될 가능성이 큼. 이러한 데이터를 Hot Data라 함. 캐시는 가장 인기있는(hottest) 데이터를 가장 빨리 사용할 수 있도록 유지하면서 퇴출을 위해 가장 인기 없는 데이터(least hot)를 선택하려고 시도 함


### 개념
#### Data Freshness 와 Expiration
- Data Freshness
  - 데이터 신선도는 데이터의 원본(SOR) 버전의 최신 상태인 데이터 복사본의 Aspect(?)를 지칭 함. 부실 복사본은 SoR과 동기화되지 않았거나 동기화되지 않을 것으로 간주됨. 데이터베이스 (및 다른 SOR)는 데이터베이스 외부에서의 캐싱을 염두에두고 작성되지 않았으므로 일반적으로 데이터가 업데이트되거나 수정되었을 때 외부 프로세스에 알리는 기본 메커니즘이 제공되지 않음. 따라서 SOR에서 데이터를 로드 한 외부 구성 요소는 데이터가 부실하지 않도록 보장하는 직접적인 방법이 없음
- Cache Entry Expiration
  - Ehcache는 일정 시간 동안 캐시 항목을 만료시킴으로써 부실 데이터가 애플리케이션에서 사용되는 가능성을 줄이는 데 도움을 줌. 만료되면 항목이 캐시에서 자동으로 제거됨. 예를 들어, 캐시는 입력 된 후 5 초 후에 항목을 만료하도록 구성 할 수 있음(TTL, time-to-live) 또는 캐시에서 항목을 마지막으로 검색 한 후 17 초가 지나면 항목을 만료시킴(TTI, time-to-idle) 캐시에 가장 적합한 만료 구성은 애플리케이션의 요구 사항과 가정에 기초한 비즈니스 및 기술적 결정이 혼합 된 것

#### Storage Tiers
- Ehcache가 제공하는 데이터 저장소
  - On-Heap Store : Java의 힙 RAM 메모리를 사용하여 캐시 항목 저장. 이 계층은 Java 애플리케이션과 동일한 힙 메모리를 사용. JVM의 가비지 컬렉터에 의해 스캐닝됨 JVM이 사용하는 힙 공간이 많을수록 애플리케이션의 성능이 가비지 콜렉션 pause의 영향을 받음. 이 저장소는 매우 빠르지 만 일반적으로 가장 제한된 저장소 리소스
  - Off-Heap Store : 사용 가능한 RAM에 의해서만 크기가 제한됨(단일 시스템에서 6TB까지 테스트됨). Java 가비지 콜렉션(GC)의 적용을 받지 않음. 꽤 빠른 편이지만, 데이터가 저장되고 다시 액세스 될 때 JVM의 힙에서 이동해야하므로 On-Heap Store보다 느림
  - Disk Store : disk(filesystem)를 활용하여 캐시 엔트리 저장. 이 유형의 저장소 리소스는 대개 매우 풍부하지만 RAM 기반 저장소보다 훨씬 느림
  
#### Topology Types
- Standalone : 데이터 세트는 애플리케이션 노드에 보관. 다른 애플리케이션 노드는 서로 통신하지 않고 독립적. 동일한 애플리케이션을 실행하는 여러 애플리케이션 노드가 있는 곳에 Standalone 토폴로지가 사용되는 경우 해당 캐시는 완전히 독립적
- Distributed / Clustered : 데이터는 각 애플리케이션 노드에 보유 된 핫 데이터의 서브 세트로 원격 서버에 보관. 이 토폴로지는 일관성(consistency) 옵션을 제공. 분산 토폴로지는 클러스터 또는 확장 된 응용 프로그램 환경에서 권장되는 방법. 최고 수준의 성능, 가용성 및 확장 성을 제공
- 많은 프로덕션 애플리케이션이 가용성 및 확장성을 위해 여러 인스턴스의 클러스터에 배포됨. 그러나 분산 캐시가 없으면 응용 프로그램 클러스터는 다음과 같은 여러 가지 바람직하지 않은 동작을 나타냄
  - Cache Drift : 각 애플리케이션 인스턴스가 자체 캐시를 유지 관리하는 경우 한 캐시에 대한 업데이트가 다른 인스턴스에는 일어나지 않음. 분산 또는 복제 된 캐시는 모든 캐시 인스턴스가 서로 동기화되도록함
  - Database Bottlenecks : 단일 인스턴스 애플리케이션에서 캐시는 중복 쿼리 오버 헤드로부터 데이터베이스를 효과적으로 보호함. 그러나 분산 애플리케이션 환경에서 각 인스턴스는 자체 캐시를 로드하고 유지해야함. 여러 캐시를 로드하고 새로 고치는 오버 헤드로 인해 더 많은 응용 프로그램 인스턴스가 추가 될 때 데이터베이스 병목 현상이 발생. 분산 또는 복제된 캐시는 데이터베이스에서 여러 캐시를 로드하고 새로 고치는 데 필요한 인스턴스 별 오버 헤드를 제거
