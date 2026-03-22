---
name: aladin-openapi
description: "Aladin (알라딘) Open API integration guide for Korean book search, book lists, item lookup, and used book store search. Use this skill whenever the user works with the Aladin bookstore API, mentions 알라딘 API, TTBKey, book search API in Korean context, or builds features that query Aladin's catalog (search, bestsellers, used books, ISBN lookup). Also trigger when code imports or references aladin API endpoints like ItemSearch, ItemList, ItemLookUp, or ItemOffStoreList."
---

# Aladin Open API

알라딘 Open API를 사용하여 도서 검색, 리스트 조회, 상품 조회, 중고매장 검색을 구현하는 가이드.

## Base URL

```
http://www.aladin.co.kr/ttb/api/
```

> 알라딘 API는 HTTP만 공식 지원. HTTPS(`https://`)로 요청하면 일부 환경(Cloudflare Workers 등)에서 실패할 수 있으므로 `http://`를 사용할 것.

## 인증

모든 API 요청에 `ttbkey` 파라미터가 필수. 알라딘 Open API 페이지에서 TTBKey를 발급받아 사용한다.

## API 목록

| API | 엔드포인트 | 설명 |
|-----|-----------|------|
| 상품 검색 | `ItemSearch.aspx` | 키워드로 도서/음반/DVD 검색 |
| 상품 리스트 | `ItemList.aspx` | 신간/베스트셀러/편집자추천 리스트 |
| 상품 조회 | `ItemLookUp.aspx` | ISBN 또는 ItemId로 개별 상품 상세 조회 |
| 중고매장 검색 | `ItemOffStoreList.aspx` | 중고상품 보유 오프라인 매장 검색 |

## 공통 파라미터

| 파라미터 | 필수 | 기본값 | 설명 |
|---------|------|--------|------|
| `ttbkey` | O | - | 발급받은 TTBKey |
| `Output` | - | `xml` | `xml` 또는 `js` (JSON) |
| `Version` | - | `20070901` | API 버전. 최신: `20131101` |
| `MaxResults` | - | `10` | 페이지당 결과 수 (1~100) |
| `Start` | - | `1` | 시작 페이지 |
| `Cover` | - | `Mid` | 표지 크기: `Big`(200px), `MidBig`(150px), `Mid`(85px), `Small`(75px), `Mini`(65px), `None` |
| `CategoryId` | - | `0` | 분야 제한 (0=전체) |
| `Partner` | - | - | 제휴 파트너코드 |
| `includeKey` | - | `0` | 1이면 링크에 TTBKey 포함 |

## 제한사항

- 한 페이지에 최대 100개 (`MaxResults` 최대값)
- 총 결과는 200개까지만 검색/조회 가능 (페이지네이션 한계)
- API는 HTTP 프로토콜만 공식 지원

## 구현 패턴

### JSON 응답 받기

`Output=js`로 요청하면 JavaScript/JSON 형식 응답을 받을 수 있다.

```typescript
const params = new URLSearchParams({
  ttbkey: TTB_KEY,
  Query: "검색어",
  QueryType: "Title",
  MaxResults: "10",
  start: "1",
  SearchTarget: "Book",
  output: "js",
  Version: "20131101",
});

const res = await fetch(`http://www.aladin.co.kr/ttb/api/ItemSearch.aspx?${params}`);
const data = await res.json();
```

### 중고상품 정보 조회

`OptResult=usedList`를 추가하면 중고 가격/수량 정보를 함께 받을 수 있다.

```typescript
const params = new URLSearchParams({
  ttbkey: TTB_KEY,
  ItemId: isbn13,
  itemIdType: "ISBN13",
  output: "js",
  Version: "20131101",
  OptResult: "usedList",
});

const res = await fetch(`http://www.aladin.co.kr/ttb/api/ItemLookUp.aspx?${params}`);
```

### 에러 핸들링

알라딘 API는 에러 시에도 HTTP 200을 반환한다. 응답 body에 `errorCode` 필드가 있으면 에러이며, 이때 `item` 배열은 없다. 에러 판별은 `"errorCode" in data && !data.item` 패턴을 사용한다.

대표적인 에러:
- **errorCode 31**: 잘못된 파라미터 조합. `SearchTarget=Used`나 `OptResult=usedList`가 특정 키워드에서 발생시킨다.

### 검색 폴백 전략 (실전 필수)

`SearchTarget=Used`로 검색하면 일부 키워드(예: "레디 플레이어 투")에서 errorCode 31이 발생한다. 중고도서 검색 서비스를 만들 때는 다단계 폴백 체인이 필수:

```
1차: SearchTarget=Used + OptResult=usedList (최적)
  ↓ errorCode 발생 시
2차: SearchTarget=Used (OptResult 제거)
  ↓ errorCode 발생 시
3차: SearchTarget=Book (새 책 검색이지만 OptResult=usedList로 중고 정보도 포함됨)
  ↓ errorCode 발생 시
4차: 키워드 축소 (3단어 이상 → 첫+끝 단어, 2단어 → 첫 단어만)
```

`SearchTarget=Book`으로 검색해도 `OptResult=usedList`를 함께 요청하면 중고 판매 정보(수량, 최저가)가 응답에 포함되므로, 중고도서 서비스에서도 유용하다.

### 커버 이미지 고해상도

API 응답의 `cover` URL은 기본적으로 작은 이미지다. URL 경로에서 `coversum` 또는 `cover\d+` 부분을 `cover500`으로 치환하면 고해상도 이미지를 얻을 수 있다.

```typescript
const largeCover = cover.replace(/cover(sum|\d+)/, "cover500");
```

### 제휴 링크 (Partner / includeKey)

`Partner` 파라미터로 파트너코드를, `includeKey=1`로 TTBKey를 링크에 포함시켜 제휴 실적을 추적할 수 있다. 중고 상품 상세 링크에도 파트너 파라미터를 붙일 수 있다:

```
https://www.aladin.co.kr/shop/wproduct.aspx?ItemId={usedItemId}&partner={ttbkey}
```

### 캐싱 권장사항

알라딘 API는 호출 횟수 제한이 있으므로 서버 사이드 캐싱을 권장한다:
- 검색 결과: 5분 TTL
- 판매자 데이터: 10분 TTL
- 인메모리 Map 기반으로 충분 (소규모 서비스)

### Edge Runtime 배포

Cloudflare Workers/Pages 등 Edge Runtime에서 알라딘 API를 호출할 때:
- 반드시 `http://` 사용 (HTTPS 미지원)
- `export const runtime = "edge"` 설정
- fetch API는 Edge Runtime에서 기본 지원

## 상세 API 레퍼런스

각 API의 전체 파라미터와 응답 필드는 레퍼런스 파일에서 확인:

- **상품 검색 API** → `references/item-search.md`
- **상품 리스트 API** → `references/item-list.md`
- **상품 조회 API** → `references/item-lookup.md`
- **중고매장 검색 API** → `references/item-offstore.md`
- **응답 필드 공통** → `references/response-fields.md`
- **중고 판매자 스크래핑** → `references/used-seller-scraping.md` (API로 제공되지 않는 판매자별 상세 데이터)

필요한 API의 레퍼런스 파일을 읽어서 상세 파라미터와 응답 구조를 확인하라.
