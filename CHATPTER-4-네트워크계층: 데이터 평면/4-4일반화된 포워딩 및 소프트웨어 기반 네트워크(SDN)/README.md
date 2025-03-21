### 4.4 일반화된 포워딩 및 소프트웨어 기반 네트워크(SDN)

---

### 1. 일반화된 포워딩 개념
일반화된 포워딩은 패킷의 여러 헤더 필드를 기반으로 매치(Match)한 후, 다양한 액션(Action)을 수행하는 방식이다.  
기존 네트워크에서 목적지 IP 주소만 매칭하는 방식보다 유연하며, 다양한 프로토콜과 네트워크 작업을 지원한다.

#### 매치와 액션의 개념
1. **매치**:
   - 패킷의 헤더 필드를 확인하여 규칙에 일치하는지 검사한다.
   - 예: IP 주소, 포트 번호, MAC 주소 등.
2. **액션**:
   - 매치된 규칙에 따라 패킷을 처리한다.
   - 예:
     - 패킷을 출력 포트로 전달.
     - 헤더 필드를 수정.
     - 패킷을 삭제하거나 로드 밸런싱.
     - 특수 서버로 패킷 전달.

---

### 2. 소프트웨어 기반 네트워크와 일반화된 포워딩
소프트웨어 기반 네트워크는 네트워크의 제어 평면을 데이터 평면과 분리하여 네트워크를 소프트웨어로 관리하고 제어한다.  
각 패킷 스위치는 원격 컨트롤러에서 계산 및 배포받은 매치 플러스 액션 테이블을 기반으로 작동한다.

#### OpenFlow 1.0의 도입
OpenFlow는 매치 플러스 액션 포워딩 테이블을 통해 이 개념을 실현한다.  

플로우 테이블의 구성:
1. **매치 규칙**:
   - 패킷 헤더의 특정 필드와 매칭한다.
   - 예: IP 주소, 포트 번호, MAC 주소 등.
2. **카운터**:
   - 매치된 패킷 수와 바이트 수를 기록한다.
3. **액션**:
   - 패킷을 출력 포트로 전달하거나 삭제, 복사, 헤더 수정 등 다양한 작업을 수행한다.

---

### 3. 매치 플러스 액션 테이블
- **매치 가능한 필드**:
  - IP 주소, MAC 주소, 포트 번호 등 다양한 헤더 필드.
  - 와일드카드 매치 지원(예: `128.119.*.*`는 128.119로 시작하는 IP 주소와 매칭).
- **우선순위**:
  - 플로우 테이블 엔트리마다 우선순위를 설정하여, 여러 규칙과 매칭되더라도 가장 높은 우선순위의 규칙이 적용된다.
- **제한 사항**:
  - 모든 필드가 매치 가능한 것은 아니며, TTL 필드나 데이터그램 길이 필드에는 매치를 허용하지 않는다.

---

### 4. 일반화된 포워딩의 장점
1. **유연성**:
   - 다양한 프로토콜과 네트워크 작업에 대응 가능하다.
2. **중앙 관리**:
   - 컨트롤러에서 플로우 테이블을 중앙에서 관리하고 배포한다.
3. **고급 네트워크 제어**:
   - 로드 밸런싱, QoS, 보안 정책 적용이 가능하다.
4. **효율성**:
   - 정교한 매치와 액션을 통해 네트워크 리소스를 효율적으로 사용한다.

---

### 5. 요약
일반화된 포워딩은 매치 플러스 액션 테이블을 기반으로 패킷을 처리하며, 기존의 단순한 목적지 기반 포워딩보다 유연한 네트워크 제어를 제공한다.       
OpenFlow는 이를 구현한 대표적인 프로토콜로, 플로우 테이블에서 패킷을 매칭하고 다양한 액션을 수행할 수 있도록 지원한다. 이를 통해 네트워크 관리를 중앙화하고 효율적으로 처리할 수 있다.   

### 4.4.1 매치

매치는 플로우 테이블에서 **패킷의 헤더 필드와 조건을 비교하여 일치 여부를 확인**하는 작업이다.  
OpenFlow 1.0 매치 플러스 액션 규칙은 매치 가능한 11개의 패킷 헤더 필드와 수신 포트 ID를 포함한다.

#### 매치의 주요 특징
1. **진입 포트**:
   - 패킷이 수신된 입력 포트를 나타낸다.
2. **와일드카드 지원**:
   - 특정 비트를 일치시키는 와일드카드를 사용할 수 있다.
   - 예: `128.119.*.*`는 주소의 처음 16비트(128.119)와 일치하는 모든 데이터그램과 매치.
