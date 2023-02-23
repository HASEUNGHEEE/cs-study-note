# OSI 모델과 TCP/IP 모델
## 등장배경

### 프로토콜(Protocol)

- 네트워크에서 데이터를 주고 받기 위해 네트워크를 구성하는 컴퓨터와 네트워크 장비들이 지켜야 할 규칙
- 지역마다 다른 언어가 존재하고, 생활영역마다 다른 법이 적용되는 것처럼, 네트워크 내에서도 어떤 기능을 하느냐에 따라 다양한 프로토콜들이 존재한다.
    - ex) 이더넷 프로토콜, IP 프로토콜, HTTP 프로토콜 등

### 네트워크 아키텍처

- 눈에 보이지 않는 데이터의 이동 과정을 세부적인 단계로 모듈화하여 단계마다 필요한 기능을 `프로토콜`로 정의하고, 각 단계를 데이터의 이동 순서에 따라 계층화시킨 것이 `네트워크 아키텍처`이다.
- **네트워크 아키텍처는 결국 프로토콜들의 집합!**
- 그런데, 컴퓨터와 네트워크 장비를 만드는 제조업체들이 저마다 독자적인 네트워크 아키텍처를 사용하여 서로 다른 제조업체의 제품 간에는 통신을 할 수 없었다.
- 제품들의 상호 연결성을 확보하기 위해 표준화를 시도한 대표적인 네트워크 아키텍처가 `OSI 모델` 과 `TCP/IP` 모델이다.

## OSI 모델과 TCP/IP 모델
<img width="1239" alt="OSI모델과 TCP:IP모델" src="https://user-images.githubusercontent.com/107941880/220607860-bab86c58-7edd-49f7-b970-0ec24d06d0e6.png">

- **`OSI 모델`**(Open Systems Interconnection Reference Model)은 국제표준화기구(ISO)에서 개발한 모델로, 컴퓨터 네트워크 프로토콜 디자인과 통신을 7개의 계층으로 분류한 모델입니다. 일반적으로 OSI 7 계층이라고 합니다.
  - 👉 [OSI 모델](https://github.com/HASEUNGHEEE/cs-study-note/blob/main/Network/OSI%207%20Layer.md)
- 현재는 네트워크상 데이터 전송 과정을 5개의 계층으로 단순화한 **`TCP/IP 모델`** 이 일반적으로 사용됩니다. 다양한 네트워크 기술이 TCP/IP를 중심으로 통합되어 사실상 인터넷 표준이 되었습니다.
  - 👉 [TCP/IP 모델](https://github.com/HASEUNGHEEE/cs-study-note/blob/main/Network/TCP,%20IP%20%EB%AA%A8%EB%8D%B8.md)
<details>
<summary>💡 TCP/IP 4계층? 5계층?</summary>
<div markdown="1">

TCP/IP는 원래 4개의 계층을 가진 모델이었지만 인터넷 개발 이후 네트워크 표준이 꾸준히 갱신되면서 하위 레이어가 다시 세분화되었습니다. TCP/IP 4계층의 Network Interface Layer가 Data Link Layer와 Physical Layer로 나뉘어져서 TCP/IP 5계층 모델이 되었습니다.

</div>
</details>

------------
### Reference
- [쉽게 이해하는 네트워크 5. 프로토콜과 네트워크 아키텍처 - OSI 모델과 TCP/IP 모델](https://better-together.tistory.com/65?category=887984)
