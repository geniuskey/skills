# 상품 검색 API (ItemSearch)

## 엔드포인트

```
GET http://www.aladin.co.kr/ttb/api/ItemSearch.aspx
```

## 요청 파라미터

### 필수

| 파라미터 | 타입 | 설명 |
|---------|------|------|
| `ttbkey` | string | TTBKey |
| `Query` | string | 검색어 |

### 옵션

| 파라미터 | 타입 | 기본값 | 설명 |
|---------|------|--------|------|
| `QueryType` | string | `Keyword` | 검색어 종류: `Keyword`(제목+저자), `Title`(제목), `Author`(저자), `Publisher`(출판사) |
| `SearchTarget` | string | `Book` | 검색 대상: `Book`, `Foreign`, `Music`, `DVD`, `Used`(중고샵), `eBook`, `All` |
| `Start` | int | `1` | 시작 페이지 (1 이상) |
| `MaxResults` | int | `10` | 페이지당 결과 수 (1~100) |
| `Sort` | string | `Accuracy` | 정렬: `Accuracy`(관련도), `PublishTime`(출간일), `Title`(제목), `SalesPoint`(판매량), `CustomerRating`(고객평점), `MyReviewCount`(마이리뷰수) |
| `Cover` | string | `Mid` | 표지 크기: `Big`(200px), `MidBig`(150px), `Mid`(85px), `Small`(75px), `Mini`(65px), `None` |
| `CategoryId` | int | `0` | 분야 제한 (0=전체) |
| `Output` | string | `xml` | 출력: `xml`, `js`(JSON) |
| `Partner` | string | - | 제휴 파트너코드 |
| `includeKey` | int | `0` | 1이면 링크에 TTBKey 포함 |
| `InputEncoding` | string | `utf-8` | 검색어 인코딩 (`utf-8`, `euc-kr`) |
| `Version` | int | `20070901` | API 버전 (최신: `20131101`) |
| `outofStockfilter` | int | `0` | 1이면 품절/절판 제외 |
| `RecentPublishFilter` | int | `0` | 출간일 필터(월 단위, 0~60). 1이면 최근 1개월 내 출간 |
| `OptResult` | string | - | 부가정보 (콤마 구분): `ebookList`, `usedList`, `fileFormatList` |

## 요청 예시

```
http://www.aladin.co.kr/ttb/api/ItemSearch.aspx?ttbkey=[TTBKey]&Query=aladdin&QueryType=Title&MaxResults=10&start=1&SearchTarget=Book&output=js&Version=20131101
```

## 제한

- 한 페이지 최대 100개
- 총 결과 200개까지만 검색 가능
