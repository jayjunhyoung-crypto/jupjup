# 줍줍 데이터 업데이트 가이드

매일 자정(Asia/Singapore) 예약 작업이 이 문서를 따라 `data.json`을 갱신한다.
이 작업에서 고치는 파일은 `data.json` 하나뿐이다. `index.html`, `version.json` 등 다른 파일은 건드리지 않는다.
사람이 지켜보지 않는 작업이므로 질문하지 말고, 애매하면 이 문서 기준으로 보수적으로 판단해서 끝까지 진행한다.

## 1. 날짜 기준
- 오늘 = 실행 시점의 Asia/Singapore 날짜(YYYY-MM-DD). 새 항목의 `added`에 이 날짜를 쓴다.
- 이벤트 기간은 주최 측이 공지한 날짜(한국 이벤트는 한국 시간 기준) 그대로 `start`, `end`에 쓴다.

## 2. 정리 (먼저 할 일)
- `end`가 오늘보다 이전인 `deals` 항목은 삭제한다.
- `type`이 `info`이고 `end`가 없는 항목은 `added`로부터 14일이 지나면 삭제한다.
- 공식 페이지에서 조기 종료나 예산 소진이 확인된 항목도 삭제한다.
- `daily` 항목은 서비스가 끝났거나 링크가 바뀐 게 확인될 때만 고친다.

## 3. 새 혜택 찾기
우선순위
1. `free` 돈을 쓰거나 가입하지 않고 누구나 받는 포인트, 쿠폰, 바우처 (클릭 적립, 출석, 퀴즈, 무료 쿠폰, 정부·지자체 무료 지원)
2. `entry` 응모만 하면 되는 추첨 (구매나 카드 보유가 필요하면 `condition`에 적는다)
3. `cond` 결제, 카드 발급, 가입이 필요한 적립·할인·캐시백
4. `info` 마일리지 소멸, 제도 변경, 사칭 주의처럼 알아두면 손해를 막는 소식

카테고리 `cat` 값: `air`, `toss`, `npay`, `kakao`, `card`, `shop`, `gov`, `sg`, `etc`

지역 `region` 값 (모든 항목에 반드시 넣는다. 앱에서 한국/싱가포르 버튼으로 거르는 기준이다)
- `kr` 한국 계정이나 한국 매장·사이트에서만 받을 수 있는 것 (토스, 네이버페이, 한국 카드사, 대한항공 한국 프로모션 등)
- `sg` 싱가포르 거주자나 싱가포르 계정 대상 (gov.sg 바우처, Shopee·Lazada SG, KrisFlyer, 싱가포르 은행·카드 등)
- `global` 어느 나라에서든 받을 수 있는 것 (드물다. 확실할 때만)

받을 수 있는 사람 (앱 사용자는 한국인이고, 지금은 주로 싱가포르에 사는 한국인이다)
- `region`이 `kr`인 항목에는 `abroad`를 넣는다. 해외(싱가포르)에 있어도 앱이나 온라인으로 받을 수 있으면 `true`, 한국 오프라인 매장에 가야 하거나 국내 거주·소득·학생·수급자 요건이 있으면 `false`.
- `region`이 `sg`인 항목에는 `citizen_only`를 넣는다. 싱가포르 국민만 받을 수 있으면(CDC 바우처, SG60, Assurance Package 등) `true`, 외국인(EP·DP 등 거주자)도 받을 수 있으면 `false`. 영주권자 조건 같은 세부 내용은 `condition`에 쓴다.
- 앱은 싱가포르에 있을 때 `citizen_only: true`와 `abroad: false` 항목을 기본으로 숨긴다. 그래서 이 두 값은 정확하게 넣는다.

지역 균형
- 한국 항목만 몰리지 않게, 매일 싱가포르 항목도 따로 찾아본다 (있으면 1~3개 목표). 특히 외국인도 받을 수 있는 싱가포르 혜택을 우선한다.

매번 훑을 곳
- 토스: toss.im/notices, toss.im/tossfeed, "토스 이벤트 포인트"
- 네이버페이: "네이버페이 이벤트", "네이버페이 포인트 적립 이벤트"
- 카카오페이: "카카오페이 이벤트 포인트"
- 항공: 대한항공 프로모션(koreanair.com/contents/promotion), 아시아나 이벤트, KrisFlyer 프로모션(singaporeair.com)
- 카드사: 신한, 현대, 삼성, KB국민, 하나, 우리, 롯데, BC의 "응모 이벤트"
- 쇼핑: 올리브영, 쿠팡, 무신사, 지마켓, 11번가의 무료 쿠폰·포인트 이벤트
- 정부: 한국 정부·지자체 지원금이나 환급 신청 마감, 싱가포르 gov.sg·ask.gov.sg 바우처·지원금
- 싱가포르: KrisFlyer 프로모션(singaporeair.com의 kf-promotions, kf-miles-paradise), Shopee SG·Lazada SG 이벤트(help.shopee.sg 등), Grab, ShopBack, FairPrice, DBS·OCBC·UOB 같은 은행과 통신사의 무료 리워드, Mothership·TheSmartLocal·MoneySmart 같은 현지 매체의 무료 혜택 기사

