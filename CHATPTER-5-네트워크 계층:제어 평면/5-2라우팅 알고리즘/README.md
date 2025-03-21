### 5.2 라우팅 알고리즘

---

###  1. 라우팅 알고리즘의 목표 
- 송신자부터 수신자까지  네트워크를 통과하는 경로(루트) 를 결정한다.
- 가장 중요한 목표는  최소 비용 경로(least-cost path) 를 찾는 것이다.
- 현실적인 문제:
  - 네트워크 정책에 따라 특정 경로를 선택하거나 제한할 수 있다.
  - 예: Y 기관의 라우터는 Z 기관의 패킷을 전달하지 않아야 한다는 정책.

---

###  2. 라우팅 문제를 그래프로 표현 

####  그래프 G(N, E)의 구성 요소 
1.  노드(Node) :
   - 그래프상의 노드는  라우터 를 나타낸다.
   - 라우터는 패킷 전달과 경로 선택을 결정하는 지점이다.
2.  에지(Edge) :
   - 그래프상의 에지는  라우터 간의 물리적 링크 를 나타낸다.
   - 에지는 링크의  비용  값을 가진다.
     - 비용의 기준: 링크 거리, 속도, 금전적 비용 등.

####  비용 표현 
- 에지 (x, y)의 비용:  c(x, y) .
- 노드 x와 y가 연결되어 있을 경우, x와 y는 서로의  이웃(neighbor) 이다.

---

###  3. 경로(Path)의 정의 
-  경로는 그래프에서  노드의 연속된 순서 로 표현된다.
-  경로의 비용 : 당 경로상의 모든 에지 비용의 합으로 계산된다.

---

###  4. 라우팅 알고리즘이 고려해야 할 요소 
-  최소 비용 경로 :
  - 각 에지 비용을 최소화하는 경로를 선택한다.
-  정책 제약 :
  - 특정 노드나 에지를 배제하는 조건이 있을 수 있다.
-  동적 네트워크 상황 :
  - 네트워크 상태(링크 장애, 혼잡도 등)에 따라 경로가 동적으로 변경될 수 있다.

---

###  5. 요약 
라우팅 알고리즘은  그래프(G(N, E))로 모델링된 네트워크 에서  최소 비용 경로 를 찾는 것을 목표로 한다.  
노드는 라우터를, 에지는 라우터 간의 링크를 나타내며, 에지의 비용은 링크 속성에 따라 결정된다.  
현실적으로는 네트워크 정책과 동적 상황을 고려하여 경로를 선택해야 한다. 

---
## 라우팅 알고리즘의 분류

라우팅 알고리즘은 중앙 집중형과 분산형 두 가지 방식으로 분류할 수 있다.

---

### 1. 중앙 집중형 라우팅 알고리즘
- 정의:
  - 네트워크 전체에 대한 완전한 정보를 기반으로 출발지와 목적지 사이의 최소 비용 경로를 계산하는 방식이다.
- 특징:
  1. 전체 네트워크 상태 정보:
     - 네트워크 내 모든 노드와 링크의 상태(비용)를 알고 있어야 한다.
  2. 집중된 계산:
     - 경로 계산은 한 장소에서 수행되거나, 계산 결과를 각 라우터로 복사하여 사용한다.
  3. 링크 상태 알고리즘(LS):
     - 네트워크 내 모든 링크의 상태 정보를 알고 있기 때문에 링크 상태 알고리즘이라고도 한다.
- 장점:
  - 전역적 최적화 가능.
  - 모든 경로가 최신 정보를 기반으로 계산됨.
- 단점:
  - 네트워크가 커질수록 상태 정보 관리와 계산 복잡도가 증가.
  - 중앙 집중형 시스템 장애 시 네트워크 전체가 영향을 받을 수 있음.

---

### 2. 분산 라우팅 알고리즘
- 정의:
  - 최소 비용 경로 계산이 라우터들 간의 분산된 방식으로 반복 수행되는 알고리즘이다.
