## HTTP를 기본으로 하는 프로토콜

### 1. HTTP의 병목 현상을 해소하는 SPDY

> Google이 2010년에 HTTP 병목 현상을 해소하고 웹 페이지 로딩 시간을 50% 단축하는 목표를 세우고 개발되었다. 
> Ajax 와 Comet 등 사용성을 쾌적하게 하는 기술이 등장하지만, 프로토콜 레벨에서의 개선이 필요해, SPDY는 HTTP 병목 현상을 해결하기 위한 프로토콜이다.

`HTTP 병목 현상` : 서버의 정보가 갱신되었는지 아닌지를 알기 위해서 클라이언트가 항상 서버 측에 확인하러 가야한다. 만약 서버 상의 정보가 갱신되지 않은 경우에는 불필요한 통신이 발생한다. 

`Ajax (Asynchronous JavaScript+XML)` : 웹 페이지의 일부분만 고쳐쓸 수 있는 비동기 통신 방법이다. 기존의 동기식 통신에 비해서 페이지의 일부분만 갱신되기 때문에 리스폰스로 전송되는 데이터의 양이 줄어든다는 장점이 있다. XMLHttpRequest 라는 API로 스크립트 언어로 서버와 HTTP 통신을 할 수 있다. Ajax를 사용해서 실시간으로 서버에서 정보를 취득하려고 하면 대량의 리퀘스트가 발생하는 문제점이 있다.

`Comet` :  서버 측의 콘텐츠에 갱신이 있을 경우, 클라이언트로부터 리퀘스트를 기다리지 않고 클라이언트에 보내기 위한 방법이다. 리스폰스를 보류 상태로 해두고, 서버의 컨텐츠가 갱신되었을 때에 리스폰스를 반환한다. 이것에 의해 갱신된 컨텐츠가 있으면 바로 반영할 수 있다. 하지만 커넥션을 유지하는 시간이 길어져, 유지하는 동안 리소스를 소비한다.

#### 1-1 설계와 기능

SPDY는 HTTP를 완전히 바꿔 놓는 것이 아닌 TCP/IP의 애플리케이션 계층과 트랜스포트 계층 사이에 새로운 세션 계층을 추가하는 형태로 동작한다. 보안을 위해서 표준으로 SSL을 사용하도록 되어있다. 

#### 1-2 장점

`다중화 스트림` : 단일 TCP 접속을 통해서 복수의 HTTP 리퀘스트를 무제한으로 처리할 수 있다. 한 번의 TCP 접속으로 리퀘스트를 주고 받는 것이 가능해 TCP 의 효율이 높아진다.

`리퀘스트 우선 순위 부여` : 무제한으로 리퀘스트를 병렬 처리할 수 있지만 각 리퀘스트에 우선순위를 부여할 수 있다. 이것은 대역폭이 좁으면 늦어지는 현상을 해결하기 위해서이다.

`HTTP 헤더 압축` : 헤더를 압축해 보다 적은 패킷수와 송신 바이트 수로 통신할 수 있다.

`서버 푸시 기능` : 서버에서 클라이언트로 데이터를 푸쉬하는 서버 푸시 기능을 지원한다. 그래서 서버 측은 클라이언트 측에서 리퀘스트를 기다리지 않고 데이터를 보낼 수 있다.

`서버 힌트 기능` : 서버가 클라이언트에게 리퀘스트 해야 할 리소스를 제안할 수 있다. 클라이언트가 자원을 발견하기 전에 리소스의 존재를 알 수 있기 때문에 캐시를 가지고 있는 상태라면 불필요한 리퀘스트를 보내지 않아도 된다.

### 2. 브라우저에서 양방향 통신을 하는 WebSocket

> 웹 브라우저와 웹 서버를 위한 양방향 통신 규격으로 프로토콜을 IETF가 책정하고, API는 W3C가 책정한다. 
> 주로 Ajax 와 Comet에서 사용하고 있는 XMLHttpRequest의 결점을 해결하기 위한 기술로서 개발이 진행되고 있다. 웹 서버와 클라이언트가 한번 접속을 확립하면 그 뒤의 통신을 모두 전용 프로토콜로 하는 방식으로 JSON, XML,HTML이나 이미지등 임의 형식의 데이터를 보낸다. HTTP에 의한 접속 출발점이 클라이언트에 있다는 것에는 변함이 없지만 **WebSocket을 사용하여 서버와 클라이언트 어느 쪽에서도 송신을 할 수 있다.**

