# Nginx 의 기능 중 리버스 프록시가 뭐냐

    리버스 프록시란 클라이언트 요청을 대신 받아 내부 서버로 전달해주는 것을 리버스 프록시라고 한다. 
    프록시란 대리 라는 의미로, 정보를 대신 전당해주는 주체라 생각하자.

## 만약 이 프록시 없이 서버 운영한다면?
<img width="767" alt="스크린샷 2023-01-13 오후 5 52 42" src="https://user-images.githubusercontent.com/73810834/212278500-0edc2a97-2490-4728-af9a-c14cd25b1573.png">

사용자가 갑자기 많아지거나 하면 웹 서버가 그대로 노출되어 있기 때문에 서버 다운되거나 보안적으로도 안좋을 것이다. 

nginx를 사용하면 로드 밸런싱으로 부하를 줄여줄 수 있고, 분산 처리 또한 가능하며 웹서버의 SSL 인증도 적용할 수 있다.

# 그럼 SSL 은 무엇인가
    Secure Socket Layer
또는 
    Transfer Layer Security
라는 보안 프로토콜을 통해 클라이언트와 서버가 보안이 향상된 통신을 하는 것을 말한다.

# HTTPS는 무엇인가..

HTTP 뒤에 붙는 S는 보안(Secure)을 나타낸다.
이는 취약점 보완으로 보안이 향상된 웹통신을 하기 위함이 가장 큰 목적이다.

기존 HTTP 프로토콜은 오픈형이라 패킷을 도청당할 수 있기에 이걸 방지한 것이다.

# SSL 과 HTTPS 의 차이

SSL과 TLS(TLS 가 SSL 의 후속 버전인데 일반적으로 그냥 SSL 이라고 한다.) 는 '보안 계층' 이라는 독립적인 프로토콜 계층을 만들어 응용계층과 전송계층 사이에 속하게 된다.

HTTPS는 SSL 또는 TLS 위에 HTTP 프로토콜을 얹어 보안된 HTTP 통신을 하는 프로토콜이다.

즉 SSL 과 TLS 는 HTTP 뿐만이 아니라 FTP, SMTP 와 같이 다른 프로토콜에도 적용할 수 있으며, HTTPS 는 TLS 와 HTTP 조합된 프로토콜만을 가리키는 것이다.




# 참조
https://narup.tistory.com/238

https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=skinfosec2000&logNo=222135874222