3. **우선순위**:
   - 플로우 테이블 엔트리에 우선순위를 지정.
   - 여러 엔트리와 매치되더라도 **가장 높은 우선순위의 규칙**이 적용된다.
4. **제한 사항**:
   - OpenFlow는 TTL 필드나 데이터그램 길이 필드에 기반한 매치를 지원하지 않는다.

---

### 4.4.2 액션

매치된 패킷은 플로우 테이블에 정의된 **액션 목록**에 따라 처리된다.  
액션은 지정된 순서대로 수행되며, 주요 작업은 다음과 같다.

#### 주요 액션
1. **포워딩**:
   - 패킷을 특정 출력 포트로 전달.
   - 모든 포트로 브로드캐스트하거나 선택된 포트로 멀티캐스트.
   - 패킷을 캡슐화하여 원격 컨트롤러로 전송.
   - 컨트롤러가 패킷을 처리한 후 새로운 플로우 테이블 규칙을 적용해 패킷을 반환할 수도 있다.
2. **삭제**:
   - 아무 액션이 없는 플로우 테이블 엔트리는 매치된 패킷을 삭제.
3. **필드 수정**:
   - 패킷이 출력 포트로 전달되기 전에 헤더 필드 값을 다시 쓴다.

---

### 4.4.3 매치 플러스 액션 작업의 OpenFlow 예

#### 첫 번째 예: 간단한 포워딩
패킷이 호스트 h5 또는 h6에서 출발해 호스트 h3 또는 h4로 전달된다고 가정한다.  
패킷은 아래와 같은 순서로 포워딩된다:

1. **스위치 s3**:
   - h5 또는 h6에서 들어온 데이터그램은 s1으로 전달.
   - 플로우 테이블 엔트리:
     ```
     입력 포트: h5/h6 → 출력 포트: s1
     ```
2. **스위치 s1**:
   - s1에 도착한 데이터그램은 s2로 전달.
   - 플로우 테이블 엔트리:
     ```
     입력 포트: s3 → 출력 포트: s2
     ```
3. **스위치 s2**:
   - s2에 도착한 데이터그램은 h3 또는 h4로 전달.
   - 플로우 테이블 엔트리:
     ```
     입력 포트: s1 → 출력 포트: h3/h4
     ```

---

#### 두 번째 예: 로드 밸런싱
h3에서 출발한 데이터그램은 s2와 s1 사이의 링크를 통해,  
h4에서 출발한 데이터그램은 s2와 s3 사이의 링크를 통해 전달된다고 가정한다.  

1. **스위치 s2**:
   - h3의 데이터그램은 s1으로, h4의 데이터그램은 s3으로 전달.
   - 플로우 테이블 엔트리:
     ```
     입력 포트: h3 → 출력 포트: s1
     입력 포트: h4 → 출력 포트: s3
     ```

---

#### 세 번째 예: 방화벽
s2는 s3에 연결된 호스트에서 보내는 트래픽만 수신한다고 가정한다.  
s2의 플로우 테이블 엔트리는 다음과 같다:

- 플로우 테이블 엔트리:
  ```
  매치 조건: 출발지 IP 주소 10.3.*.*
  액션: 출력 포트 → s2의 연결된 호스트
  ```

위 설정으로 인해 10.3.*.*의 트래픽만 전달되며, 다른 트래픽은 차단된다.

---

### 요약
매치 플러스 액션은 OpenFlow에서 데이터그램의 헤더 필드와 매치 조건을 기반으로 **라우팅 및 데이터 조작**을 수행하는 방식이다.  
- **매치**는 패킷의 헤더 필드와 규칙의 조건을 비교.
- **액션**은 매치된 패킷에 대해 포워딩, 삭제, 필드 수정 등의 작업을 수행.

OpenFlow는 이를 통해 간단한 포워딩, 로드 밸런싱, 방화벽과 같은 다양한 네트워크 동작을 구현할 수 있다.


### **OpenFlow에서 TTL 필드와 데이터그램 길이 필드를 매치하지 않는 이유**

---

#### 1. **TTL 필드**
- **TTL(Time-to-Live)** 필드는 패킷이 네트워크를 통과할 때 라우터에서 1씩 감소하며, 무한 루프를 방지하는 역할을 한다.
- **매치에서 제외하는 이유**:
  1. **동적 변화**:
     - TTL 값은 라우터를 지날 때마다 변경되므로, 동일한 패킷이 네트워크 내에서 서로 다른 TTL 값을 가질 수 있다.
     - 결과적으로 TTL 필드를 매치 조건으로 사용하면 일관된 규칙 적용이 어려워진다.
  2. **주요 매치 조건이 아님**:
     - TTL은 네트워크 루프 방지와 관련된 필드로, 트래픽 분류나 정책 적용에 크게 기여하지 않는다.