- 특징:
  1. 부분 정보 기반 시작:
     - 각 라우터는 자신에게 직접 연결된 링크의 비용 정보만 알고 있다.
  2. 반복적 계산:
     - 이웃 노드와 정보를 교환하며 점진적으로 목적지까지의 최소 비용 경로를 계산한다.
  3. 거리 벡터 알고리즘(DV):
     - 각 라우터는 네트워크 내 다른 모든 노드까지의 비용 추정치를 벡터 형태로 유지한다.
- 장점:
  - 네트워크 확장성 높음.
  - 분산 방식이므로 단일 장애 지점 없음.
- 단점:
  - 전역 최적화가 어려움.
  - 계산 과정에서 정보 전파 지연이 발생할 수 있음.
  - 특정 상황에서 루프(loop)가 발생할 위험.

---

### 3. 비교

| 구분             | 중앙 집중형 라우팅 알고리즘                     | 분산 라우팅 알고리즘                      |
|---------------------|-------------------------------------------|-------------------------------------------|
| 정보 기반        | 네트워크 전체 상태 정보 필요                    | 직접 연결된 링크 정보만으로 시작                  |
| 계산 방식        | 중앙에서 경로 계산 후 배포                      | 각 라우터가 분산적으로 계산                     |
| 알고리즘 종류     | 링크 상태 알고리즘 (Link-State)                | 거리 벡터 알고리즘 (Distance-Vector)         |
| 장점             | 전역 최적화 가능                              | 확장성 높고 단일 장애 지점 없음                 |
| 단점             | 복잡한 상태 관리, 중앙 장애 시 네트워크 영향          | 루프 발생 가능성, 정보 전파 지연 발생             |
| 적용 사례        | OSPF (Open Shortest Path First)           | RIP (Routing Information Protocol), BGP  |

---

### 4. 요약
- 중앙 집중형:
  - 네트워크 전체 정보를 활용해 최적 경로를 계산하는 방식으로 링크 상태 알고리즘을 사용한다.
- 분산형:
  - 라우터들이 직접 정보 교환과 계산을 통해 경로를 찾아가는 방식으로 거리 벡터 알고리즘을 사용한다.
- 선택은 네트워크의 규모, 요구 사항, 확장성, 관리 용이성에 따라 달라진다.

---

### 라우팅 알고리즘의 두 번째 분류: 정적 라우팅 알고리즘과 동적 라우팅 알고리즘

---

### 1. 정적 라우팅 알고리즘 (Static Routing Algorithm)
- 정의:
  - 네트워크 관리자가 수동으로 경로를 설정하며, 경로가 거의 고정적으로 유지되는 방식이다.
  - 링크 비용이나 경로 변경이 필요할 경우, 사람이 직접 수정을 해야 한다.

- 특징:
  1. 경로는 매우 느리게 변함:
     - 네트워크의 트래픽 부하나 토폴로지 변화에 즉각적으로 반응하지 않음.
  2. 단순하고 안정적:
     - 사전에 정의된 경로를 사용하므로 예측 가능성이 높다.

- 장점:
  - 구현과 관리가 간단하다.
  - 안정적이며, 경로 진동(oscillation)과 같은 문제가 발생하지 않는다.

- 단점:
  - 네트워크 변화에 대한 대응이 느리다.
  - 대규모 네트워크에서 관리가 어려울 수 있다.

---

### 2. 동적 라우팅 알고리즘 (Dynamic Routing Algorithm)
- 정의:
  - 네트워크의 트래픽 부하, 토폴로지 변화에 따라 라우팅 경로를 자동으로 변경하는 방식이다.
  - 경로를 주기적으로 업데이트하거나, 토폴로지나 링크 비용의 변경에 즉각적으로 반응한다.

