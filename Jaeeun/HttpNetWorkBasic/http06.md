### 1. 1대로 멀티 도메인을 가능하게 하는 가상 호스트
> HTTP/1.1에서는 하나의 HTTP 서버에 여러 개의 웹 사이트를 실행할 수 있습니다. 웹 호스팅을 제공하고 있는 사업자는 1대의 서버에 여러 고객의 웹 사이트를 넣을 수 있습니다.
> **가상 호스트 기능을 사용하면 물리적으로는 서버가 1대지만 가상으로 여러대가 있는 것처럼 설정하는 것이 가능하다.**

인터넷에서 도메인명은 DNS에 의해서 IP 주소로 변환되고 나서 액세스하게 됩니다. 결국 리퀘스트가 서버에 도착한 시점에는 IP 주소를 기준으로 액세스하게 됩니다. 그렇기 때문에 HTTP 리퀘스트를 보내는 경우에는 호스트명과 도메인 명을 완전하게 포함한 URI를 지정하거나, 반드시 Host 헤더 필드에서 지정해야 한다.

### 2. 통신을 중계하는 프로그램 (프록시, 게이트웨이, 터널)

#### 2-1 프록시
> 프록시 서버의 기본적인 동작은 클라이언트로부터 받은 리퀘스트를 다른 서버에게 전송하는 것이다. 
> 리소스 본체를 가진 서버를 오리진 서버라고 부르는데, 오리진 서버로부터 되돌아온 리스폰스는 프록시 서버를 경유해서 클라이언트에 돌아온다.

프록시 서버를 사용하는 이유는 **캐시를 사용**해서 네트워크 대역 등을 효율적으로 사용하는 것과 조직 내의 특정 웹 사이트에 대한 액**세스 제한, 액세스 로그를 획득하는 정책을 철저하게 지키려는 목적**으로 사용한다.

`캐싱 프록시 (Cashing Proxy)` : 프록시로 리스폰스를 중게하는 때에는 프록시 서버 상에 리소스 캐시를 보존해 두는 타입의 프록시이다.
<<<<<<< HEAD
프록시에 같은 리소스에 리퀘스트가 온 경우, 캐시를 리스폰스로 되돌려 주는 것이다.

`투명 프록시 (Transparent Proxy)` : 리퀘스트와 리스폰스를 중계를 할 때 메세지 변경을 하지 않는 타입의 프록시를 투명 프록시라고 한다.
반대로 메세지에 변경을 가하면 비투과 프록시라고 한다.
=======
프록시에 같은 리소스에 리퀘스트가 온 경우, 캐시를 리스폰스로 되돌려 주는 것이다. <br>

`투명 프록시 (Transparent Proxy)` : 리퀘스트와 리스폰스를 중계를 할 때 메세지 변경을 하지 않는 타입의 프록시를 투명 프록시라고 한다.
반대로 메세지에 변경을 가하면 비투과 프록시라고 한다. <br>
>>>>>>> ef6f1b2da9c5959acefa4028d6e12a0659dde65a

#### 2-2 게이트웨이
> 게이트웨이의 동작은 프록시와 비슷하지만, 다음에 있는 서버가 HTTP 서버 이외의 서비스를 제공하는 서버라는 것이다. 클라이언트와 게이트웨이 사이를 암호화하는 등으로 안전하게 접속함으로써 통신의 안정성을 높인다.

#### 2-3 터널
> 터널은 요구에 따라서 다른 서버와의 통신 경로를 확립한다. 이 때 클라이언트는 SSL 같은 암호화 통신을 통해 서버와 안전하게 통신을 하기 위해 사용한다.

### 3. 리소스를 보관하는 캐시 (Cache)
> 캐시는 프록시 서버와 클라이언트의 로컬 디스크에 보관된 리소스의 사본을 가리킵니다. 캐시를 사용하면 리소스를 가진 서버에의 액세스를 줄이는 것이 가능하기 때문에 통신량과 통신 시간을 절약할 수 있다.

#### 3-1 캐시 서버의 장점
캐시 서버는 프록시 서버의 하나로 캐싱 프록시로 분류된다. 캐시 서버의 장점은 캐시를 이용함으로써 같은 데이터를 몇 번이고 오리진 서버에 전송할 필요가 없다는 것이다. 그래서 클라이언트는 네트워크에서 가까운 서버로부터 리소스를 얻을 수 있어 서버는 같은 리퀘스트를 매번 처리 할 필요가 없다.

#### 3-2 캐시의 유효기간
**캐시 서버에 캐시가 있는 경우라도 같은 리소스의 리퀘스트에 대해서 항상 캐시를 돌려준다고 볼 수 없다.** 이것은 캐시되어 있는 **리소스의 유효성**과 관계가 있다. 
오리진 서버에 있는 원래 리소스가 갱신되는 경우에, 캐시 서버는 갱신되기 전의 낡은 리소스를 그대로 보내게 된다. 그래서 캐시를 가지고 있더라도 캐시의 유효 기간 등에 의해서 오리진 서버에 유효성을 확인하거나 새로운 리소스를 획득하러 가게 되는 경우가 있다.

#### 3-3 클라이언트의 캐시 
**캐시 서버만 캐시를 가지고 있는 게 아니라 클라이언트가 사용하고 있는 브라우저에서도 캐시를 가질 수 있다.** 인터넷 익스플로어에서 클라이언트가 보존하는 캐시를 인터넷 임시 파일이라고 부릅니다. **서버에 액세스 하지 않고 로컬 디스크로부터 불러온다.**
캐시 서버와 마찬가지로 리소스가 오래된 것으로 판단된 경우에 오리진 서버에 리소스의 유효성을 확인하러 가거나 새로운 리소스를 다시 획득하러 간다.