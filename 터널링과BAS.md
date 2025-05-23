<img width="868" alt="image" src="https://github.com/user-attachments/assets/0a3c3c23-6bf1-4138-85a7-a02fefe1d799" />

---

# 터널링(Tunneling)

터널링은 네트워크에서 서로 다른 프로토콜이나 네트워크 환경 간에 데이터를 전송하기 위해, 원래의 데이터를 다른 프로토콜로 캡슐화하여 중간 네트워크를 통해 전달하는 기술이다.
이를 통해 방화벽, NAT 등으로 인해 직접 연결이 어려운 환경에서도 안전하고 독립적인 가상 통신 경로를 만들 수 있다.

* **주요 목적**

  * 사설망처럼 안전하게 데이터를 전송
  * 통신 경로 우회 및 연결 가능
  * 서로 다른 네트워크 프로토콜 간 연결

* **주요 프로토콜 예시**

  * GRE, IPSec, L2TP, PPTP, SSH 터널링, PPPoE 등

* **작동 원리**

  1. 송신 측에서 원본 데이터를 새로운 프로토콜 헤더로 캡슐화
  2. 중간 네트워크를 통해 전달
  3. 수신 측에서 캡슐을 해제하여 원래 데이터 복원

---
터널링은 사용하는 프로토콜에 따라 네트워크의 다양한 계층(Layer)에서 이루어진다. 보통 다음과 같이 분류한다.

## 터널링의 주요 계층별 분류

| 계층                      | 설명                                 | 대표 터널링 프로토콜 예시                        |
| ----------------------- | ---------------------------------- | ------------------------------------- |
| **Layer 1 (물리계층)**      | 물리적인 신호 전송 방식의 터널링(예: 광섬유 다중화)     | 광섬유 다중화(WDM 등)                        |
| **Layer 2 (데이터 링크 계층)** | 프레임 단위로 캡슐화하는 터널링. MAC 주소 기반 연결 제공 | PPPoE, L2TP, PPTP, Ethernet over MPLS |
| **Layer 3 (네트워크 계층)**   | IP 패킷 단위로 캡슐화하는 터널링. 라우팅 가능        | GRE, IPSec, IP-in-IP                  |

---

## 참고

* **PPPoE**는 이더넷(Layer 2) 위에 PPP를 캡슐화한 터널링이다.
* **GRE, IPSec** 등은 IP 계층(Layer 3)에서 IP 패킷을 캡슐화한다.
* 터널링 프로토콜에 따라 캡슐화 대상과 터널의 범위가 달라진다.

즉, 터널링은 특정 계층에 국한되지 않고, 상황에 따라 2계층 또는 3계층 등 여러 계층에서 수행될 수 있다.


# BAS (Broadband Access Server)

BAS는 ISP에서 사용자의 인터넷 접속을 관리하는 장비로, 가입자 인증, 세션 관리, IP 주소 할당, 트래픽 제어, 과금 정보 수집 등을 담당한다.
주로 ADSL, VDSL, FTTH 환경에서 PPPoE 세션을 처리하며, RADIUS 서버와 연동해 사용자 인증을 수행한다.

* **주요 역할**

  * PPPoE 세션 수립 및 관리
  * 사용자 인증 및 IP 주소 할당
  * 트래픽 정책 적용 및 QoS 관리
  * 과금 정보 수집 및 연동

* **터널링과의 관계**
  BAS는 PPPoE 세션이라는 논리적 터널의 종단점 역할을 수행하며, VPN 등 다른 터널링 트래픽을 통과시키거나 제어할 수 있다.
  직접 터널링 프로토콜을 생성하기보다는 터널링 세션을 관리하는 기능에 집중한다.

