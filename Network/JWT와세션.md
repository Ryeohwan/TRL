# JWT와 세션의 기능
유저가 로그인 하면 서버가 유저한테 입장권 제시하는 것이다. 

입장권에 문제 없다고 보면 서비스 이용하게 하는 것이다. 

# 세션
세션 방식을 쓰면 안에 정보가 많지 않다. 

회원이 입장권 제시하면 서버에 저장된 리스트같은 것에서 보고 확인한다.

# JWT 토큰
jwt 쓰면 정보가 매우 많다. 

입장권 하나만 보면 안에 다 들어있어서 그 내용만 확인하면 된다.

이건 유저가 엄청 많아질 수록 서버의 부담이 적어진다. 

## JWT 토큰을 쓸 떄 유의할 점

사람들이 모르는 허점들 alg 에 none 으로 채우면 안된다! 

그러면 사람들이 None 으로 된 토큰을 넣어보기도 한다.

jwt 는 디코딩이 매우 쉬워서 여기에 민감 정보 넣으면 안된다.

여기에 시크릿 키를 대충 적으면 문제 생긴다. 

이거 부르트포스 어택에 뚫리는 경우가 있다. 

### jwt 탈취 문제 

깊게 생각안해도 된다. 잃어버린 사람 책임이다. 개발자들이 굳이 깊게 생각은 안한다. 

그대신 도난당한 카드 정지정도의 생각은 해야한다. 

## JWT 보안 대책

HttpOnly cookie , 입장권 블랙리스트 목록을 저장해두는 방식(이거 쓰면 jwt 방식 이점 옅어짐)

아니면 유효기간 짧게 → 리프레쉬 토큰을 운영해야 한다.  이 리프레쉬 토큰 로테이션이 필요하다. 

refresh token은 1회용으로 써야 안전하게 쓸 수 있다. → 복잡할 수 있으나 라이브러리 가능