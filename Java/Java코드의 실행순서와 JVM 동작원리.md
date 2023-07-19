# Java 코드의 실행 순서
<img width="677" alt="image" src="https://github.com/Ryeohwan/Spring_study/assets/73810834/e2ddf5bd-7213-4f14-992b-35086d4f4b8c">

1. 작성한 자바소스 파일을 자바 컴파일러를 통해 자바 바이트코드로 컴파일한다.
2. 컴파일된 바이트코드를 JVM 의 클래스 로더에게 전달한다. 
3. 클래스 로더는 동적로딩을 통해 필요한 클래스들을 로딩 및 링크하여 런타임 데이터영역, 즉 JVM 의 메모리에 올린다.
4. 실행엔진(Excution Engine)은 JVM 메모리에 올라온 바이트 코드들을 명령어 단위로 하나씩 가져와서 실행한다.

# JVM 의 구조

<img width="684" alt="image" src="https://github.com/Ryeohwan/Spring_study/assets/73810834/3b5e5ddf-3b16-46c0-a502-74dbd4fe2f67">

1. 클래스 로더
2. 런타임 데이터 영역
3. 실행 엔진

으로 구성되어 있다.

## 클래스 로더
클래스 로더의 특징은 다음과 같다.
1. 계층구조
2. 위임모델
3. 가시성 제한
4. 언로드 불가
5. 이름 공간

<img width="684" alt="image" src="https://github.com/Ryeohwan/Spring_study/assets/73810834/11ba29f8-7b1b-491c-93f5-2ab36fec490a">


클래스 로더는 단순하게 하나로 이루어져 있지 않아요 위의 그림처럼 여러 클래스 로더끼리 부모자식 관계를 
이루고 있어서 계층적인 구조로 되어 있다. 

### 부트스트랩 클래스 로더(Bootstrap ClassLoader)
- 최상위 클래스로더로 유일하게 JAVA가 아니라 네이티브 코드로 구현이 되어있다.
- JVM이 실행될 때 같이 메모리에 올라간다
- Object 클래스를 비롯하여 JAVA API들을 로드한다.
### 익스텐션 클래스 로더(Extension ClassLoader)
- 기본 JAVA API를 제외한 확장 클래스들을 로드한다. (다양한 보안 확장기능 로드)
### 시스템 클래스 로더(System Class Loader)
- 부트스트랩과 익스텐션 클래스로더가 JVM 자체의 구성요소들을 로드한다면, 시스템 클래스 로더는 어플리케이션의 클래스들을 로드한다.
- 사용자가 지정한 $CLASSPATH 내의 클래스들을 로드한다.
### 사용자 정의 클래스 로더(User-Defined Class Loader)
- 어플리케이션 사용자가 직접 코드상에서 생성하여 사용하는 클래스로더.

웹 어플리케이션 서버(Web Application Server : VAS)와 같은 프레임 워크는 웹 어플리케이션, 엔터프라이즈 어플리케이션이 서로 독릭접으로 동작하게 하기 위해서 사용자 정의 클래스 로더들을 사용하여 클래스 로더의 위임 모델을 통해 어플리케이션의 독립성을 보장한다.

따라서 WAS의 클래스 로더 구조는 WAS벤더마다 조금씩 다른 형태의 계층 구조를 사용하고 있다고 한다.


