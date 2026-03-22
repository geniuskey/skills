# 중고 판매자 상세 데이터 (웹스크래핑)

알라딘 API의 `OptResult=usedList`는 중고 판매 요약(수량, 최저가)만 제공한다. 판매자별 상세 정보(개별 가격, 배송비, 도서 상태, 등급)는 API로 제공되지 않으므로 웹페이지 HTML 파싱이 필요하다.

## 대상 URL

```
https://www.aladin.co.kr/shop/UsedShop/wuseditemall.aspx?ItemId={itemId}&TabType={tabType}&page={page}
```

| 파라미터 | 설명 |
|---------|------|
| `ItemId` | 도서 ItemId (API 검색 결과에서 획득) |
| `TabType` | 0=전체, 1=알라딘 직배송, 2=회원 직배송 |
| `page` | 페이지 번호 (1부터) |

## 필수 헤더

알라딘은 User-Agent 없이 요청하면 차단할 수 있다:

```typescript
const HEADERS = {
  "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 ...",
  "Referer": "https://www.aladin.co.kr/",
  "Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8",
  "Accept-Language": "ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7",
};
```

## HTML 파싱 구조

판매자 목록은 `class="Ere_usedsell_table"` 테이블의 `<tbody>` 안에 있다.

```
table.Ere_usedsell_table > tbody > tr (각 판매자)
```

`sell_tableTF` 클래스를 포함한 행은 헤더이므로 스킵한다.

### 추출 가능한 데이터

| 데이터 | CSS 셀렉터/패턴 | 설명 |
|--------|----------------|------|
| 판매자 이름 | `class="seller"` 내 `<a>` 태그 텍스트 | |
| 판매자 링크 | `class="seller"` 내 `<a>` href | 프로필 페이지 |
| 판매자 등급 | `Ere_used_fresh/power/gold/pro` 클래스 | 새내기/파워/골드/전문셀러 |
| 도서 가격 | `class="Ere_fs20"` 내 숫자 | 원 단위 |
| 도서 상태 | `Ere_sub_high/middle/low` 클래스 | 최상/상/중/하 |
| 배송비 | `배송비\s*:\s*([\d,]+)원` 패턴 | price 셀 내 |
| 반값택배 | `반값택배\s*:\s*([\d,]+)원` 패턴 | price 셀 내 |
| 무료배송 기준 | `([\d,]+)원\s*이상\s*무료` 패턴 | price 셀 내 |
| 상품 링크 | `wproduct.aspx.*ItemId=(\d+)` | 중고 상품 상세페이지 |

### 총 페이지 수

HTML에서 `/ {N}페이지` 패턴으로 추출:

```typescript
const match = html.match(/\/\s*(\d+)페이지/);
const totalPages = match ? parseInt(match[1], 10) : 1;
```

## 속도 제한 및 동시성

과도한 요청은 IP 차단될 수 있으므로 적절한 제한이 필요하다:

| 설정 | 권장값 | 설명 |
|------|--------|------|
| 페이지 간 딜레이 | 300ms | 같은 도서의 다음 페이지 |
| 배치 간 딜레이 | 700ms | 다른 도서 배치 사이 |
| 동시 요청 수 | 3개 | Promise.allSettled로 배치 처리 |
| 도서당 최대 페이지 | 2 | 대부분의 판매자는 1~2페이지에 포함 |

## 결과 데이터 구조

```typescript
interface SellerData {
  sellerName: string;           // 판매자 이름
  sellerGrade: string;          // 새내기/파워/골드/전문셀러
  price: number;                // 도서 가격
  condition: string;            // 최상/상/중/하
  shippingFee: number;          // 기본 배송비 (보통 3,000~4,000원)
  halfDeliveryFee: number | null;      // 반값택배 가격
  freeShippingThreshold: number | null; // 무료배송 기준금액
  link: string;                 // 판매자 프로필 링크
  productLink?: string;         // 중고 상품 상세페이지
  usedItemId?: number;          // 중고 상품 ID
}
```

## 배송비 최적화 활용

이 데이터의 핵심 가치는 **묶음배송 최적화**다. 여러 권을 구매할 때:
- 각 판매자가 보유한 도서 목록을 매칭
- 한 판매자에서 많은 책을 사면 배송비를 한 번만 부담
- 무료배송 기준을 넘으면 배송비 0원
- 총비용(도서가격 합 + 배송비)이 최소인 판매자 조합을 찾는 것이 목표
