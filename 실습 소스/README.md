#### 실습 소스
<br>

#### Upstage API 인증 URL :
```
Solar url : https://api.upstage.ai/v1/solar
Document OCR url : https://api.upstage.ai/v1/document-digitization
```

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

#### 장바구니 AI Agent Description :
```
Google Sheets에서 찾은 부류코드
예: 딸기면 200, 고구마면 100 처럼  "숫자 + 00"의 형태
```
<br>

#### 장바구니 AI Agent System Message :
```
너는 사용자의 질문에 따라 KAMIS api를 이용하여 현재 농수산물에 대한 정보를 알려 주는 장바구니 AI Agent야.
사용자의 농수산품 질문에 따라 구글 시트에서 적절한 코드를 찾고 이를 이용하여 현재 가격과 최근 가격의 추이를 정리해 줘.

요즘, 최근이라고 하면 {{ $now }} 날짜 기준으로 하루 전 데이터를 조회해.
```

#### AI 영수증 정리봇 System Message :
```
3가지를 정제해

1. 구매 날짜(시분초)
2. 카드번호
3. 총금액
```
<br>

### JSON Example :
```
{
    "timestamp": "YYYY-MM-DD HH:MM:SS",
	"amt": "총 금액",
	"card": "카드번호"
}
```
<br>

### 주식 시세 — AI 투자 리포트 메일링
<br>

#### Gemini 입력 프롬프트 -1 (지수시세정보) :
```
n8n의 HTTP Request 노드를 이용해 코스피지수를 가져올 거야.
첨부된 자료를 이용해서 코스피지수를 가져오기 위한 HTTP 요청 정보를 cURL 형식으로 만들어 줘.
```
<br>

```
curl -X GET "https://apis.data.go.kr/1160100/service/GetMarketIndexInfoService/getStockMarketIndex?serviceKey=YOUR_SERVICE_KEY&numOfRows=1&pageNo=1&resultType=json&idxNm=%EC%BD%94%EC%8A%A4%ED%94%BC"
```

#### Gemini 입력 프롬프트 -2(주식시세정보) :
```
n8n의 HTTP Request 노드를 이용해 주식 종목 가격을 가져올 거야.
종목명은 {{$json.name}}에 있어. 첨부된 자료를 이용해서 종목 가격을 가져오기 위한 http 요청을 cURL 형식으로 만들어줘.
```
<br>

```
curl -X GET "https://apis.data.go.kr/1160100/service/GetStockSecuritiesInfoService/getStockPriceInfo?serviceKey=YOUR_SERVICE_KEY&numOfRows=1&pageNo=1&resultType=json&itmsNm={{$json.name}}"
```
<br>


#### Upstage API URL :

```
Solar url : https://api.upstage.ai/v1/solar
Document OCR url : https://api.upstage.ai/v1/document-digitization
```