규칙
- 공식 출처(회사 공지, 프로모션 페이지, 보도자료, 정부 사이트)를 우선한다. 커뮤니티 글은 단서로만 쓰고, 공식 페이지에서 기간과 조건을 확인한 뒤 넣는다. 확인이 안 되면 넣지 않는다.
- 예외: 네이버페이 클릭 적립처럼 커뮤니티가 그날그날 모아 올리는 무료 링크는, 그날 모음 글 하나만 `free` 항목으로 넣을 수 있다 (`id`는 `npay-clicks-YYYYMMDD`, `end`는 오늘, 개별 링크는 옮겨 적지 않는다).
- 추천인·레퍼럴 링크, 제휴 수수료 링크, 단축 URL, 사칭이 의심되는 사이트, 개인정보를 과하게 요구하는 사이트는 넣지 않는다.
- 원문을 그대로 베끼지 말고 자기 말로 짧게 요약한다. `desc`는 두 문장 이내, `condition`은 한 문장.
- 말투는 앱 화면에 맞게 짧은 해요체로 쓴다.
- 하루 새 항목은 3~10개가 목표지만 품질이 우선이다. 쓸 만한 게 없으면 적게 넣어도 된다.
- 이미 있는 이벤트와 같은 거면 새로 만들지 말고 필요한 내용만 고친다 (`added`는 그대로 둔다).
- `deals`는 최대 40개까지 유지한다. 넘치면 마감이 먼 `cond`부터 뺀다.

## 4. 항목 형식

```json
{
  "id": "kal-cashandmiles-2611",
  "cat": "air",
  "region": "kr | sg | global",
  "type": "free | entry | cond | info",
  "title": "짧은 제목",
  "reward": "받는 것 한 줄 (info면 생략 가능)",
  "desc": "무슨 혜택인지 두 문장 이내",
  "condition": "대상이나 조건 한 문장 (없으면 생략)",
  "points": ["info에서 꼭 알아야 할 것 몇 줄 (선택)"],
  "start": "YYYY-MM-DD (모르면 생략)",
  "end": "YYYY-MM-DD (마감일 또는 info의 핵심 날짜, 상시면 생략)",
  "badge": "end가 없을 때 날짜 칸에 보일 짧은 말 (예: 11/13부터, 주의, 소진 시)",
  "added": "YYYY-MM-DD",
  "url": "참여하거나 확인하는 https 링크",
  "source_url": "url과 다른 출처가 있으면 (선택)",
  "cta": "버튼 문구 (예: 응모하러 가기, 이벤트 보기, 공지 보기)"
}
```

- `id`는 영문 소문자, 숫자, 하이픈만 쓴다. 날짜나 연월을 붙여서 겹치지 않게 한다.
- `url`은 반드시 `https://`로 시작해야 한다.
- `kr` 항목은 `"abroad": true | false`, `sg` 항목은 `"citizen_only": true | false`를 같이 넣는다.
- 앱은 한국어와 영어를 지원한다. 모든 항목에 `en` 객체를 넣고, 화면에 보이는 글자 필드(`title`, `reward`, `desc`, `condition`, `points`, `badge`, `cta`)를 자연스러운 영어로 옮긴다. 예: `"en": {"title": "...", "desc": "...", "cta": "View promotion"}`. 원화 금액은 ₩로 쓴다.

## 4-1. 생활 혜택 (`guides`)
기간 한정이 아니라 늘 쓸 수 있는 할인 앱과 제도다 (Eatigo, Chope, Burpple Beyond, MRT 아침 할인 등). 한국인이 잘 모르는 현지 혜택을 쉽게 알려주는 게 목적이다.
- 매일 새로 채울 필요는 없다. 서비스가 끝났거나 가격·조건이 크게 바뀐 게 확인되면 고치거나 지운다.
- 싱가포르에 사는 한국인과 외국인에게 확실히 쓸모 있는 상시 혜택을 새로 찾으면 하루 최대 3개까지 추가한다 (공식 링크 필수). 외국인도 쓸 수 있는 것을 우선한다.
- 생활 혜택에도 `en` 객체(`title`, `tagline`, `saving`, `cost`, `desc`, `how`, `tip`, `cta`)를 넣는다.
- `group` 값: `dining`(외식 할인), `leftover`(마감·잉여 음식), `grocery`(장보기), `cashback`(캐시백), `transport`(교통), `free`(무료 즐길거리), `finance`(환전·송금·돈 관리), `life`(통신·구독 등 그 밖의 생활)

```json
{
  "id": "sg-eatigo",
  "region": "sg",
  "citizen_only": false,
  "group": "dining",
  "icon": "⏰",
  "title": "서비스 이름",
  "tagline": "한 줄 설명",
  "saving": "얼마나 아끼는지",
  "cost": "무료 또는 유료 금액",
  "desc": "두 문장 이내 설명",
  "how": ["쓰는 법 1", "쓰는 법 2"],
  "tip": "알아두면 좋은 점 (선택)",
  "url": "앱스토어나 공식 사이트 https 링크",
  "cta": "버튼 문구",
  "source_url": "참고한 기사 (선택)",
  "added": "YYYY-MM-DD"
}
```

## 5. 저장
1. `python3 -m json.tool data.json > /dev/null` 로 유효성을 확인한다.
2. `updated`를 실행 시각(ISO 8601, `+08:00`)으로 바꾼다.
3. 커밋 메시지는 `data: YYYY-MM-DD 혜택 업데이트 (+추가 n, -삭제 m)` 형식으로 쓰고, 세션 안내에 있는 attribution 줄을 붙인다.
4. `main`에 push 한다.
5. 끝나면 추가·삭제한 항목을 두세 줄로 요약한다.
