# 응답(Response) 필드 - 공통

상품 검색, 상품 리스트, 상품 조회 API의 공통 응답 구조.

## 최상위 필드

| 필드 | 설명 | 타입 |
|------|------|------|
| `version` | API 버전 | int (날짜형) |
| `title` | API 결과 제목 | string |
| `link` | 관련 알라딘 페이지 URL | string(URL) |
| `pubDate` | API 출력일 | string(날짜) |
| `totalResults` | 총 결과수 | int |
| `startIndex` | 페이지 수 | int |
| `itemsPerPage` | 페이지당 상품 수 | int |
| `query` | 조회 쿼리 | string |
| `searchCategoryId` | 분야 ID | int |
| `searchCategoryName` | 분야명 | string |

## item 필드 (상품 정보)

| 필드 | 설명 | 타입 |
|------|------|------|
| `title` | 상품명 | string |
| `link` | 상품 링크 URL | string(URL) |
| `author` | 저자/아티스트 | string |
| `pubDate` | 출간일(출시일) | date |
| `description` | 상품설명 (요약) | string |
| `isbn` | 10자리 ISBN | string |
| `isbn13` | 13자리 ISBN | string |
| `priceSales` | 판매가 | int |
| `priceStandard` | 정가 | int |
| `mallType` | 몰타입 | string |
| `stockStatus` | 재고상태 (정상이면 빈값) | string |
| `mileage` | 마일리지 | int |
| `cover` | 표지 이미지 URL | string(URL) |
| `publisher` | 출판사 | string |
| `salesPoint` | 판매지수 | int |
| `adult` | 성인등급 여부 | bool |
| `fixedPrice` | 정가제 여부 | bool |
| `customerReviewRank` | 리뷰 평점 (0~10, 별 0.5개당 1점) | int |
| `bestDuration` | 베스트셀러 순위 추가 정보 | string |
| `bestRank` | 베스트셀러 순위 | int |

**mallType 값:**
- `BOOK` — 국내도서
- `MUSIC` — 음반
- `DVD` — DVD
- `FOREIGN` — 외서
- `EBOOK` — 전자책
- `USED` — 중고상품

## seriesInfo (시리즈 정보)

| 필드 | 설명 | 타입 |
|------|------|------|
| `seriesId` | 시리즈 ID | int |
| `seriesLink` | 시리즈 조회 URL | string(URL) |
| `seriesName` | 시리즈 이름 | string |

## subInfo 부가정보

### usedList (중고상품 정보)

`OptResult=usedList` 요청 시 포함.

| 필드 | 설명 | 타입 |
|------|------|------|
| `aladinUsed.itemCount` | 알라딘 직배송 중고 보유수 | int |
| `aladinUsed.minPrice` | 알라딘 직배송 중고 최저가 | int |
| `aladinUsed.link` | 알라딘 직배송 중고 리스트 URL | string(URL) |
| `userUsed.itemCount` | 회원 직배송 중고 보유수 | int |
| `userUsed.minPrice` | 회원 직배송 중고 최저가 | int |
| `userUsed.link` | 회원 직배송 중고 리스트 URL | string(URL) |
| `spaceUsed.itemCount` | 광활한 우주점 중고 보유수 | int |
| `spaceUsed.minPrice` | 광활한 우주점 중고 최저가 | int |
| `spaceUsed.link` | 광활한 우주점 중고 리스트 URL | string(URL) |

중고 상품의 `usedType` 필드: `userUsed`(회원 직배송), `aladinUsed`(알라딘 직배송), `spaceUsed`(광활한 우주점)

### ebookList (전자책 정보)

`OptResult=ebookList` 요청 시 포함.

| 필드 | 설명 | 타입 |
|------|------|------|
| `itemId` | 전자책 ItemId | int |
| `isbn` | 전자책 ISBN | string |
| `isbn13` | 전자책 13자리 ISBN | string |
| `priceSales` | 전자책 판매가 | int |
| `link` | 전자책 상품페이지 URL | string(URL) |