- 특징:
  1. 유연한 경로 변경:
     - 네트워크 상태에 따라 자동으로 최적의 경로를 계산.
  2. 네트워크 상황 반영:
     - 혼잡이나 링크 장애 발생 시 경로를 자동으로 우회.
  3. 라우팅 프로토콜 사용:
     - RIP, OSPF, BGP와 같은 동적 라우팅 프로토콜을 통해 작동.

- 장점:
  - 네트워크 변화에 빠르게 대응.
  - 대규모 네트워크에서 관리 용이.

- 단점:
  - 경로 루프(loop)와 경로 진동(oscillation) 문제가 발생할 수 있다.
  - 계산과 정보 교환에 따른 오버헤드가 증가.

---

### 3. 토폴로지와의 관계
- 토폴로지(topology):
  - 네트워크의 링크와 노드가 물리적으로 연결된 방식 또는 논리적 연결 형태를 나타낸다.
  - 라우팅 알고리즘은 이러한 토폴로지를 기반으로 동작하며, 동적 라우팅 알고리즘은 토폴로지 변화에 민감하게 반응한다.

---

### 4. 비교

| 구분               | 정적 라우팅 알고리즘                  | 동적 라우팅 알고리즘                  |
|-----------------------|--------------------------------------|--------------------------------------|
| 경로 설정 방식      | 사람이 수동으로 설정                     | 네트워크 상태에 따라 자동 변경              |
| 변화 대응 속도       | 느림                                   | 빠름                                   |
| 관리 복잡도         | 낮음                                   | 높음                                   |
| 문제 발생 가능성     | 안정적, 경로 루프나 진동 없음                | 경로 루프, 경로 진동 발생 가능               |
| 적용 사례          | 소규모, 고정된 네트워크                  | 대규모, 동적으로 변화하는 네트워크            |
| 사용 프로토콜       | 없음                                   | RIP, OSPF, BGP 등                    |

---

### 5. 요약
- 정적 라우팅 알고리즘은 관리자가 직접 설정하며, 간단하고 안정적이지만 네트워크 변화에 적응하지 못한다.
- 동적 라우팅 알고리즘은 네트워크 상태에 따라 자동으로 경로를 변경하며, 유연하고 확장성이 있지만 경로 루프나 진동에 취약할 수 있다.
- 네트워크의 크기, 변화 빈도, 안정성 요구에 따라 적합한 알고리즘이 선택된다.

---

### 라우팅 알고리즘의 세 번째 분류: 부하 민감 여부

라우팅 알고리즘은 링크의 현재 부하(혼잡)를 반영하여 경로를 선택하는지에 따라 부하에 민감한 알고리즘과 부하에 민감하지 않은 알고리즘으로 분류할 수 있다.

---

### 1. 부하에 민감한 알고리즘 (Load-Sensitive Algorithm)
- 정의:
  - 링크 비용이 현재 혼잡 상태에 따라 동적으로 변화하는 알고리즘이다.
- 특징:
  - 혼잡한 링크에 더 높은 비용을 부과하여 혼잡 우회 경로를 선택하려는 경향이 있다.
  - 링크의 트래픽 부하를 실시간으로 반영해 경로를 동적으로 변경한다.
- 장점:
  - 네트워크 혼잡을 완화할 수 있다.
  - 트래픽 부하 분산으로 특정 링크에 과부하가 걸리는 상황을 방지.
- 단점:
  - 경로 진동(oscillation) 문제가 발생할 수 있다.
    - 예: 혼잡 링크를 우회하려는 트래픽이 다른 링크를 혼잡하게 만들어 반복적으로 경로를 변경.
  - 부하 정보를 지속적으로 갱신해야 하므로 계산 및 통신 오버헤드가 증가.
- 사례:
  - 초기 ARPAnet 라우팅 알고리즘이 부하 민감한 방식으로 설계되었으나, 경로 진동 문제가 심각하게 나타남.

---