---

#### 2. **데이터그램 길이 필드**
- **데이터그램 길이**는 패킷의 전체 길이를 바이트 단위로 나타내는 필드이다.
- **매치에서 제외하는 이유**:
  1. **비효율성**:
     - 데이터그램 길이는 트래픽 필터링이나 라우팅에 일반적으로 사용되지 않으며, 이를 기반으로 매치를 수행하면 불필요한 계산 비용이 발생한다.
  2. **변동 가능성**:
     - 데이터그램 길이는 단편화 및 재조합 과정에서 변경될 수 있어 매치 조건으로 적합하지 않다.
     - 예: 패킷이 네트워크를 통과하며 단편화되면 각 단편의 길이가 원본 데이터그램의 길이와 다르게 된다.

---
### **미들박스와 매치 플러스 액션의 관계**

미들박스와 매치 플러스 액션은 개념적으로 유사한 작업을 수행하지만, **정확히 동일한 기술**은 아니다.  
두 기술은 패킷을 처리하고 조작한다는 점에서 공통점을 가지지만, 동작 방식과 설계 철학에서 차이가 있다.

---

### **1. 미들박스**
- **역할**:
  - 네트워크의 특정 위치에서 **트래픽을 처리하거나 변환**하는 장치.
  - 예: NAT, 방화벽, 로드 밸런서 등.
- **주요 특징**:
  1. **계층 간 데이터를 조작**:
     - 네트워크 계층(IP), 트랜스포트 계층(TCP/UDP), 애플리케이션 계층(HTTP 등)을 모두 처리.
     - 예: NAT는 IP 주소와 포트 번호를 변환, 방화벽은 특정 트래픽을 차단.
  2. **분산형 아키텍처**:
     - 미들박스는 특정 네트워크 장치에 설치되며, **개별 장치가 독립적으로 동작**.
  3. **정적 규칙 기반**:
     - 대부분의 미들박스는 사전에 정의된 규칙을 사용하여 패킷을 처리.

---

### **2. 매치 플러스 액션**
- **역할**:
  - 소프트웨어 기반 네트워크에서 패킷 헤더를 기반으로 매치(Match)하고, **정의된 액션(Action)을 수행**.
  - 예: OpenFlow 플로우 테이블.
- **주요 특징**:
  1. **매치 기반 처리**:
     - 패킷 헤더(IP 주소, 포트 번호, MAC 주소 등)를 기준으로 매치.
     - 여러 조건을 조합하여 복잡한 정책 구현 가능.
  2. **중앙 집중식 제어**:
     - 원격 컨트롤러(SDN 컨트롤러)에서 정책을 설정하고, 이를 각 장치로 배포.
     - 네트워크의 전반적인 제어 및 관리를 중앙에서 수행.
  3. **동적 정책 변경**:
     - 네트워크 상태에 따라 실시간으로 플로우 테이블 규칙을 변경 가능.

---

### **3. 공통점**
1. **패킷 처리 및 조작**:
   - 둘 다 패킷을 처리하며, 특정 규칙에 따라 포워딩, 수정, 삭제 등의 작업을 수행.
2. **트래픽 관리**:
   - 네트워크 트래픽을 효율적으로 제어하고 보안을 강화.

---

### **4. 차이점**

| **구분**         | **미들박스**                                   | **매치 플러스 액션**                             |
|-------------------|-----------------------------------------------|-----------------------------------------------|
| **제어 방식**     | 분산 제어 (장치별 독립 작동)                  | 중앙 집중식 제어 (SDN 컨트롤러 사용)          |
| **정책 변경**     | 사전에 정의된 정적 규칙                       | 실시간 동적 규칙 변경 가능                     |
| **계층 처리**     | 네트워크, 트랜스포트, 애플리케이션 계층 조작  | 주로 네트워크 및 트랜스포트 계층 헤더 처리     |
| **설계 철학**     | 특정 기능(NAT, 방화벽 등)을 위한 특화 장치    | 범용 네트워크 제어를 위한 유연한 처리 방식    |

---

### **결론**
미들박스는 특정 네트워크 기능을 수행하는 **전통적인 네트워크 장치**이고, 매치 플러스 액션은 SDN 환경에서 **중앙 집중식 제어를 통해 동적으로 네트워크를 제어**하는 방식이다.  
미들박스는 개별적으로 작동하며 특정 작업에 최적화된 반면, 매치 플러스 액션은 네트워크의 전반적인 유연성과 확장성을 강화하기 위해 설계되었다. 

따라서, 미들박스는 매치 플러스 액션의 **특정 사례**로 볼 수 있으나, 둘을 동일하게 간주하기에는 설계와 활용 목적에서 차이가 있다.
