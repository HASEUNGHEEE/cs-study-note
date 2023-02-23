# 브라우저의 주소창에 [https://www.example.com](https://www.google.com) URL을 입력하고 엔터를 누르면 발생하는 일
<img width="1511" alt="url 입력하고 일어나는 일" src="https://user-images.githubusercontent.com/107941880/220913194-eecb5a94-ae25-40bb-a589-ef799c8179c6.png">


## 1. 브라우저의 주소창에 URL을 입력하고 Enter 키를 입력합니다.

먼저, `https://www.example.com` 라는 URL을 분류해보겠습니다.

### 1.1 통신 규약(Protocol)

- `https://` 는 통신 프로토콜입니다.
- 이 스키마는 브라우저에 전송 계층 보안 프로토콜인 TLS(Transfer Layer Secure)를 사용하여 서버에 연결하도록 지시합니다.
- HTTPS는 Hypertext Transfer Protocol Secure를 나타냅니다.

### 1.2 도메인 (Domain)

- `www.exmple.com` 은 도메인 이름입니다.

## 2. 브라우저가 캐시에서 DNS 기록을 확인하여 URL의 IP 주소를 찾습니다.

### 2.1 DNS(Domain Name System)

- 웹사이트의 IP 주소와 도메인 주소를 연결해주는 시스템입니다.
- 인터넷의 모든 URL에는 고유한 IP 주소가 할당되어 있습니다.
- 각 웹사이트의 IP 주소를 모두 외워 입력하는 것은 비효율적이므로 DNS가 등장하였습니다.

### 2.2 캐시(Cache)

- DNS는 복잡하고 매우 빨라야 하기 때문에 DNS 데이터는 웹 브라우저 사이의 서로 다른 계층과 인터넷의 다양한 위치에 임시로 저장됩니다. 이를 캐시(Cache)라고 합니다.
- 웹 브라우저는 고유한 캐시, 운영체제(OS) 캐시, 라우터의 로컬 네트워크 캐시, 인터넷 서비스 제공업체(ISP)의 DNS 서버 캐시를 확인합니다.
- 먼저, `브라우저 자체의 캐시`를 확인합니다.
    - 브라우저는 이전에 방문한 웹사이트의 DNS 기록을 일정 기간 동안 저장하고 있기 때문에 가장 먼저 DNS 쿼리를 실행합니다.
- 둘째, `OS 캐시`를 확인합니다.
    - OS도 DNS 기록 캐시를 저장하고 있습니다.
    - 따라서 브라우저 캐시에 원하는 DNS 기록이 없다면, 브라우저는 컴퓨터 OS에 시스템 호출을 수행하여 DNS 기록을 가져옵니다.
- 셋째, `라우터 캐시`를 확인합니다.
    - 만약 컴퓨터에도 원하는 DNS 기록이 없다면, 브라우저는 자체 DNS 기록 캐시를 저장하고 있는 라우터와 통신하여 라우터 캐시를 확인합니다.
- 넷째, `ISP 캐시`를 확인합니다.
    - 만약 위의 모든 단계에서 DNS 기록을 찾지 못한다면, 브라우저는 ISP(Internet Service Provider)가 가지고 있는 자체 DNS 서버를 확인하여 DNS 기록 캐시를 찾습니다.

## 3. 요청한 URL이 캐시에 없는 경우, ISP의 DNS 서버가 DNS 쿼리로 해당 URL을 호스팅하는 서버의 IP 주소를 찾습니다.

### 3.1 DNS 쿼리(query)
<img width="944" alt="DNS 쿼리" src="https://user-images.githubusercontent.com/107941880/220913337-c50f4991-b863-435f-9f05-3195c0dfe55d.png">


