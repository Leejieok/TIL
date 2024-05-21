* 클라이언트에서 서버로 데이터 전송
* HTTP API 설계 예시

클라이언트에서 서버로 데이터 전송
데이터 전달 방식은 크게 2가지

* 쿼리 파라미터를 통한 데이터 전송
	* GET
	* 주로 정렬 필터 (검색어)
* 메시지 바디를 통한 데이터 전송
	* POST, PUT, PATCH
	* 회원 가입, 상품 주문, 리소스 등록, 리소스 변경

4가지 상황 (클라이언트 -> 서버)
* 정적 데이터 조회
	* 이미지, 정적 텍스트 문서
* 동적 데이터 조회
	* 주로 검색, 게시판 목록에서 정렬 필터(검색어)
* HTML Form을 통한 데이터 전송
	* 회원 가입, 상품 주문, 데이터 변경
* HTTP API를 통한 데이터 전송
	* 회원가입, 상품 주문, 데이터 변경
	* 서버 to 서버, 앱 클라이언트, 웹 클라이언트(Ajax)

 ***정적 데이터 조회
 쿼리 파라미터 미사용
 
GET **/static/star.jpg** HTTP/1.1 -->이 경로만 보고 데이터를 찾아서 클라이언트에게 전달
Host: localhost:8080
->
/static/star.jpg
HTTP/1.1 200 OK
Content-Type: image/ jpeg
Content-Length: 34012

정적 데이터 조회
정리

* 이미지, 정적 텍스트 문서
* 조회는 GET 사용
* ==정적 데이터는 일반적으로 쿼리 파라미터 없이 리소스 경로로 단순하게 조회 가능==


동적 데이터 조회
쿼리 파라미터 사용

https://www.google.com/search?q=hello&hl=ko
GET /search==?q=hello&hl=ko== (<-쿼리파라미터) HTTP/1.1
Host: www.google.com

서버에서는 쿼리 파라미터를 기반으로 정렬 필터해서 결과를 동적으로 생성

정리
* 주로 검색, 게시판 목록에서 정렬 필터(검색어)
* 조회 조건을 줄여주는 필터, 조회 결과를 정렬하는 정렬 조건에 주로 사용
* 조회는 GET 사용
* GET은 쿼리 파라미터 사용해서 데이터를 전달


HTML Form 데이터 전송
POST 전송 - 저장

![[Pasted image 20240520144503.png]]

HTML Form 데이터 전송
multipart/form-data
![[Pasted image 20240520151019.png]]

정리
* HTML Form submit시 POST 전송
	* 예) 회원가입, 상품주문, 데이터 변경
* Content-Type: application/x-www-form-urlencoded 사용
	* form의 내용을 메시지 바디를 통해서 전송(key=value, 쿼리 파라미터 형식)
	* 전송 데이터를 url encoding 처리
		* 예) abc김 -> abc%EA%B9%80
* HTML Form은 GET 전송도 가능
* Content-Type: multipart/form-data
	* 파일 업로드 같은 바이너리 데이터 전송시 사용
	* 다른 종류의 여러 파일과 폼의 내용 함께 전송 가능(그래서 이름이 multipart)
* 참고: HTML Form 전송은 GET, POST만 지원



HTTP API 데이터 전송
![[Pasted image 20240520152026.png]]