### 2. 부하에 민감하지 않은 알고리즘 (Load-Insensitive Algorithm)
- 정의:
  - 링크 비용이 현재 부하나 혼잡 상태를 반영하지 않는 알고리즘이다.
- 특징:
  - 링크 비용은 고정되거나, 물리적 거리, 대역폭 등과 같은 정적인 기준으로 설정된다.
  - 트래픽 부하 변화가 라우팅 경로 선택에 영향을 미치지 않는다.
- 장점:
  - 경로 선택이 안정적이며, 진동 문제가 발생하지 않는다.
  - 계산과 통신 오버헤드가 적어 성능이 효율적이다.
- 단점:
  - 특정 링크에 과도한 트래픽 부하가 걸릴 수 있다.
  - 혼잡 상황을 고려하지 않으므로 트래픽 최적화가 어렵다.
- 사례:
  - 오늘날의 주요 라우팅 알고리즘(RIP, OSPF, BGP 등)이 부하에 민감하지 않은 방식으로 동작.

---

### 3. 비교

| 구분               | 부하에 민감한 알고리즘                  | 부하에 민감하지 않은 알고리즘             |
|-----------------------|--------------------------------------|--------------------------------------|
| 링크 비용           | 현재 혼잡 상태에 따라 동적으로 변화             | 고정되거나 정적인 기준(물리적 거리, 대역폭 등) |
| 혼잡 반영 여부       | 반영                                    | 반영하지 않음                            |
| 경로 변경 빈도       | 빈번할 수 있음                            | 안정적으로 유지                            |
| 장점               | 혼잡 우회 가능, 트래픽 부하 분산               | 경로 안정성, 계산 및 통신 오버헤드 감소         |
| 단점               | 경로 진동 문제, 계산 및 통신 오버헤드 발생       | 혼잡 상황 고려 불가, 트래픽 최적화 어려움        |
| 적용 사례          | 초기 ARPAnet                             | RIP, OSPF, BGP                       |

---

### 4. 요약
- 부하에 민감한 알고리즘은 혼잡한 링크를 우회하는 경로를 동적으로 선택하지만, 경로 진동 문제와 계산 부담이 단점이다.
- 부하에 민감하지 않은 알고리즘은 경로 안정성이 뛰어나고 계산 오버헤드가 적지만, 혼잡 상황에 대한 대응이 어렵다.
- 오늘날 인터넷에서는 안정성과 효율성을 이유로 부하 민감하지 않은 알고리즘이 주로 사용된다.

---

### 5.2.1 링크 상태(LS) 라우팅 알고리즘

---

### 1. 링크 상태 알고리즘의 기본 개념
- 네트워크의 토폴로지와 모든 링크 비용 정보가 알고리즘의 입력값으로 사용된다.
- 각 노드는 자신과 직접 연결된 링크의 식별자와 비용 정보를 브로드캐스트하여 네트워크상의 모든 노드에 이 정보를 전달한다.
- 일반적으로 인터넷 라우팅 프로토콜 중 OSPF가 이 방식을 따른다.

---

### 2. 다익스트라 알고리즘(Dijkstra's Algorithm)
- 목적: 
  - 하나의 출발 노드에서 네트워크 내 모든 노드로의 최소 비용 경로를 계산.
- 특징:
  - k번째 반복: 최소 비용 경로가 계산된 k개의 목적지 노드가 결정된다.
  - 중앙 집중형 알고리즘으로 초기화 단계와 반복 단계로 구성된다.
- 기호 정의:
  -  D(v) : 출발지 노드에서 목적지  v 까지의 최소 비용 경로의 비용.
  -  p(v) :  v  직전에 있는 노드.
  -  N' : 출발지에서 최소 비용 경로가 계산된 노드들의 집합.

---

### 3. 알고리즘 수행 결과
- 링크 상태 알고리즘은 각 노드의 포워딩 테이블을 생성한다.
  - 포워딩 테이블은 각 목적지에 대한 최소 비용 경로상의 다음 홉 노드 정보를 저장한다.