#### 2-1 장점

`서버 푸시 기능` : 서버에서 클라이언트로 데이터를 푸쉬하는 서버 푸시 기능을 지원한다. 그래서 서버 측은 클라이언트 측에서 리퀘스트를 기다리지 않고 데이터를 보낼 수 있다.

`통신량의 삭감` : 접속을 한번 확립하면 접속을 유지하려고 한다. HTTP에 비해 오버헤드가 적어지고, 또 헤더의 사이즈도 작기 때문에 통신량을 줄일 수 있다. WebSocket을 하려면 HTTP에 접속을 확립하고, 핸드쉐이크 절차를 밟을 필요가 있다.

`핸드쉐이크 / 리퀘스트` : WebSocket 하려면 HTTP의 Upgrade 헤더 필드를 사용해서 프로토콜을 변경하는 것으로 핸드쉐이크를 실시한다. 

### 3. 등장이 기다려지는 HTTP/2.0 

> 현재 주류인 HTTP/1.1은 1999년 공개된 이후로 개정되고 있지 않다. HTTP/2.0은 사용자가 웹을 이용할 때의 체감 속도의 개선을 목표로 하고 있다. SPDY, HTTP Speed+Mobility, Network-Friendly HTTP Upgrade 프로토콜이 베이스가 되어 사양이 검토되고 있다.
> **HTTP Speed+Mobility**는 마이크로소프트 사가 제안하고 있는 모바일 통신에서 통신 속도와 효율성을 개선하기 위한 규격이다. **Network-Friendly HTTP Upgrade**는 주로 모바일 통신에서 HTTP 의 효율 개선을 위한 규격이다. 
> ~~예전 책이기 때문에 현재는 다를것으로 예상된다. 추후에 추가해야겠다.~~

### 4. 웹 서버 상의 파일을 관리하는 WebDAV

> 웹 서버의 콘텐츠에 대해서, 직접 파일 복사나 편집 작업을 할 수 있는 분산 파일 시스템으로 HTTP/1.1을 확장한 프로토콜로 RFC4918에 정의되어 있다. 파일 작성이나 삭제 등 기본적인 기능 이외에 파일 작성자 등의 관리나 편집중에 다른 유저가 다시 고쳐 쓰지 못하도록 잠금 기능, 갱신 정보를 관리하는 리비전 기능 등이 준비되어 있다.

#### 4-1 HTTP/1.1 을 확장한 기능

`컬렉션(Collection)` :  여러 개의 리소스를 한꺼번에 관리하기 위한 개념이다. 
`프로퍼티(Property)` : 리소스의 프로퍼티를 정의한 것이다. "이름 = 값"의 형식으로 이루어져 있다.
`잠금(Lock)` : 파일을 편집할 수 없는 상태로 한다. 여러 명의 사람이 동시에 편집하는 경우 등 동시에 작성되는 것을 예방한다. 



#### 5. HTTP는 왜 이렇게까지 사용될까?

과거 네트워크를 이용한 시스템이나 소프트웨어를 새로 마들 때에는 필요한 기능을 구현한 새로운 프로토콜을 만드는 경우가 있었다. 최근에는 HTTP를 사용하는 쪽인데, **기업이나 조직 등의 방화벽 설정과 크게 관련이 있다.** 방화벽의 기본 기능 중에 지정된 프로콜이나 포트 번호 이외의 패킷은 통과시키지 않는다는 기능이 있어 이로 인해 새로운 프로토콜이나 포트번호를 사용하는 경우에는 설정을 변경할 필요가 생긴다. 인터넷에서 가장 많이 활용되고 있는 분야는 웹이다. FTP나 SSH등이 허가되지 않아도 웹으로 액세스 가능한 회사가 대부분이다. 그렇기 때문에 많은 회사나 조직에서 허가된 통신 환경인 경우가 많기 때문에 방화벽의 설정을 변경할 필요가 없고 HTTP라면 도입이 용이하다는 장점이 있다. 다른 이유로는 **HTTP 클라이언트인 브라우저가 보급되어 있는 것도 이유이다.**