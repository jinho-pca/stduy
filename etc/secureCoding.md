# Secure Coding

### 서울 1반 이진호

------

## Index

- XSS
- CSRF
- 어플레케이션 보안 취약점 정적 분석과 동적 분석
- OWASTP TOP 10 2021

------

### 1. XSS

#### (1) 정의

- 클라이언트 사용자로부터 데이터를 받을 때 검증이나 인코딩하지 않고 문자열 그대로 브라우저에 뿌리거나 서버 디비에 저장되어 브라우저에서 로딩될 때 악성코드가 실행되어 발생

#### (2) 과정

- 가해자 : 스크립트 삽입이 가능한 취약점이 있는 웹사이트 확인 -> 가해자는 방문자 세션 쿠기를 전송하는 악성 스크립트 웹 사이트에 주입
- 피해자 : 피해자는 웹사이트를 방문하여 악성 스크립트를 받게되고 사용자 클라이언트에서 실행되게 된다 -> 피해자의 세션 쿠키 등이 스크립트로 인해 가해자의 서버로 전송

#### (3) Code

- javascript`<script>alert("메롱")</script>` `<img src=1 onerror=alert("메롱")>` 이름에 입력하면 경고창이 나타난다`내 이름은 <span v-html="myname"></span> 입니다` vue에서 데이터 값을 인코딩이나 필터 회피 없이 텍스트 그대로 보여주는 속성

JavaScript

1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19

```
<body>    <div>        이름: <input type="text" id"myname"/><button onClick="getName()">입력</button>    <br>    내 이름은 <span id="name"></span>입니다.    </div> <body> <script type="text javascript"> function getName() {    axios.get('http://localhost:8000/xss?name=' + document.gerElementById("myname").value)    .then(response => {        console.log(response.data);        document.gerElementById("name").innerHTML = response.data.name;    })    .catch(error =>        console.log(error);    ) } 
```

- python (django)

  - settings.py `MIDDLEWARE`에 `django.middleware.security.SecurityMiddleware` 추가

  - `axios.get('http://localhost:8000/xss?name=' + document.gerElementById("myname").value)`에서 `axios.get('http://localhost:8000/xss/name' + document.gerElementById("myname").value)`

     

    get method는 파라미터보다는 path variable로 해야 악의적인 스크립트가 완성되지 않는다

Python

1 2 3

```
@api_view(['GET']) def xss_test(request):    return Response({"name": request.GET.get('name')})
```

------

### 2. CSRF

#### (1) 정의

- CSRF 공격(Cross Site Request Forgery)은 웹 어플리케이션 취약점 중 하나로 인터넷 사용자(희생자)가 자신의 의지와는 무관하게 공격자가 의도한 행위(수정, 삭제, 등록 등)를 특정 웹사이트에 요청하게 만드는 공격

#### (2) 과정

- 피해자 : 가래자는 웹 사이트에 대한 인증을 완료하여 인증 쿠키가 유효합니다 -> 피해자는 인증이 유효함으로 서버로부터 온 요청은 처리가 되고 완료가 됩니다
- 가해자 : 가해자는 피해자의 유효한 세션 쿠키가 존재할 때 가해자의 이득이 되는 위조된 요청의 링크를 생성해 보내고 클릭하게끔 피싱을 합니다 -> 결과 등을 확인할 수 있습니다

#### (3) 인증과 인가

- 인증 (Authentication) 사이트 이용하여 로그인
- 인가 (Authorization) 게시글 CRUD

#### (4) Code

- python (django)settings.py `MIDDLEWARE`에 `django.middleware.csrf.CsrfViewMiddleware` 추가

------

### 3. 분석

#### (1) 정적 분석과 동적 분석

|          | 정적 분석               | 동적 서버                         |
| :------- | :---------------------- | :-------------------------------- |
| 점검대상 | 어플리케이션 소스 코드  | 실제 어플리케이션 운영 서버       |
| 평가기술 | 패턴 비교 분석          | HTTP 메시지의 변경 점검           |
| 점검단계 | 어플리케이션 개발 시점  | 어플리케이션 운영 시점            |
| 결과     | 소스의 라인 별결과 표시 | HTTP 요청 응답에 따른 겨로가 표시 |

#### (2) sonarqube

