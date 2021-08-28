#### 1. http?
- http는 www 상에서 정보를 주고받을 수 있는 프로토콜
- 주로 html 문서를 주고받는데 쓰임
- tcp 80번 포트 사용
- 전달되는 자료는 http:로 시작하는 url(인터넷 주소)로 조회

> * 클라이언트인 웹 브라우저가 http를 통하여 서버로부터 웹페이지나 그림을 요청
> * 서버는 이 요청에 응답하여 필요한 정보를 해당 사용자에게 전달



#### 2. http 프로토콜
* 요청 내용에 포함되는 요청 메소드

> get : url에 해당하는 자료의 전송을 요청
  head : get과 같은 요청이지만 자료에 대한 정보(meta-information)만을 받음
  post : 서버가 처리할 수 있는 자료 전송
  
  
* 요청에 대한 서버 응답

> Header와 Body로 나누어져서, Header는 상태를 넣고, 요청한 내용을 body에 담아서 전송한다.


#### 3. HTML 구조
* HTML은 정형화된 구조가 없어서 어렵다 !
* HTML은 Head와 Body로 나누어진다 !
![](https://images.velog.io/images/hm1lee/post/67b454e5-acdb-4d7d-86dc-8acae62ed28d/image.png)



#### 4. Beautiful Soup을 사용한 HTML 분석

<Beautiful soup 이란?>
- 루이스 캐럴의 "이상한 나라의 앨리스에 나오는 동명의 시에서 이름을 지음"
- 이상한 HTML(구성이 정형화 되어 있지 않음)을 분석할 수 있는 기능을 갖춤
- 파싱을 규칙적으로 하기가 많이 어렵지만, beautiful soup은 가능해 !
- 설치: **pip install beautifulsoup4** (기본적으로 아나콘다에 설치되어 있음)



<Beutiful soup 객체 사용하기>
- urlopen 함수를 사용하여 가져온 html 파일을 분석
- bsObj.html을 사용하면 html 부분을 가져올 수 있음
- bsObj.html뿐만 아니라 다양한 객체를 불러올 수 있음

![](https://images.velog.io/images/hm1lee/post/ea6b0728-02a2-4e16-82db-f47ef8735f56/image.png)

![](https://images.velog.io/images/hm1lee/post/5024c8fe-7748-4122-8a9a-66362bf2d097/image.png)

* 사이트 div-text 데이터 str에 저장
* 파일 만들어서 webbrowser로 렌더링
![](https://images.velog.io/images/hm1lee/post/b92852d6-0512-4983-b7d2-39702e827812/image.png)



<find와 findAll 활용하기>
- 원하는 태그를 검색하는 기능
- 앞서 사용한 "."과 유사한 기능

![](https://images.velog.io/images/hm1lee/post/ed01fed2-2c2b-42d7-8f00-3c8b3a4a8957/image.png)


#### 5. 하이퍼링크 조사

* href 속성은 링크의 목적지를 지정
```
<a href = "https://www.naver.com"> hello </a>
```


* 하이퍼링크 : 웹 사이트는 수많은 하이퍼링크의 연결이다!
![](https://images.velog.io/images/hm1lee/post/31c02ac8-9957-4486-80fc-ae72d697b2c1/image.png)


<하이퍼링크의 종류>

1) http://othersite.com
2) http://www.naver.com/~

![](https://images.velog.io/images/hm1lee/post/d3263266-1a45-4bca-8b7e-649905faf937/image.png)

3) /~

![](https://images.velog.io/images/hm1lee/post/9f2b6b4b-f384-4fac-b828-7a7bca668643/image.png)


4) 기타 (#~ 등)




#### 6. Robots.txt

웹 크롤링을 하기 전에 알아두어야 할 것!
* 웹 사이트 root에는 Robots.txt가 존재
* 웹 사이트에서 크롤링되어선 안되는 부분을 표시함으로 웹 보안을 유지
* disaloow한 항목들은 들어가서는 안됨
* 마케팅이냐, 보안이냐? 양면성을 따져서 설정해야 함


보안을 중요시하는 "네이버"
-> 모든 사이트에 대해서 검색을 못하도록 하겠다.

홍보를 중요시하는 "백악관"
-> disallow 항목도 있지만, allow하는 부분도 존재함을 확인해볼 수 있음


