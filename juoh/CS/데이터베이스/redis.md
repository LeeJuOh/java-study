# **레디스(Redis)란?**

- REDIS(REmote Dictionary Server)는 메모리 기반의 “키-값” 구조 데이터 관리 시스템이며, 모든 데이터를 메모리에 저장하고 조회하기에 빠른 Read, Write 속도를 보장하는 **비 관계형 데이터베이스**이다.
- 레디스는 크게 5가지< String, Set, Sorted Set, Hash, List >의 데이터 형식을 지원한다.
- Redis는 빠른 오픈 소스 인 메모리 키-값 데이터 구조 스토어이며, 다양한 인 메모리 데이터 구조 집합을 제공하므로 사용자 정의 애플리케이션을 손쉽게 생성할 수 있다.
- 주로 캐시를 사용할 때 사용한다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7a7db10d-f05f-4e51-9a17-aa52dca49c09/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7a7db10d-f05f-4e51-9a17-aa52dca49c09/Untitled.png)

## 그럼 이 캐시는 **어떻게 사용할까**?

![https://media.vlpt.us/images/hyeondev/post/d13edf64-100b-412c-a5ea-af0c80ef2147/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-10-03%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%201.07.33.png](https://media.vlpt.us/images/hyeondev/post/d13edf64-100b-412c-a5ea-af0c80ef2147/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-10-03%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%201.07.33.png)

- 일반적인 패턴 : `Look aside cache`이런 순서로 처리하는 방법이다.
    1. 웹 서버는 클라이언트 요청을 받아서, 데이터가 존재하는지 캐시를 **먼저** 확인한다.
    2. Cache 에 데이터가 있으면 그걸 꺼내주는데, 만약 없으면
    3. DB 에서 읽어서 -> 먼저 캐시에 저장한다음 클라이언트에게 데이터를 돌려준다.
- `Write Back`
    - 데이터를 캐시에 전부 먼저 저장해놓았다가 특정 시점마다 한번씩 캐시 내 데이터를 DB `insert` 하는 방법이다
    - `insert` 를 1개씩 500번 수행하는 것보다 500개를 한번에 삽입하는 동작이 훨씬 빠름에서 알 수 있듯, write back 방식도 성능면에서 뒤쳐지는 방식은 아니다.
    - 하지만 어쨌든 여기서 데이터를 일정 기간동안은 유지하고 있어야 하는데, 이때 이걸 유지하고 있는 storage 는 **메모리 공간**이므로 서버 장애 상황에서 데이터가 손실될 수 있다는 단점이 있다.
    - 그래서 다시 재생 가능한 데이터나, 극단적으로 heavy 한 데이터에서 `write back` 방식을 많이 사용한다.

## Redis 는 어떤 특징을 가지고 있을까.

캐시로 많이 사용하는 `Memcached` 와 `Redis` 의 가장 큰 차이는 `Collection` 을 제공하냐의 여부이다. Redis 에서는 `Collection` 을 제공한다.

`Collection` 은 개발의 편의성과 난이도에서 이점을 볼 수 있다고 하는데, 제공해주는 것들이 많기 때문이다. 예를 들어보자.

- 대상 사용자가 많은 경우 랭킹을 산출하는 서버를 구현하기
    - 이 때 디스크기반 storage 를 사용하게 된다면, 가져와야 하는 데이터 셋이 많아질수록 디스크 접근 횟수가 많아지므로 속도가 점점 느려질 수 밖에 없다.
    - Redis 의 `Sorted Set` 을 사용하면 랭킹 서버를 쉽게 구현 가능하며 replication 까지도 가능하다. 하지만 이렇게 제공하는 걸 가져다가 쓴다는 건 한계에 종속적이 되긴 한다.
- 친구 리스트를 관리할 때 데이터를 key-value 형태로 저장해야 한다면
    - 같은 친구 리스트를 읽은 후, 서로 다른 클라이언트에서 리스트에 서로 다른 친구를 추가하고자 했다고 가정해보자.
    - 이런 상황에서 친구 리스트의 최종 상태는 -> 두 클라이언트가 추가한 사람 A, B 가 전부 반영되지 않을 수 있다.
    - 이런걸 `race condition` 이라고 하는데, 지금 회사에서 스터디중인 `데이터 중심 애플리케이션 설계 #트랜젝션` 파트에서 이걸 다루고 있길래 도움이 될만한 그림을 가져와봤다.그림속 상황에 매치시켜보면,친구리스트에 `B` 와 `C` 를 동시에 추가하게 될 경우

      ![https://media.vlpt.us/images/hyeondev/post/6fafd94a-3b9e-4727-959a-4af2ea5cde60/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-10-03%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%201.49.40.png](https://media.vlpt.us/images/hyeondev/post/6fafd94a-3b9e-4727-959a-4af2ea5cde60/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-10-03%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%201.49.40.png)

      > T1[친구 B 추가] -> T2[친구 C 추가] -> T1[B 추가한걸 최종상태에 반영(쓰기)] -> T2[C 추가한걸 최종상태에 반영(쓰기)]
      >

      각 트랜젝션에서는 이런 순서로 로직을 처리하게 된다.그럼 우연한 타이밍에 3번째와 4번째 프로세스가 순서대로 진행되면서 리스트 덮어쓰기가 발생하고,최종 상태에서도 **결국 context switching 때문에 T1 과 T2 둘 중 뭐가 먼저 발생할지 예측할 수 없기 때문에** 친구리스트가 `[A,B]` or `[A,C]` 랜덤으로 유지될 수 있다.


Redis 자료구조는 **`Atomic 하다는 특징`** 때문에 이런 `race condition` 을 피할 수 있다.

- 씽글 쓰레드이긴 때문

즉, Redis Transaction 은 한번의 딱 하나의 명령만 수행할 수 있다. 이에 더하여 **`single-threaded 특성`** 을 유지하고 있기 때문에 다른 스토리지 플랫폼보다는 이슈가 덜하다고 한다.하지만 이 특징이 더블클릭 같은 동작으로 같은 데이터가 2번씩 들어가게 되는 불상사는 막을 수 없기 때문에 별도 처리가 필요하다.

따라서 레디스는 `remote data storage` 로서 여러 서버에서 같은 데이터를 공유하고 보고싶을 때 많이 사용한다. 그래서 우리는 인증 토큰을 저장하거나 유저 API limit 을 두는 상황 등에서 레디스를 많이 사용하고 있다.

## Redis vs Memcached

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4a33cde5-ae42-4db3-bcd2-85932a9d87b2/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4a33cde5-ae42-4db3-bcd2-85932a9d87b2/Untitled.png)

- In Memory
    - Redis는 읽기 쓰기의 빠른 액세스를 위해 키 값을 기본 메모리에 저장한다.
- Replication
    - Redis는 마스터-슬레이브 복제를 지원한다. 슬레이브에서 데이터 액세스를 수행하고 마스터에서 쓰기를 수행할 수 있다.
    - 이 Replication 확장성과 가용성을 제공한다. 슬레이브 중 하나가 실패하면 다른 슬레이브는 데이터 액세스를 제공한다.
- Data Structures
    - Redis는 문자열뿐만 아니라 목록, 세트, 해시 및 정렬된 세트도 저장한다.
    - memcached는 string만
- Virtual memory

  Redis는 메모리 저장소에 RAM을 사용하고, RAM이 부족할 때는 가상 메모리를 사용하여 데이터를 보관한다.

- Pub/Sub Model
    - Redis는 Redis 클라이언트가 데이터를 사용하기 위해 모든 채널에 가입할 수 있는 Publish and Subscribe 채널을 만들고 채널에 가입한 클라이언트는 데이터를 게시할 수 있다.(옵저버 패턴)
- Data Persistence
    - 정기적인 간격으로 인메모리 데이터를 파일 시스템에 보존한다.
    - Redis 노드가 실패하는 동안 Redis 데이터 파일에서 데이터를 복원할 수 있다.

그리고 Redis에는 다양한 SDK 지원 세트가 존재한다(http://redis.io/clients).

- 스택 오버플로, 깃헙 등 많은 서비스들이 레디스 사용

참고출처

- [https://velog.io/@hyeondev/Redis-란-무엇일까](https://velog.io/@hyeondev/Redis-%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%BC%EA%B9%8C)
- [https://dzone.com/articles/redis-an-introduction](https://dzone.com/articles/redis-an-introduction)