- 계산 복잡도:
  - 네트워크 노드 수가  n 일 때, 알고리즘의 최악 시간 복잡도는  O(n^2) .

---

### 4. 진동(oscillation) 문제
- 발생 원인:
  - 링크 비용이 트래픽 부하에 따라 변하는 경우, 경로가 지속적으로 변경되면서 경로 진동 문제가 발생할 수 있다.
- 예시:
  - 모든 노드가 최소 비용 경로를 선택하지만, 이 선택이 전체 네트워크 부하를 증가시켜 비용이 다시 변경되는 과정에서 진동 발생.
- 해결 방법:
  1. 라우터들이 동시에 링크 상태 알고리즘을 실행하지 않도록 설정.
  2. 링크 상태 정보를 송신하는 시각을 임의로 설정해 자기 동기화(self-synchronization)를 방지.

### 진동 문제의 영향과 해결 방법

#### 문제가 되는 이유
1. 지연 증가  
   - 경로가 계속 변경되면서 패킷이 불필요하게 우회하거나 지연이 발생한다.

2. 라우팅 테이블 불안정  
   - 경로 변경으로 인해 라우터가 자주 계산 및 갱신을 반복하며 추가적인 메시지 트래픽이 발생한다.

---

#### 해결 방법
1. 부하 민감 라우팅 제한  
   - 링크 비용이 트래픽 부하에 따라 급격히 변하지 않도록 제한한다.

2. 동기화 방지  
   - 라우터들이 동시에 라우팅 알고리즘을 실행하지 못하도록 실행 시간을 랜덤화한다.

3. 최소 변동 제한  
   - 링크 비용이 변하더라도 일정 임계값 이상일 때만 라우팅 경로를 변경하도록 설정한다.

---

### 5. 요약
- 링크 상태 알고리즘은 전체 네트워크 정보를 기반으로 최소 비용 경로를 계산하며, 다익스트라 알고리즘이 사용된다.
- 포워딩 테이블은 계산 결과를 통해 생성되며, 각 노드는 목적지에 대한 최적의 다음 홉을 알게 된다.
- 트래픽 부하 기반 링크 비용이 사용될 경우, 진동 문제가 발생할 수 있으며, 이를 방지하기 위한 기법이 필요하다.

---


### 5.2.2 거리 벡터(DV) 라우팅 알고리즘

---

### 1. 거리 벡터 라우팅 알고리즘의 특징
- 분산적(distributed):
  - 각 노드는 자신의 이웃 노드로부터 정보를 받고 이를 사용해 계산하며, 계산 결과를 다시 이웃에게 전달한다.
- 반복적(iterative):
  - 이웃 간 정보 교환이 완료되어 더 이상 변화가 없을 때까지 계속 동작한다.
- 비동기적(asynchronous):
  - 모든 노드가 동시에 동작할 필요가 없다.

---
### 5.2.2 거리 벡터(DV) 라우팅 알고리즘

---

### 1. 거리 벡터 알고리즘의 특징
1. 분산적(distributed):  
   - 각 노드는 이웃 노드로부터 정보를 받아 계산하고, 계산 결과를 이웃에게 전달한다.
2. 반복적(iterative):  
   - 이웃 간 정보 교환이 완료되고 더 이상 변화가 없을 때까지 동작을 반복한다.
3. 비동기적(asynchronous):  
   - 모든 노드가 동시에 동작할 필요가 없으며 독립적으로 작동한다.

---

### 2. 벨만-포드 식과 동작 과정
#### 벨만-포드 식:
<img width="327" alt="Image" src="https://github.com/user-attachments/assets/16ec4ee3-ef05-442c-9fe0-d688e91a579e" />

#### 동작 과정:
1. 초기화:  
   - 각 노드는 이웃까지의 링크 비용만 알고 있으며, 나머지 경로는 무한대로 설정한다.
