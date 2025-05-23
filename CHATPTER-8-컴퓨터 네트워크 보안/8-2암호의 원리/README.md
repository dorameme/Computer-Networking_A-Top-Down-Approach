## **8.2 암호의 원리**

송신자가 보내는 원래 형태의 메시지를 평문 또는 원문이라고 한다.        
송신자는 평문을 암호화 알고리즘을 사용해서 암호화하며, 암호화된 메시지인 암호문은 다른 침입자가 해석할 수 없다.       
암호화 알고리즘은 누구에게나 알려져 있고 쉽게 사용할 수 있다.              
그러나 전송된 데이터를 침입자가 복원할 수 없게 하는 비밀 정보인 키가 필요하다.

---

### 시나리오

**대칭키 시스템**

- 앨리스와 밥은 동일한 키를 사용하며, 이 키는 둘만의 비밀이다.

**공개키 시스템**

- 공개키 암호화에서는 송수신자가 각각 키를 갖는다기보다 수신자가 2개의 키를 갖는다.
- 하나는 세상 모두에게 알려진 공개키이고, 다른 하나는 수신자만 아는 개인키이다.


---

### 8.2.1 대칭키 암호화

### 1. 카이사르 암호

- 영어 원문에서 각 철자를 알파벳 순서로 k번째 뒤에 오는 철자로 대치한다.
- 여기서 k가 암호화 키가 된다.
- 단, 암호화 방식이 단순하여 쉽게 복호화될 수 있다.

**예시**:

- 평문: HELLO
- k=3이라면, H는 K, E는 H, L은 O로 변환된다.
- 결과 암호문: KHOOR

### 2. 단일 문자 대응 암호

- 철자를 일정한 규칙 없이 고유한 대응 문자로 변환한다.
- 가능한 문자 조합은 26!이지만, 자주 사용되는 문자(e, t 등)로 인해 해독 가능성이 높다.

**예시**:

- 평문: HELLO
- 대응표: H -> Q, E -> W, L -> R, O -> T
- 결과 암호문: QWRRT

### 3. 다중 문자 대응 암호화

- 여러 단일 문자 대응법을 조합하여 위치에 따라 다른 암호법을 사용한다.
- 예를 들어, 첫 번째 문자는 C1, 두 번째 문자는 C2 방식으로 암호화한다.

**예시**:

- 평문: HELLO
- 각 문자를 다른 대응표를 사용해 암호화한다. 첫 번째 문자는 C1, 두 번째 문자는 C2로 처리된다.
- 예를 들어, 대응표 C1에서는 H -> X, 첫 번째 L -> F로 대치되고, 대응표 C2에서는 E -> G, 두 번째 L -> P, O -> M으로 대치된다.
- 대응표는 미리 송신자와 수신자가 공유한 규칙에 따라 생성되며, 특정 문자가 어떤 문자로 변환되는지 정해진다.
- 결과 암호문: XGFPM (H -> X, E -> G, 첫 번째 L -> F, 두 번째 L -> P, O -> M)

### 4. 블록 암호화

- 메시지를 k 비트 단위로 나누어 암호화한다.
- AES, DES, 3DES 등이 대표적이다.
- 블록 암호화 과정에서 순열 테이블을 사용하여 입력과 출력을 매칭한다.

**예시**:

- 평문: 01101001 (8비트 블록)
- 암호화 과정:
  1. 평문 블록(01101001)을 순열 테이블에 입력한다.
  2. 테이블에 정의된 대로 비트를 재배치한다.
  3. 결과 출력 블록: 11001100
- 결과 암호문: 11001100

**비유**:
블록 암호화는 "레고 블록을 조립하는 것"과 비슷하다. 각 블록을 조립하기 전에 블록의 모양(비트 배열)을 바꾸는 과정을 거치며, 이 과정은 비밀스러운 순열 테이블에 따라 이루어진다.

### 5. 암호 블록 체이닝(CBC)

- 동일한 평문 블록이 동일한 암호문 블록을 생성하는 문제를 해결하기 위해 초기화 벡터(IV)를 사용한다.
- 각 블록 암호화는 이전 블록 암호문의 결과에 의존하여 이루어진다.

**비교 표**:

| **항목**          | **블록 암호화**               | **암호 블록 체이닝 (CBC)**              |
| --------------- | ------------------------ | -------------------------------- |
| **기본 원리**       | 각 블록을 독립적으로 암호화          | 이전 블록 암호문 결과를 현재 블록 암호화에 사용      |
| **패턴 제거 여부**    | 동일한 평문 블록은 동일한 암호문 블록 생성 | 동일한 평문 블록이라도 다른 암호문 블록 생성 가능     |
| **초기화 벡터 (IV)** | 필요하지 않음                  | 필요 (첫 번째 블록 암호화에 사용)             |
| **보안 수준**       | 평문 패턴이 암호문에 남을 가능성 있음    | 평문 패턴 제거 가능                      |
| **적용 예시**       | DES, AES 등               | TLS, IPsec, HTTPS 등              |
| **암호화 계산 방식**   | 각 블록을 고정된 키와 독립적으로 암호화   | m(i)와 이전 암호문 c(i-1)의 XOR 결과를 암호화 |

