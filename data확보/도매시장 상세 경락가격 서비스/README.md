# 쓸만한 변수 모음집

##### 2015년 기준으로 표준코드가 개편되었음

#### 종속변수

- sbidpric 거래가격

#### 변수

- stdunitnewcode 단위코드 

  연관코드

  - stdunitnewnm(단위명)

  종류

  단위명이 g되어있는 것도 있는데 확인이 필요함

  - 13 - ton
  - 12 - kg
  - 11 - g

- delngprut 거래단량

  매매단위명과 포장상태명을 조합하여 의미를 가지게 되는 숫자임

- delngQy 거래량

  상품의 거래 물량

- delngDe 경락일자

- whsalmrktcode 구시장코드

  연관코드

  - whsalmrktnewcode 시장코드
  - whsalmrktnewnm 시장명
  - whsalmrktnm 구시장명

  만약 오래전 데이터를 사용할수도 있을테니 이거 쓰는게 맞을듯

- stdspciescode 구품종코드
  연관코드

  - stdspciesnewcode 품종코드
  - stdspciesnewnm 품종명
  - stdspciesnm 구품종명

- stdqlitynewcode 등급코드
  종류

  - 11 특
  - 12 상
  - 13 보통
  - 14 4등
  - 15 5등
  - 16 6등
  - 17 7등
  - 18 8등
  - 19 등외
  - 1A 유기농산물
  - 1C 무농약
  - 1Z 무등급

- stdprdlstcode 구품목코드

  연관코드

  - stdprdlstnewcode 품목코드
  - stdprdlstnewnm 품목명
  - stdprdlstnm 구품목명

- cprmtccode 구산지코드

  연관코드

  - stdmtcnewcode 산지코드
  - stdmtcnewnm 산지명

  산지에 따라 작물이 영향을 받을 수 있을것 같아 집어넣음

##### 새롭게 만들 컬럼 목록

- 가격(sbidpric) / 거래단량(delngprut) = Kg당 가격(kgperprice)

  거래 단량은 단위당 몇 Kg인지 나타내는 단위이며

  가격은 거래단량으로 얼마가 팔렸는지 나타낸다

  그래서 Kg당 가격을 구하고 싶으면 가격 / 거래단량

  거래 단량은 단위당 몇 Kg인지 나타내는 단위이며

  가격은 거래단량으로 얼마가 팔렸는지 나타낸다

  그래서 Kg당 가격을 구하고 싶으면 가격 / 거래단량

- 총거래가격(totalprice)

  거래(delngQy)량 x 가격 하면 거래 가격이 나옴

- 총량(totalweight)

  거래단량 x 거래량

##### 사용하지 않은 컬럼 목록

- 법인코드는 결측치가 너무 많아서 뻈음

- 출하구분코드도 결측치가 꽤나 있어서 뻈음

---

## 처리할 코드 목록

- Kg당 가격(kgperprice)
  - 가격(sbidpric) / 거래단량(delngprut)
  - 거래단위(stdunitnewcode)가g(11)일경우 *1000, kg(12)일경우 그대로, ton(13)일경우 /1000
- 총거래가격(totalprice)
  - 거래량(delngQy) * 가격(sbidpric)
- 총량(totalweight)
  - 거래단량(delngprut) * 거래량(delngQy)
- 구품목코드(stdprdlstcode)
  - 현재 배추(1001)만
  - 추후 양파, 무, 마늘 등등 주요 민감 5개품목 관련해서도 진행
- 등급코드(stdqlitynewcode)
  - 일단 상(12)만 하는것으로 회의했었음
  - 하지만 작황이 좋으면 특(11) 안좋으면 보통(13) 또는 그 이외도 나올수 있기때문에 고려해봐야됨
- 구시장코드(whsalmrktcode)
  - 가락시장(110001)만

# 쓰게 될 코드 목록

- 종속변수

  sbidpric 거래가격 또는 Kg당 가격(kgperprice)

- 설명변수
  1. delngDe 경락일자
  2. cprmtccode 구산지코드

- 필요한 설명변수
  1. 가락시장에 반입된 배추량(공공기관 데이터와 맞지않아 알아보는중)
  2. 중국 수입량(확보 필요)
  3. 배추 비축량 데이터(확보 필요)
  4. 소비자물가지수
  
- 유추가능한 변수들

  1. 평년 가격
  2. 평년 반입량
  3. 전일?전주? 반입량
  4. 전일?전주? 평균가격, 15 30 60 이평선

---

### 사용할만한 주제

1. 당일 입하량에 따른 가격 예측

2. 다른 품종도 추가하여 가격 예측

3. 시간별 추세에 따른 미래 가격 예측

   폭등, 폭락에 대한 조건들을 파악해서 평년 기준 파악
   이쯤에는 이만큼 등락이 있었는데 어떤 요인에 의해 작용을 했으며 이 추세 토대로 앞으로의 가격예측 조사

### 사용중인 OPEN API

1. 농수축산물 도매시장 상세 경락가격 서비스
   - http://apis.data.go.kr/B552895/openapi/service/OrgPriceAuctionService
2. 가락시장 OPEN API(농산물 주요품목 가격)
   - <http://www.garak.co.kr/gongsa/jsp/gs/data_open/list.jsp?kind=22>

### 관련 OPEN API

- 수산물 위판 정보
- 기관조사가격서비스
- 실시간경매속보서비스
- 과수지재배변동정보
- 어류양식통계
- 과실류 가공현황
- 집단급식소 현황
- 충청남도_주요 농축산물 주간 가격동향 정보
- 충청남도 물가정보
- 냉동선어류입출하내역
- 어업생산동향정보
- 농장 HACCP 인증업체 현황
- 축산물 HACCP 인증업체 상세정보
- 축산물 유통 HACCP업체 현황
- 사료제조 HACCP업체 현황
- 수산물수출검사통계
- 수산물품질인증현황
- 중국 도매시장 가격정보
- 해양수산주요통계



#### 다양한 의견들

- 배추 등급에 따른 가격 결정 부분도 고려
- 국가의 농업정책 조사 필요
- 배추 등급을 상 등급 하나로 가고자 하였으나, 시간이 지남에 따라 농산물 등급에 변화가 존재하여 위 파일 및 사이트(농협 공판장 인터넷통합거래 사이트)를 참고하는것을 권유받음
- 월 단위 가격 책정 후 국가 정책 유도를 목표로 갈 수 있음
- 정책만들기 전에 제시할 수 있는 분석(물가안정을 목표로)