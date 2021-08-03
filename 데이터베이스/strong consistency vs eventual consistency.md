데이터베이스에서 consistency란 read 작업 시 데이터가 일관적으로 보여지는 것에 관한 성질이다.

분산 처리 시스템에서 등장하는 consistency model을 정리하려고 한다.

아래와 같은 상황을 가정해보자.

- 저장소가 로드 밸런싱을 위해 N, M 두 개로 복제되어 있다.
- 이 때, A가 저장소 N에 write 작업을 수행했다.
- 그리고 B가 저장소 M에 read 작업을 수행했다.

이 때, B에게 보이는 데이터의 상태는 두 가지 경우로 나뉠 것이다.

1. A의 write 작업이 반영된 최신 상태의 데이터
2. A의 write 작업이 아직 반영되지 않은 이전 상태의 데이터

저장소는 위 2가지 중 하나를 선택해야 할 것이다. 그리고 이렇게 consistency를 어떤 식으로 보장할 것인가에 대한 설계가 consistency model이다.

## strong consistency
strong consistency는 데이터가 항상 최신의 상태로 보여지는 것을 보장하는 모델이다.

![](/데이터베이스/images/strong-consistency.png)

write가 발생한 데이터의 업데이트가 완료되지 않았을 때, 해당 데이터에 대한 read access가 발생하면, 업데이트가 끝날 때까지 기다린 후 return한다.

따라서 strong consistency는 항상 최신의 데이터를 제공한다는 장점이 있지만, latency가 길어질 수 있다는 단점이 있다.

## eventual consistency
eventual consistency는 결과적으로는 모든 데이터의 consistency가 보장되지만, 항상 보장하지는 않는 모델이다.

![](/데이터베이스/images/eventual-consistency.png)

데이터 업데이트가 끝나지 않았어도 read access에 대해 결과를 return한다. 즉, 사용자는 업데이트가 끝나기 전까진 최신의 데이터를 볼 수도 있고, stale한 데이터를 볼 수도 있는 것이다.

따라서 eventual consistency는 짧은 latency를 제공하는 장점이 있지만, 항상 최신의 데이터가 제공되진 않는 단점이 있다.