**예시**:

- 평문 블록: M1 = 0110, M2 = 1001
- 초기화 벡터(IV): 1010
- 첫 번째 암호문 블록: C1 = K(M1 XOR IV)
- 두 번째 암호문 블록: C2 = K(M2 XOR C1)

---

### 8.2.2 공개키 암호화

### 1. 공개키 암호화

- 공개키 암호화에서는 수신자가 두 개의 키를 가진다.
  - 하나는 세상 모두에게 공개된 공개키.
  - 다른 하나는 수신자만 알고 있는 개인키.

**시나리오**:

1. 송신자는 수신자의 공개키를 이용해 메시지를 암호화한다.
2. 암호문을 수신자에게 보낸다.
3. 수신자는 자신의 개인키를 이용해 암호문을 복호화한다.

---

### 2. RSA 암호화

#### RSA의 기본 개념
RSA는 모듈로 n 연산(나머지 계산)을 기반으로 동작

#### 공개키와 개인키 생성

1. 두 개의 큰 소수 p와 q를 선택한다.
2. n = p × q와 z = (p-1)(q-1)를 계산한다.
3. z와 서로소인 e를 선택한다.
4. ed mod z = 1을 만족하는 d를 계산한다.
5. 공개키는 (n, e), 개인키는 (n, d)이다.

**예시**:

- p = 5, q = 7이라고 하자.
- n = 5 × 7 = 35, z = (5-1)(7-1) = 24
- z와 서로소인 e = 5를 선택한다.
- ed mod z = 1을 만족하는 d = 29를 계산한다.
- 공개키는 (35, 5), 개인키는 (35, 29)이다.

#### RSA 암호화 및 복호화

1. 송신자는 공개키 (n, e)를 사용해 메시지를 암호화한다:
2. 수신자는 개인키 (n, d)를 사용해 복호화한다:


### 3. 세션키와 RSA의 결합

RSA는 계산 비용이 높으므로, 대칭키 암호화 방식과 함께 사용된다.

**시나리오**:

1. 송신자는 대칭키 암호화에 사용할 세션키를 선택한다.
2. 세션키를 수신자의 공개키로 암호화한다.
3. 수신자는 자신의 개인키로 복호화해 세션키를 얻는다.
4. 이후 데이터는 대칭키 암호화를 통해 전송된다.

---


| **구분**               | **대칭키**                                                            | **공개키**                                                            | **개인키**                                                                 |
|------------------------|-------------------------------------------------------------------|-------------------------------------------------------------------|-------------------------------------------------------------------------|
| **기본 원리**          | 송신자와 수신자가 동일한 키를 사용해 데이터를 암호화 및 복호화                     | 데이터를 암호화하기 위해 공개된 키를 사용                             | 공개키로 암호화된 데이터를 복호화하기 위해 사용되는 비밀 키                                                |
| **키 개수**             | 하나의 키를 사용 (송신자와 수신자 동일)                                           | 공개된 키 (누구나 접근 가능)                                           | 수신자만 알고 있는 비밀 키                                                   |
| **보안 방법**           | 키를 송신자와 수신자가 안전하게 공유해야 함                                       | 공개키는 자유롭게 배포 가능하나, 개인키는 엄격히 보호해야 함                     | 개인키는 절대 외부에 노출되지 않아야 함                                              |
| **장점**               | 속도가 빠르고 계산 비용이 낮음                                                | 키 분배 문제 해결 (송신자와 수신자가 키를 사전에 공유하지 않아도 됨)              | 공개키와의 조합으로 강력한 보안성 제공                                              |
| **단점**               | 키 분배 및 관리가 어렵고, 동일 키 사용으로 보안성 약화 가능                       | 계산 비용이 높아 대량 데이터 암호화에는 부적합                                | 분실 시 데이터 복호화 불가능                                                   |
| **적용 예시**           | AES, DES, 3DES 등                                                  | RSA, ECC 등                                                        | RSA의 개인키, HTTPS 연결 시 비공개 키                                             |
| **사용 사례**           | 파일 암호화, 데이터 전송 시 사용 (대용량 데이터 처리에 적합)                     | 대칭키 전송, 메시지 서명, 인증서 발급 등                                | 암호문 복호화, 메시지 서명 검증 등에 사용                                            |

### **요약**
- **대칭키**: 암호화와 복호화에 동일한 키를 사용하며, 속도가 빠르지만 키를 안전하게 공유해야 하는 부담이 있다.
- **공개키**: 누구나 사용할 수 있도록 공개된 키로 데이터를 암호화한다.
- **개인키**: 공개키로 암호화된 데이터를 복호화하거나, 디지털 서명을 생성하는 데 사용된다.

