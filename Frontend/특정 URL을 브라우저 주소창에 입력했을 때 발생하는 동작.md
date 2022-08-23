# 특정 URL을 브라우저 주소창에 입력했을 때 발생하는 동작

2022/08/21

## 1.www.google.com을 브라우저 주소창에 입력했다 가정하자. Browser는 캐싱된 DNS 기록들을 통해 www.google.com에 대응되는 IP 주소가 있는지 확인한다.

- 인터넷에 있는 **모든 URL들에는 고유의 IP 주소가 지정**되어있다. 이 IP 주소를 통해서 해당 웹사이트를 호스팅하고 있는 서버 컴퓨터에 접근을 할 수 있다.
- 브라우저는 브라우저 캐시 -> OS 캐시 -> router 캐시 -> ISP 캐시 순으로 접근하며 DNS 기록을 확인한다.

### DNS?

- URL들의 이름과 IP주소를 저장하고 있는 데이터베이스다. 사람들이 웹사이트 주소에 쉽게 접속할 수 있게 매핑을 해주는 역할을 한다.

## 2. 요청한 URL이 캐시에 없으면, ISP의 DNS 서버가 www.google.com을 호스팅하고 있는 서버의 IP 주소를 찾기 위해 DNS query를 날린다.

- 여러 다른 DNS 서버들을 검색해서 해당 사이트의 IP 주소를 찾는 것이 목적이다.

### DNS recursor?

- ISP의 DNS 서버(인터넷을 통해 다른 DNS 서버들에게 연락해서 도메인 이름의 올바른 IP 주소를 찾는다.)

### Name server?

- google이 아닌 다른 DNS 서버들

### 검색 순서

1. DNS recursor가 **root name server**에 연락한다.
2. .com 도메인 **name server**로 **redirect**한다.
3. **google.com name server**로 **redirect**한다.
4. 최종적으로 DNS기록에서 'www.google.com' 에 매칭되는 **IP주소 찾는다**.
5. 찾은 주소를 DNS recursor로 보낸다.