- 소나큐브는 20개 이상의 프로그래밍 언어에서 버그, 코드 스멜, 보안 취약점을 발견할 목적으로 정적 코드 분석으로 자동 리뷰를 수행하기 위한 지속적인 코드 품질 검사용 오픈 소스 플랫폼이다.
- [소나큐브](https://www.sonarqube.org/downloads/?gads_campaign=Asia-SonarQube&gads_ad_group=SonarQube&gads_keyword=sonarqube&gclid=Cj0KCQiAys2MBhDOARIsAFf1D1cn0-BiUsM4L7QNjLUCRCwgaSVChF2EwIlr_JTOkGxC0531ZGm0QTgaAv89EALw_wcB)

------

### 4. OWASP top 10

#### (1) OWASP

- 오픈 웹 어플리케이셔느이 보안 위약점 중에서 가장 위험하고 치명적이며 빈번한 취약점을 10개 발표

#### (2) SQL Injection

- OWASP top 10의 취약점 1위로서 시스템 DB에 쿼리문을 주입하는 방식

#### (3) 2021

- **Broken Access Control (접근 권한 취약점)**
  - 엑세스 제어는 사용자가 권한을 벗어나 행동할 수 없도록 정책을 시행
  - 만약 엑세스 제어가 취약하면 사용자는 주어진 권한을 벗어나 모든 데이터를 무단으로 열람, 수정 혹은 삭제 등의 행위 가능 
- **Cryptographic Failures (암호화 오류)**
  - Sensitive Data Exposure(민감 데이터 노출)의 명칭이 2021년 Cryptographic Failures(암호화 오류)로 변경
  - 적절한 암호화가 이루어지지 않으면 민감 데이터가 노출
- **Injection (인젝션)**
  - SQL, NoSQL, OS 명령, ORM(Object Relational Mapping), LDAP, EL(Expression Language) 또는 OGNL(Object Graph Navigation Library)
  - 인젝션 취약점은 신뢰할 수 없는 데이터가 명령어나 쿼리문의 일부분으로써, 인터프리터로 보내질 때 취약점이 발생
- **Insecure Design (안전하지 않은 설계)**
  - Insecure Design(안전하지 않은 설계)는 누락되거나 비효율적인 제어 설계로 표현되는 다양한 취약점을 나타내는 카테고리
  - 안전하지 않은 설계와 안전하지 않은 구현에는 차이가 있지만, 안전하지 않은 설계에서 취약점으로 이어지는 구현 결함이 존재 가능
- **Security Misconfiguration (보안설정오류)**
  - 애플리케이션 스택의 적절한 보안 강화가 누락되었거나 클라우드 서비스에 대한 권한이 적절하지 않게 구성되었을 때
  - 불필요한 기능이 활성화 되거나 설치되었을 때, 기본계정 및 암호화가 변경되지 않았을 때, 지나치게 상세한 오류 메세지를 노출할 때
  - 최신 보안기능이 비활성화 되거나 안전하지 않게 구성되었을 때 발생
- **Vulnerable and Outdated Components (취약하고 오래된 요소)**
  - 취약하고 오래된 요소는 지원이 종료되었거나 오래된 버전을 사용할 때 발생
  - 애플리케이션 뿐만 아니라, DBMS, API 및 모든 구성요소 들이 포함
- **Identification and Authentication Failures (식별 및 인증 오류)**
  - Broken Authentication(취약한 인증)으로 알려졌던 해당 취약점은 identification failures(식별 실패)까지 포함하여 더 넓은 범위를 포함할 수 있도록 변경
  - 사용자의 신원확인, 인증 및 세션관리가 적절히 되지 않을 때 취약점이 발생
- **Software and Data Integrity Failures(소프트웨어 및 데이터 무결성 오류)**
  - 2021년 새로 등장한 카테고리로 무결성을 확인하지 않고 소프트웨어 업데이트, 중요 데이터 및 CI/CD 파이프라인과 관련된 가정을 하는데 중점
- **Security Logging and Monitoring Failures (보안 로깅 및 모니터링 실패)**
  - Insufficient Logging & Monitoring(불충분한 로깅 및 모니터링) 명칭이었던 카테고리가 Security Logging and Monitoring Failures (보안 로깅 및 모니터링 실패)로 변경
  - 로깅 및 모니터링 없이는 공격활동을 인지할 수 없습니다. 이 카테고리는 진행중인 공격을 감지 및 대응하는데 도움
- **Server-Side Request Forgery (서버 측 요청 위조)**
  - 2021년 새롭게 등장한 SSRF 결함은 웹 애플리케이션이 사용자가 제공한 URL의 유효성을 검사하지 않고 원격 리소스를 가져올 때마다 발생
  - 이를 통해 공격자는 방화벽, VPN 또는 다른 유형의 네트워크 ACL(액세스 제어 목록)에 의해 보호되는 경우에도 응용 프로그램이 조작된 요청을 예기치 않은 대상으로 보내도록 강제 가능