#### 실습 소스

<br>

#### Google RSS 피드 주소 : 주식뉴스
``` https://news.google.com/rss/search?q=%EC%A3%BC%EC%8B%9D&hl=ko&gl=KR&ceid=KR%3Ako ```
<br>

```
{{ $today.format("DD") }}의 뉴스
```
<br>


```
오늘의 뉴스요약
===================
{{ $json.data.map(item => `${item.title}\n${item.link}`).join('\n\n') }}
```
<br>

#### Discord Parameters의 Message 설정 :
```
{{ $json.dt.toDateTime('s').format('yyyy-MM-dd HH:mm') }} 기준 날씨 리포트

📍 현재 서울 날씨
- 상태: {{ $json.weather[0].description }}
- 기온: {{ $json.main.temp }}°C (체감 {{ $json.main.feels_like }}°C)
- 일출: {{ $json.sys.sunrise.toDateTime('s').format('HH:mm') }} / 일몰: {{ $json.sys.sunset.toDateTime('s').format('HH:mm') }}

🌥️ 내일 예보 (오전 6시)
- 상태: {{ $json.list[8].weather[0].description }}
- 기온: {{ $json.list[8].main.temp }}°C (체감 {{ $json.list[8].main.feels_like }}°C)
```

<br>

```
너는 사용자의 질문에 따라 KAMIS api를 이용하여 현재 농수산물에 대한 정보를 알려 주는 장바구니 AI Agent야.
사용자의 농수산품 질문에 따라 구글 시트에서 적절한 코드를 찾고 이를 이용하여 현재 가격과 최근 가격의 추이를 정리해 줘.

요즘, 최근이라고 하면 {{ $now }} 날짜 기준으로 하루 전 데이터를 조회해.
```