- 앞서 언급했듯이 내 컴퓨터가 `www.example.com` 을 호스팅하는 서버에 연결하려면 `www.example.com의` IP 주소가 필요합니다.
- **DNS 쿼리의 목적**은 웹사이트에 대한 올바른 IP 주소를 찾을 때까지 인터넷의 여러 DNS 서버를 검색하는 것입니다. 이러한 유형의 검색은 필요한 IP 주소를 찾거나 찾을 수 없다는 오류 응답을 반환할 때까지 DNS 서버에서 DNS 서버로 검색을 반복적으로 계속하기 때문에 **재귀적 질의(Recursive Query)** 라고 합니다.
- **DNS Resolver(DNS Recursor)**
    - ISP의 DNS 서버를 **DNS 리졸버(또는 DNS 리커서)** 라고 부르는데, 인터넷의 다른 DNS 서버에 응답을 요청하여 의도한 도메인 이름의 적절한 IP 주소를 찾아내는 역할을 합니다.
    - 클라이언트와 DNS 네임서버 사이의 중개자 역할을 합니다.
    - 이 과정 중 DNS 리졸버는 권한 있는 네임서버에서 받은 정보를 캐시합니다. 이에 따라, 만약 클라이언트가 다른 클라이언트가 최근에 요청한 도메인 이름의 IP 주소를 요청한다면, 네임서버와의 통신 프로세스를 우회하고 캐시되어 있는 DNS 기록을 클라이언트에 전달합니다.
- **Root Name Server**
    - 사람이 알기 쉬운 도메인 이름을 컴퓨터가 알기 쉬운 IP 주소로 변환하는 작업을 수행합니다.
    - 루트 서버는 리졸버의 쿼리를 수락하고, 쿼리에 표시된 도메인 이름에 따라 다음 단계의 TLD 네임 서버로 쿼리를 리디렉션합니다.
- **Top Level Domain(TLD) Name Server: `.com` 네임 서버**
    - TLD 네임서버는 도메인 네임에 대한 정보를 유지 관리할 책임이 있습니다.
    - 예를 들어 `.com` 또는 `.org`로 끝나는 웹사이트 또는 `국가 수준 도메인`에 대한 정보를 포함할 수 있습니다.
    - TLD 네임 서버는 루트 서버에서 쿼리를 가져와 쿼리의 특정 도메인과 연결된 권한 있는 DNS 네임 서버로 리디렉션합니다.
- **Authoritative Name Server: `example.com` 네임 서버**
    - DNS 기록에서 `www.example.com`와 일치하는 IP 주소를 찾아서 DNS 리졸버에게 반환하고, 리졸버는 이를 다시 브라우저로 전송합니다.

## 4. 브라우저와 서버가 TCP 연결을 시작합니다.
<img width="896" alt="handshake" src="https://user-images.githubusercontent.com/107941880/220913392-3fef03c9-762f-4282-ac52-ab4ce5922932.png">

### 4.1 TCP `3-way handshake`

브라우저가 올바른 IP 주소를 받으면 해당 IP 주소와 일치하는 서버와 **TCP 연결을 설정**하여 정보를 전송합니다. TCP의 `3-way handshake` 는 클라이언트와 서버가 SYN(동기화) 및 ACK(확인) 메시지를 교환하여 연결을 설정하는 3단계 프로세스입니다.

> 🔍 **동작 과정**

1. 클라이언트는 인터넷을 통해 서버에 `SYN` 패킷을 전송하여 새 연결이 가능한지 묻습니다.
2. 서버에 새 연결을 수락할 수 있는 열린 포트가 있으면, `SYN/ACK` 패킷을 사용하여 SYN 패킷에 대한 ACK(승인)으로 응답합니다.
3. 클라이언트는 서버로부터 SYN/ACK 패킷을 수신하고 `ACK` 패킷을 전송하여 이를 승인합니다.

그러면 데이터 전송을 위한 TCP 연결이 설정됩니다.

