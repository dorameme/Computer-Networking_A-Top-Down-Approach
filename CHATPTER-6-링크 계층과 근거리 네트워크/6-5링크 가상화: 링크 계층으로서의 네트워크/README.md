## 6.5 링크 가상화: 링크 계층으로서의 네트워크

### 핵심 정리

1. 링크 가상화의 개념

   - 링크 가상화는 물리적 네트워크를 추상화하여 논리적인 링크 계층을 형성하는 기술이다.
   - 이를 통해 여러 장치나 네트워크가 마치 단일 링크처럼 동작하도록 만든다.

2. 링크 계층 역할

   - 링크 계층은 데이터를 물리적 네트워크를 통해 전달하기 위한 프로토콜과 규칙을 정의한다.
   - 가상화된 링크 계층은 물리계층의 복잡성을 숨기고, 더 단순한 논리적 네트워크로 변환한다.
   - 비유: 링크 가상화는 물리적 네트워크를 하나의 큰 전기 배선도로 만들고, \
     각각의 연결된 장치를 한꺼번에 제어할 수 있는 스위치처럼 동작하게 한다. 복잡한 배선을 일일이 조정할 필요 없이 간단히 작동한다.

3. 링크 가상화의 주요 기술

   - MPLS (Multi-Protocol Label Switching): 패킷에 레이블을 붙여 경로를 효율적으로 제어하는 방식이다.
   - VPN (Virtual Private Network): 공용 네트워크 위에 사설 네트워크를 구현하여 안전한 데이터 전송을 가능하게 한다.
   - VLAN (Virtual LAN): 물리적 네트워크를 가상적으로 분리하여 효율성을 높이고 보안을 강화한다.

4. 링크 가상화의 장점

   - 유연성: 네트워크 자원을 효율적으로 분배할 수 있다.
   - 확장성: 새로운 네트워크 추가가 용이하다.
   - 보안성: 데이터 전송 경로를 분리하거나 암호화하여 보안을 강화한다.

5. 적용 사례

   - 기업 네트워크: 사내 네트워크를 가상화하여 부서별로 분리된 네트워크를 제공한다.
   - 데이터 센터: 클라우드 환경에서 물리적 네트워크를 추상화하여 자원을 최적화한다.

### MPLS

1. MPLS의 목표

   - MPLS의 주요 목표는 고정 길이 레이블과 가상 회선을 사용해 데이터그램을 빠르게 전달하는 것이다.&#x20;
   - IP 데이터그램 전달 방식을 완전히 대체하지 않고, 기존의 IP 인프라를 확장하여 필요할 때 레이블 기반 경로를 사용하도록 한다.
   - 간단히 말해, MPLS는 IP 주소 기반 라우팅과 레이블 기반 스위칭을 조화롭게 사용해 속도를 높인다.

   비유: IP 기반 라우팅은 일반적인 도로 경로이며, MPLS는 고속도로처럼 특정 목적지로 더 빠르게 가는 전용 경로를 제공한다.

2. MPLS 헤더

   - MPLS 헤더는 2계층 헤더와 3계층 헤더 사이에 삽입되는 작은 데이터 구조이다.
   - 주요 구성 요소:
     - 레이블: 패킷의 목적지를 빠르게 식별한다.
     - S 비트: 스택된 MPLS 헤더의 끝을 나타낸다.
     - TTL (Time to Live): 패킷이 네트워크를 얼마나 오래 통과할 수 있는지를 제한한다.
   - MPLS 헤더는 MPLS 가능 라우터 간에만 사용된다.

   비유: MPLS 헤더는 소포의 스티커와 같다. 스티커를 통해 택배가 최적의 경로로 빠르게 이동할 수 있다.

3. MPLS 동작 과정

   - MPLS 라우터는 MPLS 레이블을 기반으로 포워딩 테이블에서 경로를 찾는다.
   - 이 과정에서 IP 주소를 확인하지 않고, 레이블만으로 패킷을 전달한다. 따라서 MPLS 라우터는 "레이블 스위치 라우터"라고 불린다.

   비유: MPLS 라우터는 택배 기사처럼 소포의 주소를 읽지 않고 바코드(레이블)만 스캔하여 최적의 경로로 전달한다.

4. MPLS 네트워크의 상호 동작

   - 예시: R1에서 R4까지 MPLS 네트워크가 있는 경우
     - R1은 A 목적지로 가는 MPLS 레이블 "6"을 설정하고 R2와 R3에 전달한다.
     - R3은 목적지 A와 D를 위한 MPLS 레이블 "10"과 "12"를 R4에 전달한다.
     - 이 과정을 반복하여 R4는 각 목적지를 향한 최적의 경로를 가지게 된다.
   - MPLS 라우터는 IP 헤더를 수정하지 않고 레이블만 사용해 패킷을 처리한다.

5. MPLS의 장점

   - 레이블 기반 스위칭을 통해 스위칭 속도를 향상시킨다.
   - 트래픽 엔지니어링 가능: 표준 IP 라우팅이 제공하지 못하는 경로를 설계할 수 있다.
   - 복잡한 네트워크 환경에서도 효율적으로 동작한다.

   비유: 표준 IP 라우팅이 한 가지 경로만 제시한다면, MPLS는 여러 대체 경로와 전용 고속 경로를 제공하여 네트워크의 교통 혼잡을 줄인다.

### 추가 설명 및 쉬운 비유

1. 링크 가상화의 비유

   - 링크 가상화를 ‘도로’로 비유하면, 물리적 네트워크는 실제 도로이며, 링크 가상화는 그 도로 위에 그려진 차선과 같다.
   - 한 도로 위에 여러 차선이 존재하듯, 하나의 물리적 네트워크에서 여러 가상 네트워크를 구축할 수 있다.

2. MPLS의 비유

   - MPLS는 택배 회사의 ‘바코드 시스템’과 유사하다.
   - 각 패킷에 레이블을 붙여 목적지까지 가장 빠르고 효율적인 경로를 선택한다.

3. VPN의 비유

   - VPN은 ‘비밀 통로’와 같다.
   - 인터넷이라는 공용 공간 위에 사설 연결을 만들어 데이터를 안전하게 전달한다.

4. VLAN의 비유

   - VLAN은 ‘회사 건물 내의 가상 방’과 같다.
   - 한 건물(물리적 네트워크) 안에서 부서별로 방(가상 네트워크)을 나누어 사용하는 방식이다.

### 정리

링크 가상화는 현대 네트워크 기술에서 필수적인 요소이다.         
물리적 네트워크를 추상화하여 더 유연하고 확장 가능한 환경을 제공하며, 주요 기술로는 MPLS, VPN, VLAN 등이 있다.      
이를 통해 네트워크 관리자는 복잡성을 줄이고 자원 활용도를 높일 수 있다.

