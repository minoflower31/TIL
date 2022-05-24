# URI (Uniform Resource Identifier)

<img width="442" alt="image" src="https://user-images.githubusercontent.com/56334513/170079522-4b9c93de-98fb-4a88-8706-12578fcc7f3d.png">

URI이란,
+ 인터넷에 존재하는 모든 자원을 나타내는 유일한 주소
+ 자원(비디오, 음악파일, 페이지 등)를 식별하는 정보

## URL (Uniform Resource Locator)

### URL(Locator): 리소스가 있는 위치를 지정

> **scheme://[userinfo@]host[:port][/path][?query][#fragment]**

#### scheme
+ 주로 **프로토콜**(어떤 방식으로 자원에 접근할 것인가?에 대해 약속을 정한 규칙)을 사용한다.
+ 포트는 생략가능함.

#### host
+ 호스트명
+ 도메인명 또는 IP 주소

#### path
+ 리소스 경로
+ e.g. /home/file1.jpg, /members/1

#### query
+ `?`로 시작하며 `&`로 추가함
  + e.g. `?keyA=valueA&keyB=valueB`
+ **query parameter** 또는 **query string**이라 함.

#### fragment
+ html 내부 북마크 등으로 사용하기 때문에 서버에 전송하지 않는 정보

<br>

# 웹 브라우저 요청 흐름

<img width="639" alt="image" src="https://user-images.githubusercontent.com/56334513/170081875-7bcba8a9-6296-4135-bdb9-cefada28bcf5.png">

<br>
<br>

> 출처: 인프런 - 김영한님의 모든 개발자를 위한 HTTP 웹 기본 지식 https://inf.run/g8eP