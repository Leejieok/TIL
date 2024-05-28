***
## URI(Uniform Resource Identifier)

URI? URL? URN?
URI는 로케이터(locator), 이름(name) 또는 둘 다 추가로 분류될 수 있다.

로케이터는 위치
네임은 이름이라고 보면 된다.

### URI
단어 뜻
* Uniform: 리소스 식별하는 통인된 방식
* Reosurce: 자원, URI로 식별할 수 있는 모든 것(제한 없음)
* Identifier: 다른 항목과 구분하는데 필요한 정보

* URL: Uniform Resource Locator
* URN: Uniform Resource Name

### URL, URN
* URL - Locator: 리소스가 있는 위치를 지정
* URN - Name: 리소스에 이름을 부여
* 위치는 변할 수 있지만, 이름은 변하지 않는다.
* urn:isbn:342749232837 (어떤 책의 isbn URN)
* URN 이름만으로 실제 리소스를 찾을 수 있는 방법이 보편화 되지 않음

**URL 분석**
https://www.google.com/search?q=hello&hl=ko

**URL 전체문법**
* scheme://[userinfo@]host[:port][/path][?query][#fragment]
* https://www.google.com:443/search?q=hello&hl=ko

* 프로토콜(https)
* 호스트명(www.google.com)
* 포트 번호(443)
* 패스(/search)
* 쿼리 파라미터(q=hello&hl=ko)

* 스키마
* 주로 프로토콜 사용
* 프로토콜: 어떤 방식으로 자원에 접근할 것인가 하는 약속 규칙
	* 예) http, http, ftp 등등
* http는 80 포트, https는 443포트를 주로 사용, 포트는 생략 가능
* https는 http 에 보안 추가(HTTP secure)

* userinfo
* URL에 사용자정보를 포함해서 인증
* 거의 사용하지 않음

* host
* 호스트명
* 도메인명 또는 IP 주소를 직접 사용가능

* PORT
* 접속 포트
* 일반적으로 생략, 생략시 http는 80, https는 443

* PATH
* 리소스 경로(path), 계층적 구조
* 예
	* /home/file1.jpg
	* /members
	* /members/100, /items/iphone12

* query
* key=value형태
* ?로 시작, &로 추가 가능 ?keyA=valueA&keyB=valueB
* query parameter, query string 등으로 불림, 웹서버에 제공하는 파라미터, 문자 형태

* fragment
* html 내부 북마크 등에 사용
* 서버에 전송하는 정보 아님

***
## 요청 흐름
클라이언트가 웹브라우저에서 해당 url을 요청했을 때 흐름
1. 구글 서버를 찾기 위해 dns서버를 조회한다.
2. ip와 포트 정보를 찾게 된다.
3. http 요청 메시지를 생성한다.
4. 패킷을 감싼 http메시지를 구글 서버에 보냄
5. 구글 서버에서는 http요청을 보고 응답 메시지를 만듦
6. 구글 서버에서 만든 http 응답 메시지를 클라이언트 웹 브라우저로 전송
7. 웹 브라우저에서 응답 메시지를 풀어서 렌더링 함
8. 사용자가 화면으로 볼 수 있게됨.
