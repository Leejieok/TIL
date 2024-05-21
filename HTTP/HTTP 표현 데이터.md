---
## 표현

* Content-Type: 표현 데이터의 형식
  * 미디어 타입, 문자 인코딩
  * 예)
    * text/html; charset=urt-8
    * application/json
    * image/png

* Content-Encoding: 표현 데이터의 압축 방식
  * 표현 데이터를 압축하기 위해 사용
  * 데이터를 전달하는 곳에서 압축 후 인코딩 헤더 추가
  * 데이터를 읽는 쪽에서 인코딩 헤더의 정보로 압축 해제
  * 예)
    * gzip
    * deflate
    * identity
  
* Content-Language: 표현 데이터의 자연 언어
* Content-Length: 표현 데이터의 길이

* 표현 헤더는 전송, 응답 둘 다 사용