2. 거리 벡터 교환:  
   - 각 노드는 자신의 거리 벡터를 이웃에게 전송하고, 이웃의 정보와 벨만-포드 식을 사용해 갱신한다.
3. 수렴:  
   - 더 이상 갱신 메시지가 없을 때 알고리즘은 멈춘다.

---

### 3. 장점과 단점
#### 장점:
1. 분산 구조로 동작하여 대규모 네트워크에 적합.
2. 구현이 간단하고, 독립적으로 작동 가능.

#### 단점:
1. 무한 계수 문제(count-to-infinity):  
   - 링크 비용 증가 시 경로 루프가 발생하며 비용이 계속 증가한다.
2. 수렴 속도 문제:  
   - 알고리즘이 최적의 경로에 도달하기까지 시간이 오래 걸린다.

---

### 4. 무한 계수 문제와 포이즌 리버스
#### 무한 계수 문제:
- 예:  경로를 반복적으로 갱신. 비용이 잘못 계산되어 점점 증가하는 라우팅 루프 발생.

#### 포이즌 리버스(Poisoned Reverse):
- 해결 방법:  
  -  z가 y를 통해  x로 가는 경로를 사용 중이면,  z는  y에게  x까지의 비용이 무한대라고 알림.  
  -  y는  z를 경유하는 경로를 고려하지 않게 된다.
- 한계:  
  - 루프에 3개 이상의 노드가 포함된 경우 해결 불가.

---

### 5. 실생활 적용 예시
1. 소규모 네트워크:
   - RIP(Routing Information Protocol):  
     소규모 네트워크에서 거리 벡터 알고리즘을 사용하며, 간단한 구현으로 효율적이다.
2. 대규모 네트워크:  
   - 대규모 네트워크에서는 무한 계수 문제와 느린 수렴 속도로 인해 거리 벡터 알고리즘 대신 링크 상태 알고리즘(OSPF 등)이 사용된다.

---

### 7. 요약
- 거리 벡터 알고리즘은 벨만-포드 식을 기반으로 한 분산적, 반복적, 비동기적 방식의 라우팅 알고리즘이다.  
- 간단한 구현으로 소규모 네트워크에 적합하지만, 무한 계수 문제와 수렴 속도의 한계를 가진다.  
- RIP 등에서 사용되며, 대규모 네트워크에서는 링크 상태 알고리즘이 더 선호된다.

---
### 링크 상태 알고리즘(LS)과 거리 벡터(DV) 라우팅 알고리즘 비교

---

### 1. 경로 계산 방법

| 특징                     | LS 알고리즘                                    | DV 알고리즘                                     |
|------------------------------|--------------------------------------------------|--------------------------------------------------|
| 전체 정보 필요 여부       | 네트워크의 전체 정보를 필요로 함.                 | 오직 직접 연결된 이웃과 정보 교환.                 |
| 정보 전달 방식            | 브로드캐스트를 통해 모든 노드와 통신.             | 이웃끼리 거리 벡터(최소 비용 경로의 추정값)만 교환. |
| 링크 비용 정보 전파        | 직접 연결된 링크 비용만 알림.                     | 변경된 링크 비용만 전파.                          |

---

### 2. 메시지 복잡성

| 특징                     | LS 알고리즘                                    | DV 알고리즘                                     |
|------------------------------|--------------------------------------------------|--------------------------------------------------|
| 메시지 수                 | O(N × E): 네트워크 내 각 링크 비용을 모든 노드에 전달. | 각 반복마다 이웃 간 메시지 교환.                    |
| 갱신 방식                 | 모든 링크 비용의 변경 정보를 브로드캐스트.          | 링크 비용 변화 시, 영향을 받는 노드만 갱신 및 전파. |
| 수렴 속도                 | 상대적으로 빠름.                                 | 네트워크 크기와 노드 상태에 따라 달라질 수 있음.   |

---

### 3. 견고성