![image](https://user-images.githubusercontent.com/76507701/185777863-8645cfac-72c4-4679-ac37-fb87284c61e6.png)

## 3. Browser가 웹서버와 TCP connection을 한다.

- 브라우저가 올바른 IP 주소를 받게 되면 웹서버와 connection을 빌드하게 된다. 브라우저는 인터넷 프로토콜을 사용해서 웹서버와 연결이 된다. 인터넷 프로토콜의 종류는 여러가지가 있지만, 웹사이트의 **HTTP 요청의 경우에는 일반적으로 TCP를 사용**한다.

- 클라이언트와 웹서버간 데이터 패킷들이 오가려면 TCP connection이 되어야 한다. **TCP/IP three-way handshake**라는 프로세스를 통해서 클라이언트와 서버간 connection이 이뤄지게 된다. 클라이언트와 서버가 SYN(Synchronization)과 ACK(Acknowledgement)메세지들을 가지고 3번의 프로세스를 거친 후에 연결이 된다.

### TCP/IP three-way handshake 과정

1. 클라이언트 머신이 SYN 패킷을 서버에 보내고 connection을 열어달라고 요청한다.
2. 웹서버가 새로운 connection을 시작할 수 있는 포트가 있다면 SYN/ACK 패킷으로 대답을 한다.
3. 클라이언트는 SYN/ACK 패킷을 서버로부터 받으면 웹서버에게 ACK 패킷을 보낸다.

## 4. Browser가 웹서버에 HTTP 요청을 한다.

- 클라이언트의 브라우저는 **GET** 요청을 통해 웹서버에게 www.google.com 웹페이지를 요구한다. 요청을 할 때 form을 제출하는 상황에서는 POST 요청을 사용할 수도 있다.

- **browser identification**, **받아들일 요청의 종류**, **추가적인 요청을 위해 TCP connection 유지를 위한 정보**, 브라우저에서 얻은 **쿠키** 등의 정보를 담은 response를 보낸다.

```
GET / HTTP/1.1
Host: www.google.com
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:89.0) Gecko/20100101 Firefox/89.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Accept-Language: ko-KR,ko;q=0.8,en-GB;q=0.6,en-US;q=0.4,en;q=0.2
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
Upgrade-Insecure-Requests: 1
Cache-Control: max-age=0
TE: Trailers
```

## 5. 웹서버가 response를 생성한다.

1. 웹서버가 브라우저로부터 요청을 받으면, 페이지의 로직이나 DB의 연동을 위해 WAS(Web Application Server)에게 이들의 처리를 요청한다.
   - **웹서버** : **정적인 컨텐츠**(HTML, CSS, IMAGE 등)를 요청받아 처리
   - **WAS** : **동적인 컨텐츠**(JSP, ASP, PHP 등)를 처리 및 **DB**에서 필요한 데이터 **정보를 받아서 파일을 생성**(DB서버에 대한 접속 정보가 있기 때문에 외부에 노출 될 경우 보안상의 문제를 이유로 웹서버와의 연결을 통해 요청을 전달받는다.)
2. WAS가 **response**를 특정한 포맷으로(JSON, XML, HTML) **작성**한다.
3. 작업 결과를 웹서버로 전송한다.

## 6. 웹서버가 HTTP response를 보낸다.

- **요청한 웹페이지**, response의 **status code**, **content-encoding**, **쿠키** 등의 정보를 담은 response를 보낸다.

```
HTTP/2 200 OK
date: Sun, 21 Aug 2022 08:03:56 GMT
expires: -1
cache-control: private, max-age=0
content-type: text/html; charset=UTF-8
strict-transport-security: max-age=31536000
content-encoding: br
server: gws
content-length: 33633
x-xss-protection: 0
x-frame-options: SAMEORIGIN
alt-svc: h3-29=":443"; ma=2592000,h3-T051=":443"; ma=2592000,h3-Q050=":443"; ma=2592000,h3-Q046=":443"; ma=2592000,h3-Q043=":443"; ma=2592000,quic=":443"; ma=2592000; v="46,43"
```

## 7. Browser가 HTML content를 보여준다.

1. HTML의 스켈레톤을 렌더링한다.
2. HTML tag들을 체크한다.
3. 추가적으로 필요한 웹페이지 요소들을(이미지, CSS 스타일시트, Javascript 파일, 등) GET으로 요청한다. 이 정적인 파일들은 **브라우저에 의해 캐싱**이 되서 **나중에 해당 페이지를 방문할 때 다시 서버로부터 불러오지 않아도 된다**.
4. www.google.com이 화면에 출력된다.
   1. HTML 파일을 파싱 후 DOM 트리를 구축한다.
   2. CSS 파일을 파싱 후 CSSOM 트리 구축한다.
   3. Javascript 파일 (script 태그)를 만나면 Javascript를 실행한다. 만약, HTML 파일 중간에 script 태그가 있는 경우는 HTML 파싱을 중단하고 Javascript를 실행한다.
   4. DOM 트리와 CSSOM 트리를 조합해서 Render 트리를 구축한다.(**Reflow**)
   5. Viewport를 기준으로 각 노드가 가지고 있는 위치를 설정한다.(**Layout**)
   6. 위 과정에서 계산된 위치를 바탕으로 화면을 그린다.(**Paint**)

![image](https://user-images.githubusercontent.com/76507701/185779543-41e8bfb4-ad11-421b-8170-9c14b4a2a70f.png)

## 참고 사이트

https://velog.io/@rtyu1120/%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80%EC%97%90-google.com%EC%9D%84-%EC%B9%98%EB%A9%B4-%EB%AC%B4%EC%8A%A8-%EC%9D%BC%EC%9D%B4-%EC%9D%BC%EC%96%B4%EB%82%A0%EA%B9%8C-%EC%95%84%EB%8B%88-%EC%A7%84%EC%A7%9C%EB%A1%9C-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC%ED%8E%B8

https://velog.io/@eassy/www.google.com%EC%9D%84-%EC%A3%BC%EC%86%8C%EC%B0%BD%EC%97%90%EC%84%9C-%EC%9E%85%EB%A0%A5%ED%95%98%EB%A9%B4-%EC%9D%BC%EC%96%B4%EB%82%98%EB%8A%94-%EC%9D%BC

https://d2.naver.com/helloworld/59361
