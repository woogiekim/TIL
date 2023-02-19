## DNS

www.naver.com 라는게 도메인 주소가 있다고 봤을때

www -> www.naver -> www.naver.com
순으로 되어있는 것임

- 여기서 naver.com 라는 것이 도메인 이름
- www 라는 것이 호스트 네임

- 컴퓨터가 ISP에게 물어봄 www.naver.com 아이피 주소 뭐야 알려줘 라고 물어봄
- 그러면 아이피주소를 받게 되는게 중요한 것은 유효기간도 함께 받음
- 받은 유효기간안에서만 캐시된것을 사용하는 것이 유효하고 그 다음은 다시 알아봄

## 분산구조

- Root DNS라는 것이 존재
  - DNS를 위한 DNS
  - https://iana.org

Root야 .com 니가 잘 알아지 알려줘 -> naver 이거 알어? 알려줘 -> 야 그러면 니네집에 호스트가 www 인거 있음? 알려줘 -> ip주소 짜잔

- 이렇게 DNS는 분산되어 있고 알고있는 애들한테 물어 물어 봄

## 알아두면 좋은 것

- 1.25 대란
  - 대한민국 DNS가 공격 받아 다운된 것 
  - https://namu.wiki/w/1.25%20인터넷%20대란
  - https://ko.wikipedia.org/wiki/1·25_인터넷_대란
- DNS 캐시하는 상황
  - DNS Cache를 통해 -> 유효기간이 있고 유효기간 내에 있을 때에는 ISP에 물어보지 않음
  - hosts 파일을 통해
  - 공유기를 통해