| 특징                     | LS 알고리즘                                    | DV 알고리즘                                     |
|------------------------------|--------------------------------------------------|--------------------------------------------------|
| 라우터 고장 시 영향       | - 잘못된 링크 비용 정보가 브로드캐스트될 가능성.    | - 잘못된 최소 비용 경로가 전체 네트워크에 확산될 가능성. |
| 정보 손실 또는 변질        | - 링크 상태 브로드캐스트 패킷이 변질/폐기될 수 있음. | - 잘못된 계산이 간접적으로 모든 이웃에게 전달됨.  |
| 확산 문제                | - 각 노드가 자신의 포워딩 테이블만 계산하므로 견고성이 있음. | - 한 노드의 잘못된 계산이 전체 네트워크로 확산될 가능성. |

#### 실제 사례: 1997년 ISP 라우팅 장애
- 발생 원인: 한 ISP의 오작동 라우터가 잘못된 라우팅 정보를 백본 라우터에 제공.  
- 결과: 대규모 트래픽이 오작동한 라우터로 집중되면서 인터넷의 상당 부분이 몇 시간 동안 단절.

---

### 4. 결론
- LS 알고리즘은 정보가 네트워크 전체에 전달되므로 견고성이 상대적으로 높으나, 브로드캐스트로 인한 메시지 복잡성이 크다.
- DV 알고리즘은 메시지 복잡성이 낮고 분산적 방식으로 작동하지만, 잘못된 정보 확산 가능성으로 인해 견고성이 떨어질 수 있다.
- 두 알고리즘 모두 명백히 우위를 점한다고 볼 수 없으며, 실제로 인터넷에서 LS와 DV 알고리즘 모두 사용되고 있다.

---
 LS 알고리즘이 중앙 집중형 경로 계산 방식을 사용한다고 해서 반드시 중앙화된 시스템 하나가 모든 라우터를 제어하는 것은 아니다.       
LS 알고리즘은 "네트워크 전체 정보를 이용해 각 라우터가 독립적으로 계산"하는 분산된 중앙 집중형 방식이다.

### LS 알고리즘의 작동 방식과 견고성
1. 중앙 집중형의 의미  
   - LS 알고리즘은 모든 라우터가 네트워크의 전체 토폴로지와 모든 링크 비용을 알고 있음을 전제로 한다.
   - 그러나 경로 계산은 각 라우터에서 독립적으로 수행되므로 계산 과정 자체는 분산적이다.
   - 중앙의 "제어 서버"가 모든 계산을 처리하는 방식이 아니다.

2. 라우터 고장 시의 영향  
   - 특정 라우터가 고장나면, 그 라우터가 브로드캐스트하는 링크 상태 정보가 다른 라우터에게 잘못된 데이터로 전달될 수 있다.
   - 하지만 LS 알고리즘에서 각 라우터는 자신의 경로를 독립적으로 계산하므로, 고장난 라우터의 영향이 전체 네트워크에 확산되지 않고 국지적으로 제한된다.
   - 따라서 하나의 라우터 고장으로 인해 네트워크 전체가 중단되지는 않는다.

3. 견고성의 이유
   - LS 알고리즘은 각 라우터가 네트워크 정보를 바탕으로 자신의 포워딩 테이블만 계산한다.
   - 네트워크의 다른 라우터는 잘못된 정보를 감지하거나, 고장난 라우터를 경로에서 제외하는 방식으로 문제를 완화할 수 있다.

---
### 결론
- LS 알고리즘은 중앙 집중형 방식으로 네트워크 전체 정보를 필요로 하지만, 경로 계산은 각 라우터에서 독립적으로 수행되므로, 한 라우터의 고장이 네트워크 전체를 마비시키지는 않는다.
- 오히려 DV 알고리즘에서 잘못된 정보가 네트워크 전역으로 확산될 가능성이 더 크기 때문에, LS 알고리즘이 상대적으로 더 견고하다고 평가된다.
