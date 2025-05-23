AS(Autonomous System, 자율 시스템)은 하나의 네트워크 관리 주체가 운영하는 인터넷상의 네트워크 그룹이다.  

쉽게 말하면, 하나의 조직(예: 인터넷 서비스 제공업체, 대기업, 학교, 정부 기관 등)이 관리하는 네트워크 집합이라고 보면 된다.  

---

## 1. AS 개념 정리
- 하나의 AS는 동일한 라우팅 정책을 따르는 네트워크 그룹  
- 각 AS는 고유한 AS 번호(ASN, Autonomous System Number)를 가지고 있음  
- AS 간의 데이터 전송은 BGP(Border Gateway Protocol)을 사용하여 최적 경로를 찾음  

✔ 예제  
> 네이버(AS 23576), KT(AS 4766), SK브로드밴드(AS 9318), 구글(AS 15169) 등  
> 각 조직은 자신만의 네트워크를 관리하고, AS 번호를 가짐.

---

## 2. AS vs LAN 비교
| 구분 | AS(Autonomous System) | LAN(Local Area Network) |
|------|--------------------|------------------|
| 정의 | 하나의 조직(ISP, 기업, 학교 등)이 운영하는 인터넷 네트워크 그룹 | 건물, 사무실, 가정 내에서 사용되는 작은 네트워크 |
| 규모 | 대규모(전 세계적으로 연결된 네트워크) | 소규모(로컬 네트워크) |
| 프로토콜 | BGP 사용 (AS 간 연결) | Ethernet, Wi-Fi 등 사용 |
| 예시 | KT, SK브로드밴드, 네이버, 구글 | 회사 내부 네트워크, 학교 컴퓨터실 |

➡ AS는 인터넷을 구성하는 큰 단위의 네트워크, LAN은 사무실이나 가정에서 쓰는 작은 네트워크!  

즉, LAN은 AS의 일부가 될 수 있지만, AS는 ISP처럼 인터넷 상에서 여러 네트워크를 연결하고 운영하는 개념이다. 
