# 7.6.1 4G/5G 네트워크에서의 이동성 관리

4G/5G 네트워크에서 이동성을 지원하기 위해 네트워크의 다양한 요소들이 상호 협력하며 동작한다.        

## 이동성 시나리오
홈 네트워크를 거쳐 데이터가 전달되는 간접 연결 시나리오
![image](https://github.com/user-attachments/assets/cc2092c4-bdfb-42c1-99c8-df4857f07e45)

1. 스마트폰 → 기지국: 스마트폰이 근처의 기지국과 연결하여 초기 신호 설정 및 통신을 시작.
2. 기지국 ↔ MME: 기지국이 MME로 이동 장치의 제어 신호를 전달하여 네트워크와 상호작용을 시작.
3. MME ↔ HSS: MME는 HSS와 통신하여 사용자의 인증 및 가입자 정보를 확인.(홈 구독 서버안에 IMSI가 저장되어 있다.)
4. MME ↔ S-GW: MME는 서빙 게이트웨이(S-GW)와 데이터 터널을 설정.
5. S-GW ↔ P-GW: S-GW는 홈 네트워크의 PDN 게이트웨이(P-GW)와 데이터를 터널링하여 인터넷에 연결.
6. P-GW → 인터넷: 최종적으로, 데이터는 P-GW를 통해 인터넷(예: 스트리밍 서버)에 도달.


## 이동성 시나리오

### 1. 이동 장치와 기지국 결합

이동 장치는 방문 네트워크의 기지국과 결합한다.

- 기지국 정보 획득: 이동 장치는 주변 기지국에서 전송되는 신호를 수신하여 최적의 기지국을 선택한다.
- 결합 과정: 이동 장치는 선택된 기지국과 제어 신호 채널을 초기 설정하며, IMSI 및 가입자 정보를 기지국에 제공한다.

### 2. 이동 장치에 대한 네트워크 요소의 제어 평면 구성

- MME와의 신호 설정: 기지국은 MME와 접촉하여 이동 장치의 인증, 암호화, 네트워크 서비스 정보를 확인한다.
- 상태 갱신: MME는 HSS에 이동 장치의 현재 위치를 알리고 데이터베이스를 갱신한다.
- 데이터 평면 준비: 기지국과 이동 장치는 데이터 평면 채널에 대한 매개변수를 설정한다.

#### 홈 네트워크(HSS)가 관여하는 이유

- 가입자 인증: HSS는 이동 장치의 IMSI와 인증 데이터를 기반으로 사용자가 네트워크에 접속할 권한이 있는지 확인한다.
- 서비스 품질 보장: 이동 장치가 필요로 하는 QoS 정보와 네트워크 리소스 상태를 기반으로 최적의 서비스를 제공하도록 한다.
- 로밍 지원: 방문 네트워크에서도 홈 네트워크가 이동 장치의 요금제와 서비스 이용 내역을 추적하고 과금을 관리한다.

> 비유: 홈 네트워크(HSS)를 사용자의 본국 정부로 생각할 수 있다. 사용자가 해외(방문 네트워크)에 있어도 본국 정부는 신원 인증, 권리 보장, 로밍 과금 관리를 지원한다.

### 3. 이동 장치에 대한 포워딩 터널의 데이터 평면 구성

- 터널 설정: 데이터 평면은 두 개의 주요 터널로 구성된다.
  1. 기지국<-> 서빙케이트웨이: 이 터널은 이동 장치가 현재 연결된 기지국과 방문 네트워크 내부의 서빙 게이트웨이를 연결하는 경로이다. 이를 통해 기지국에서 발생하는 모든 데이터가 서빙 게이트웨이로 전달된다.
  2. 서빙 게이트웨이 <-> 홈 네트워크의 PDN 게이트웨이 사이: 이 터널은 방문 네트워크와 홈 네트워크를 연결하며, 데이터가 홈 네트워크의 PDN 게이트웨이를 통해 인터넷으로 전달될 수 있도록 한다.
- GTP 프로토콜: 터널은 GTP(GPRS Tunneling Protocol)를 사용한다. 이 프로토콜은 TEID(Tunnel Endpoint Identifier)를 활용하여 여러 데이터 흐름을 한 터널로 다중화하거나 여러 터널로 나누는 역할을 한다.      
예를 들어, 하나의 서빙 게이트웨이와 여러 기지국 사이에서 다수의 터널이 존재할 수 있는데, TEID는 각 데이터그램이 속한 터널을 식별하는 데 사용된다. 이를 통해 데이터는 목적지까지 효율적으로 전달된다.
---
### SGW와 PGW가 따로 필요한 이유

### 1. SGW(Serving Gateway): 로컬 데이터 관리
- 기능: SGW는 기지국(eNodeB)과 PGW 사이에 위치하며, 사용자의 로컬 네트워크 데이터를 관리한다.
  - 기지국 연결 관리: 이동 장치(UE)가 기지국을 이동할 때도 데이터 세션이 끊기지 않도록 핸드오버 지원 역할을 한다.
  - 로컬 터널링: UE가 기지국과 서빙 게이트웨이 간 데이터를 교환할 때 데이터를 중계한다.
  - 부하 분산: 네트워크의 로컬 영역에서 데이터 처리와 연결을 효율적으로 수행해 PGW로의 부하를 줄인다.

- 필요성: 
  - UE가 기지국 간 이동(핸드오버)을 할 때 SGW가 데이터 연결의 안정성을 보장해준다.
  - 핸드오버 최적화: SGW가 기지국과 가까운 곳에 위치하기 때문에 데이터 경로가 최적화된다.

---

### 2. PGW(PDN Gateway): 인터넷과의 연결 관리
- 기능: PGW는 외부 네트워크(PDN, Packet Data Network)와의 연결을 담당한다.
  - IP 주소 할당: PGW는 이동 장치(UE)에 인터넷 사용을 위한 IP 주소를 할당한다.
  - 패킷 라우팅: 외부 네트워크(예: 인터넷, 사설 네트워크)로 데이터를 전달하거나 수신하는 역할을 한다.
  - QoS(서비스 품질) 관리: 각 사용자 데이터 흐름에 대해 QoS를 적용하여 우선순위 또는 대역폭 제한을 관리한다.
  - 트래픽 필터링 및 과금: 사용자별 트래픽을 모니터링하고 과금 정책을 적용한다.

- 필요성:
  - 네트워크 보안: 외부 인터넷에 직접 노출되지 않도록 데이터를 PGW에서 처리한다.
  - 서비스 분리: PGW는 QoS와 과금 같은 정책을 관리하는 역할에 집중할 수 있다.

---

### 3. SGW와 PGW의 분리 필요성
SGW와 PGW를 분리하면 다음과 같은 장점이 있다:

1. 역할 분리로 효율성 증가:
   - SGW는 로컬 네트워크(기지국-사용자) 데이터 관리를 담당.
   - PGW는 외부 네트워크 연결 및 IP 기반 서비스 관리를 담당.
   - 이로 인해 데이터 관리와 라우팅이 분리되어 네트워크가 더 효율적으로 동작한다.

2. 핸드오버 최적화:
   - SGW는 UE의 위치 변화를 따라 이동하지만, PGW는 변하지 않는다.
   - 사용자가 기지국을 이동할 때 SGW만 변경되며, PGW는 그대로 유지되어 세션 연결이 끊기지 않는다.

3. 확장성과 유연성:
   - SGW와 PGW가 독립적이기 때문에, 네트워크 확장이나 특정 부분의 업그레이드를 쉽게 수행할 수 있다.
   - 예를 들어, 특정 지역의 데이터 트래픽이 급증하면 SGW를 증설하고, 전체 네트워크의 데이터 처리를 개선할 수 있다.

4. 트래픽 분산:
   - SGW는 로컬 데이터를 처리하며, PGW는 외부 인터넷 트래픽을 처리하므로 트래픽이 효율적으로 분산된다.
   - 하나의 게이트웨이에 모든 역할을 맡기는 경우 트래픽 병목이 발생할 가능성이 높아진다.

---

### 4. GTP(GPRS Tunneling Protocol)의 역할
SGW와 PGW 사이의 통신은 GTP 프로토콜을 사용하여 이루어진다. GTP는 각 데이터 패킷이 어떤 터널에 속하는지를 나타내기 위해 TEID(Tunnel Endpoint Identifier)를 사용한다.

- 예: 
  - 한 사용자의 데이터 세션은 TEID 100으로 식별되며, SGW에서 PGW로 전달된다.
  - 여러 사용자의 세션이 한 터널에 묶여도, TEID를 통해 각 사용자를 구분하여 데이터가 정확히 전달된다.

---

### 5. 정리
SGW와 PGW의 분리는 네트워크의 핸드오버, 트래픽 관리, 보안, 확장성을 위해 필수적이다. SGW는 기지국과 가까운 위치에서 로컬 트래픽을 처리하며, PGW는 홈 네트워크 및 인터넷 연결을 담당한다. 이를 통해 데이터가 효율적으로 전달되고, 네트워크 성능과 안정성이 크게 향상된다.

---

### 5G 네트워크에서의 이동성 향상

- 5G 네트워크는 작은 셀 크기로 인해 핸드오버 빈도가 높아진다.
- SDN 기반의 제어 평면 구현으로 고용량, 저지연의 이동성을 지원할 수 있다.

---

# 7.6.2 이동 IP

이동 IP는 인터넷에서 이동 중인 사용자에게 이동성을 지원하는 구조 및 프로토콜로, 다음과 같은 세 가지 주요 기능을 제공한다.

### 1. 에이전트 발견

- 외부 에이전트는 이동 장치에게 COA(Care-of Address)를 제공하고, 홈 네트워크와의 등록 및 데이터그램 송수신을 지원한다.

### 2. 홈 에이전트와의 등록

- 이동 장치는 홈 에이전트에 COA를 등록하여 데이터그램이 올바르게 전달되도록 한다.

### 3. 데이터그램의 간접 라우팅

- 데이터그램은 홈 에이전트를 통해 터널링되며, 오류 처리 및 다양한 형태의 터널링 규칙이 포함된다.

### 4. 이동 IP에서 간접 통신이 기본인 이유

- 중앙화된 제어: 홈 네트워크는 이동 장치의 위치를 추적하고, 로밍 중에도 안정적인 서비스를 제공하도록 관리한다.
- 보안 강화: 데이터가 홈 네트워크를 경유하므로, 이동 장치의 인증과 암호화 상태를 지속적으로 확인할 수 있다.
- 호환성 보장: 다양한 방문 네트워크 환경에서도 안정적인 통신을 유지할 수 있다.

### 직접 연결이 로밍을 제공하지 않는 이유



1\. 보안 및 인증 부족 &#x20;

&#x20;   직접 연결은 이동 장치와 방문 네트워크 간에 데이터를 바로 송수신하기 때문에 홈 네트워크(HSS)가 인증이나 암호화를 제어할 수 없다. &#x20;

&#x20;   이는 보안 위험을 초래할 수 있으며, 이동 장치가 불법적인 네트워크에 연결될 가능성을 높인다.



2\. 가입자 관리 어려움 &#x20;

&#x20;   홈 네트워크는 가입자 정보를 관리하고, 사용량에 따라 요금을 책정한다. &#x20;

&#x20;   직접 연결은 홈 네트워크를 우회하므로, 로밍 중 발생한 데이터 사용량 추적이나 정확한 과금이 어렵다.



3\. 네트워크 간 호환성 문제 &#x20;

&#x20;   이동 장치가 방문 네트워크와 직접 통신하려면, 두 네트워크 간의 프로토콜 호환성이 보장되어야 한다. &#x20;

&#x20;   이 과정에서 충돌이나 데이터 손실이 발생할 수 있으므로, 홈 네트워크를 경유하는 방식이 더 안정적이다.



4\. 이동성 지원 부족 &#x20;

&#x20;   직접 연결은 네트워크 간 핸드오버(이동 장치가 한 기지국에서 다른 기지국으로 이동)를 원활히 처리하기 어렵다. &#x20;

&#x20;   반면 간접 연결은 홈 네트워크가 중심 역할을 하며, 핸드오버 시 데이터를 빠르게 재경로 설정할 수 있다.



---



### 결론

직접 연결은 지연 시간과 대역폭 절약 측면에서 유리할 수 있으나, 보안, 가입자 관리, 이동성 지원 측면에서 한계가 있다. &#x20;

따라서 로밍을 제공하는 환경에서는 홈 네트워크를 경유하는 간접 연결이 기본으로 사용된다.





---

## 추가 설명 및 비유

### SGW와 PGW 동작 예시

아래는 실제 사용자가 인터넷에 접속하고 데이터를 전송하는 과정에서 SGW(Serving Gateway)와 PGW(PDN Gateway)가 어떻게 동작하는지를 설명하는 예시입니다.

---

### 상황: 사용자가 스마트폰(UE)으로 유튜브 동영상을 스트리밍할 때

#### 1. 네트워크 초기 연결 (Attach Process)  
- UE(사용자 단말): 스마트폰이 기지국(eNodeB)에 연결하고 네트워크 서비스를 요청한다.
- MME(Mobility Management Entity): 사용자의 인증을 수행한다. 이를 위해 HSS(Home Subscriber Server)에서 사용자의 IMSI(고유 식별자)를 확인하고, 인증 및 보안 절차를 완료한다.
- 인증이 완료되면, MME가 SGW와 PGW에 데이터 세션을 생성하라고 요청한다.

---

#### 2. SGW 역할: 로컬 데이터 관리  
- SGW는 사용자와 기지국 간의 데이터를 중계한다.
- 데이터 터널 생성: 
  - 기지국과 SGW 간에 GTP 터널을 생성한다.
  - 이 터널은 스마트폰에서 발생한 데이터(유튜브 동영상 요청 등)를 SGW로 전달하는 경로가 된다.
- 핸드오버 관리:
  - 사용자가 이동 중에 기지국이 바뀌더라도 SGW는 세션을 유지하여 동영상 스트리밍이 끊기지 않도록 한다.

---

#### 3. PGW 역할: 외부 네트워크 연결 관리
- SGW에서 전달받은 데이터는 PGW로 전달된다.
- 인터넷 연결:
  - PGW는 사용자의 데이터를 외부 네트워크(인터넷)로 전달한다.
  - 예를 들어, 유튜브 서버에 동영상 스트리밍 요청을 보낸다.
- IP 주소 할당:
  - PGW는 사용자에게 인터넷 통신을 위한 IP 주소를 할당한다.
  - 이 IP 주소를 통해 유튜브 서버가 응답 데이터를 정확히 사용자에게 전달할 수 있다.
- QoS 관리:
  - PGW는 사용자의 데이터 트래픽 우선순위(QoS)를 적용한다. 예를 들어, 유튜브 스트리밍은 고품질 영상 전송을 위해 높은 우선순위가 부여될 수 있다.

---

#### 4. 데이터 흐름
- 다운로드 데이터 처리:
  - 유튜브 서버에서 동영상 데이터를 인터넷으로 전달하면, 이 데이터는 PGW → SGW → 기지국(eNodeB) → 스마트폰으로 전달된다.
- 업로드 데이터 처리:
  - 스마트폰에서 유튜브 서버로의 데이터 요청도 SGW와 PGW를 통해 처리된다.

---

### 핸드오버 시 동작
- 사용자가 동영상을 보며 이동할 때, 다른 기지국(eNodeB)으로 연결이 변경되는 경우:
  - SGW는 새로운 기지국과 연결을 설정하고 기존 데이터를 이어받는다.
  - PGW는 변화 없이 인터넷 연결을 유지하며, 세션의 연속성을 보장한다.

---

### 정리
1. SGW(Serving Gateway):
   - 사용자와 기지국 간 데이터 전송 담당.
   - 핸드오버 시 세션 유지.
   - 로컬 네트워크에서 데이터 라우팅 최적화.

2. PGW(PDN Gateway):
   - 인터넷 또는 외부 네트워크 연결 담당.
   - IP 주소 할당, QoS 관리, 트래픽 모니터링.
   - SGW와 통신하여 데이터를 외부 네트워크로 전달.

---

### 시각적 예
1. 업로드 흐름:
   - 스마트폰(UE) → 기지국(eNodeB) → SGW → PGW → 인터넷(유튜브 서버).
   
2. 다운로드 흐름:
   - 인터넷(유튜브 서버) → PGW → SGW → 기지국(eNodeB) → 스마트폰(UE).

---

이러한 동작을 통해 SGW와 PGW는 역할을 분리하여 데이터 전달을 효율적으로 수행하고, 사용자가 이동 중에도 끊김 없이 서비스를 사용할 수 있도록 보장한다.
