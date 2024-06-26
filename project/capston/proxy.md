# Proxy

## 프록시(Proxy)

프록시는 “대리”의 의미로, 인터넷과 관력해서 쓰이는 경우, 특히 내부 네트워크에서 인터넷 접속을 할 때에, 빠른 액세스나 안전한 통신등을 확보하기 위한 중계서버를 “프록시 서버”라고 일컫는다. 클라이언트와 Web서버의 중간에 위치하고 있어, 대신 통신을 받아 주는 것이 프록시 서버이다.

## 프록시(Proxy)의 종류

### 포워드 프록시

클라이언트의 대신 프록시 서버가 목적 서버에 통신해주는 구성을 “포워드 프록시”라고 한다. 프록시를 사용하지 않는 경우 자신의 직접적인 로그를 남기게 된다.

<figure><img src="../../.gitbook/assets/image (113).png" alt=""><figcaption></figcaption></figure>

포드워 프록시의 경우 서버가 외부 Web 서버와 통신을 한다. 그러므로 클라이언트 프록시 서버만을 통해 정보를 얻게 된다. 따라서 Web 서버쪽에서는 프록시 서버를 통한 액세스 로그가 남는다.

<figure><img src="../../.gitbook/assets/image (114).png" alt=""><figcaption></figcaption></figure>

### 포워드 프록시의 장점

#### 캐시 저장(액세스 고속화)

프록시 서버에 캐시를 저장할 수 있다. 다시 동일한 페이지를 리퀘스트 했을 때에는 캐시에 남아 있는 정보를 클라이언트에게 준다. 이것으로 사이트에 접속하는 속도가 빨라진다.

<figure><img src="../../.gitbook/assets/image (115).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (116).png" alt=""><figcaption></figcaption></figure>

#### URL 필터링

외부의 액세스는 프록시 서버를 경유하므로 사용자 전원의 외부 웹 사이트로의 액세스를 필터링할 수 있다.

<figure><img src="../../.gitbook/assets/image (117).png" alt=""><figcaption></figcaption></figure>

### 리버스 프록시

포워드 프록시와 달리 Web 서버쪽에 위치하여 클라이언트의 접근을 최초로 받아 리퀘스트에 해당하는 Web 서버에 배분해주는 역할을 한다.

<figure><img src="../../.gitbook/assets/image (118).png" alt=""><figcaption></figcaption></figure>

클라이언트에서 액세스를 프록시 서버에 집약해서 URL에 따라 리퀘스트를 받을 Web 서버가 바뀌도록 서정하고 있다.

이 때 클라이언트의 입장에 있어서는 프록시 서버가 Web 서버와 같은 동작을 하므로 Web서버가 여러 개 존재하는 것을 은폐할 수 있는 것도 리버스 프록시의 특징이다.

### 리버스 프록시의 장점

#### 부담 분산

설정으로 정적 콘텐츠와 동적 콘텐츠의 보는 곳을 나눔으로써 메모리 사용량의 효율화를 할 수 있다. 로드 밸런스와 병용하면 더욱 부담을 분산할 수 있다.

#### 캐시의 저장

포워드 프록시와 동일하게 동일한 데이터를 얻을 때에 프록시 서버가 저장했던 내용을 돌려준다

#### 세큐리티 대책, 바이러스 대책

통신시 프록시 서버에 집약되므로 프록시 서버 내에 세큐리티 대책, 바이러스 대책을 구현하여 Web 서버로의 부정 액세스, 사용등을 방지할 수 있다.