👉 [TCP의 통신 과정](https://github.com/HASEUNGHEEE/cs-study-note/blob/main/Network/TCP%2C%20IP%20%EB%AA%A8%EB%8D%B8.md)

### 4.2 HTTPS `TLS handshake`

TLS(Transport Layer Security)는 웹 브라우저와 서버 간의 통신을 암호화하기 위해 설계된 프로토콜입니다. 브라우저와 서버는 연결을 보호하기 위해 `TLS handshake`를 시작합니다.

> 🔍 **동작 과정**

#### 1. ClientHello

- TCP 연결을 통해 클라이언트는 다음 정보를 `ClientHello 메시지`에 담아 서버로 보냅니다.
    - 브라우저가 사용하는 SSL 혹은 TLS 버전 정보
    - 브라우저가 지원하는 암호화 방식 모음(cipher suite)
    - 브라우저가 순간적으로 생성한 임의의 난수(숫자)
    - 만약 이전에 SSL handshake가 완료된 상태라면, 그 때 생성된 세션 아이디(Session ID)
    - 기타 확장 정보(extension)

#### 2. ServerHello

- 서버는 클라이언트에게 다음 정보를 `ServerHello 메시지`에 담아 답장합니다.
    - 브라우저의 암호화 방식 정보 중에서, 서버가 지원하고 선택한 암호화 방식(cipher suite)
    - 서버의 공개키가 담긴 SSL 인증서
        - 인증서는 CA의 비밀키로 암호화되어 발급된 상태입니다.
        - 이 인증서는 대칭키가 생성되기 전까지 클라이언트가 나머지 handshake 과정을 암호화하는 데에 쓸 공개키를 담고 있습니다.
        - cf) 개인 키는 서버가 소유
    - 서버가 순간적으로 생성한 임의의 난수(숫자)
    - 클라이언트 인증서 요청(선택 사항)

#### 3. Certificate

- 클라이언트는 서버가 보낸 CA(Certificate Authority, 인증 기관)의 개인 키로 암호화된 이 SSL 인증서를 이미 모두에게 공개된 CA의 공개키를 사용하여 복호화합니다.
- 복호화에 성공하면 이 인증서는 CA가 서명한 것이 맞으니 진짜임이 증명되는 것입니다.
- 또한 클라이언트는 데이터 암호화에 사용할 대칭키(비밀키)를 생성한 후 SSL 인증서 내부에 들어 있던(서버가 발행한) 공개키를 이용해 암호화하여 서버에게 전송할 것입니다.

#### 4. Server Key Exchange / ServerHello Done

- Server Key Exchange는 서버의 공개키가 SSL 인증서 내부에 없는 경우, 서버가 직접 전달하는 것을 의미합니다.
- 만약 공개키가 SSL 인증서 내부에 있다면 Server Key Exchange는 생략됩니다.
- 인증서 내부에 공개키가 있다면 클라이언트가 CA의 공개키를 통해 인증서를 복호화한 후 서버의 공개키를 확보할 수 있습니다.

#### 5. Client Key Exchange

- 대칭키(비밀키, 데이터를 실제로 암호화하는 키)를 클라이언트가 생성하여 SSL 인증서 내부에서 추출한 서버의 공개키를 이용해 암호화한 후 서버에게 전달합니다.
- 여기서 전달된 `대칭키`가 바로 이 handshake의 목적이자 가장 중요한 수단인 데이터를 실제로 암호화할 대칭키(비밀키)입니다.
- 이제 키를 통해 클라이언트와 서버가 교환하고자 하는 데이터를 암호화합니다.

#### 6. ChangeCipherSpec / Finished

- 클라이언트는 Finished 메시지를 서버에 보내면서, 지금까지의 교환 내역을 해시한 값을 대칭키로 암호화하여 담습니다.
- 서버는 스스로도 해시를 생성해 클라이언트에서 도착한 값과 일치하는 지 확인합니다.
- 일치하면, 서버도 마찬가지로 대칭키를 통해 암호화한 Finished 메시지를 클라이언트에 보냅니다.
- 이로써 서버/클라이언트는 TSL handshake를 종료하고, HTTPS 통신을 시작합니다.

👉 [HTTP/HTTPS]()

## 5. 브라우저는 웹서버에 HTTP 요청을 보냅니다.