### newBookList (중고상품의 새책 정보)

| 필드 | 설명 | 타입 |
|------|------|------|
| `itemId` | 새책 ItemId | int |
| `isbn` | 새책 ISBN | string |
| `isbn13` | 새책 13자리 ISBN | string |
| `priceSales` | 새책 판매가 | int |
| `link` | 새책 상품페이지 URL | string(URL) |

### fileFormatList (전자책 포맷)

| 필드 | 설명 | 타입 |
|------|------|------|
| `fileType` | 포맷 (EPUB, PDF 등) | string |
| `fileSize` | 용량 (byte) | int |

## 상품 조회 전용 부가정보

`ItemLookUp` API에서 `OptResult`로 요청 시 추가되는 필드.

| 필드 | 설명 | 타입 |
|------|------|------|
| `fullDescription` | 책소개 | string |
| `fullDescription2` | 출판사 제공 책소개 | string |
| `subTitle` | 부제 | string |
| `originalTitle` | 원제 | string |
| `itemPage` | 쪽수 | int |
| `toc` | 목차 | string |
| `taxFree` | 비과세 여부 | bool |

### ratingInfo (평점 정보)

| 필드 | 설명 | 타입 |
|------|------|------|
| `ratingScore` | 별점 | float |
| `ratingCount` | 별점 참여수 | int |
| `commentReviewCount` | 100자평 수 | int |
| `myReviewCount` | 마이리뷰 수 | int |

### authors (저자 상세)

| 필드 | 설명 | 타입 |
|------|------|------|
| `authorId` | 저자 ID | int |
| `authorName` | 저자 이름 | string |
| `authorType` | 참여 타입 | string |
| `authorTypeDesc` | 참여 타입 설명 | string |
| `authorInfo` | 저자 소개 | string |
| `authorInfoLink` | 저자 페이지 URL | string(URL) |

### c2bsales (중고 매입 정보)

| 필드 | 설명 | 타입 |
|------|------|------|
| `c2bsales` | 매입 여부 (1=가능, 2=불가) | int |
| `c2bsales_price` | 매입가 (AA=최상, A=상, B=중, C=균일가 1000원, C=0이면 매입불가) | int |

### packing (판형/포장)

| 필드 | 설명 | 타입 |
|------|------|------|
| `styleDesc` | 판형 (양장본, 반양장본 등) | string |
| `weight` | 무게 (g) | int |
| `sizeDepth` | 깊이 (mm) | int |
| `sizeHeight` | 세로 (mm) | int |
| `sizeWidth` | 가로 (mm) | int |

### offStoreInfo (매장 정보)

| 필드 | 설명 | 타입 |
|------|------|------|
| `offCode` | 매장 offCode | string |
| `offName` | 매장명 | string |
| `link` | 매장 상품페이지 URL | string(URL) |
| `hasStock` | 보유 여부 (1=보유, 0=미보유) | int |
| `maxPrice` | 최고가 | int |
| `minPrice` | 최저가 | int |
| `location` | 매장 내 위치 | string |

### reviewList (리뷰)

| 필드 | 설명 | 타입 |
|------|------|------|
| `reviewRank` | 리뷰 평점 (10점 만점) | int |
| `writer` | 작성자 (닉네임) | string |
| `link` | 리뷰 URL | string(URL) |
| `title` | 리뷰 제목 | string |

### phraseList (책속에서)

| 필드 | 설명 | 타입 |
|------|------|------|
| `pageNo` | 페이지 범위 | string |
| `phrase` | 문구 또는 이미지 (HTML) | string |

### mdRecommendList (편집장의 선택)

| 필드 | 설명 | 타입 |
|------|------|------|
| `title` | 제목 | string |
| `comment` | 내용 (HTML) | string |
| `mdName` | MD 이름/날짜 | string |