- TLS handshake가 완료되면 브라우저는 웹 서버에 HTTPS 요청을 보냅니다.
- HTTPS 요청은 기본적으로 HTTP 요청과 동일하지만 TLS handshake 중에 설정된 세션 키를 사용하여 암호화됩니다.
- HTTP 요청에는 요청 라인, 헤더(또는 요청에 대한 메타데이터) 및 본문이 포함됩니다. 요청 라인에는 클라이언트가 수행하려는 작업을 서버가 결정하는 데 사용할 수 있는 정보가 포함되어 있습니다.
    - GET, POST, PUT, PATCH, DELETE 등의 요청 메서드
    - 요청된 리소스를 가리키는 경로
    - 통신할 HTTP 버전
- 브라우저는 `www.example.com` 웹 페이지를 요청하는 GET 요청을 보냅니다.

## 6. 서버가 요청을 처리하고 응답을 다시 보냅니다.

- 서버에는 브라우저로부터 요청(Request)를 받아 요청 핸들러에 전달하여 응답을 읽고 생성하는 웹서버(예: Apache, IIS)가 포함되어 있습니다.
- 요청 핸들러는 요청, 헤더, 쿠키를 읽어 요청 내용을 확인하고 필요한 경우 서버의 정보를 업데이트하는 프로그램입니다.(ASP.NET, PHP, Ruby 등으로 작성됨)
- 그런 다음 응답(Response)를 특정 포맷(JSON, XML, HTML)으로 작성합니다.

## 7. 서버가 HTTP 응답을 전송합니다.

- 서버 응답에는 요청한 웹 페이지와 함께 상태 코드(status code), 압축 유형(Content-Encoding), 페이지 캐싱 방법(Cache-Control), 설정할 쿠키, 개인정보 등이 포함됩니다.
- HTTP 상태 코드는 응답의 상태를 알려주기 때문에 매우 중요하며, 숫자 코드를 사용하여 HTTP 응답 결과를 5가지 상태로 나타냅니다.
    - 1xx는 정보 메시지만 나타냅니다.
    - 2xx는 서버와의 요청이 성공함을 나타냅니다.
    - 3xx는 요청 완료를 위해 추가 작업 조치가 필요함을 의미합니다.
    - 4xx는 클라이언트의 Request에 오류가 있음을 나타냅니다.
    - 5xx는 서버 측의 오류로 Request를 수행할 수 없음을 나타냅니다.
- 따라서 오류가 발생한 경우 HTTP 응답을 살펴보고 어떤 유형의 상태 코드를 받았는지 확인할 수 있습니다.

## 8. 브라우저에 HTML 콘텐츠가 표시됩니다(가장 일반적인 HTML 응답의 경우).

- 브라우저는 HTML 콘텐츠를 단계적으로 표시합니다.
- 먼저 HTML 골격을 렌더링합니다. 그런 다음 HTML 태그를 확인하고 이미지, CSS 스타일시트, JavaScript 파일 등과 같은 웹 페이지의 추가 요소에 대한 GET 요청을 보냅니다. 이러한 정적 파일은 브라우저에 캐싱되므로 다음에 페이지를 방문할 때 다시 가져올 필요가 없습니다. 마지막으로 브라우저에 `www.example.com` 페이지가 나타납니다.

---

### Reference

- [웹 브라우저에 URL을 입력하면 어떤 일이 생기나요?](https://aws.amazon.com/ko/blogs/korea/what-happens-when-you-type-a-url-into-your-browser/)
- [What happens when you type a URL in the browser and press enter?](https://medium.com/@maneesa/what-happens-when-you-type-an-url-in-the-browser-and-press-enter-bb0aa2449c1a)
- [DNS 서버 유형](https://www.cloudflare.com/ko-kr/learning/dns/dns-server-types/)
- [TLS 핸드셰이크란 무엇일까요?](https://www.cloudflare.com/ko-kr/learning/ssl/what-happens-in-a-tls-handshake/)
- [HTTPS 통신 과정 쉽게 이해하기](https://aws-hyoh.tistory.com/39)
- [이미지 출처](https://dev.to/wassimchegham/ever-wondered-what-happens-when-you-type-in-a-url-in-an-address-bar-in-a-browser-3dob)
- [네트워크 스터디 2주차 - DNS](https://velog.io/@pu1etproof/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-%EC%8A%A4%ED%84%B0%EB%94%94-2%EC%A3%BC%EC%B0%A8-DNS)
