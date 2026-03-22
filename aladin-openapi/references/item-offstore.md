# 중고상품 보유 매장 검색 API (ItemOffStoreList)

## 엔드포인트

```
GET http://www.aladin.co.kr/ttb/api/ItemOffStoreList.aspx
```

## 요청 파라미터

### 필수

| 파라미터 | 타입 | 설명 |
|---------|------|------|
| `ttbkey` | string | TTBKey |
| `ItemId` | string/int | 상품 고유값 (ISBN 또는 알라딘 ItemId) |

### 옵션

| 파라미터 | 타입 | 기본값 | 설명 |
|---------|------|--------|------|
| `itemIdType` | string | `ISBN` | `ISBN`(10자리), `ISBN13`(13자리), `ItemId` |
| `Output` | string | `xml` | 출력: `xml`, `js` |

## 응답 필드

| 필드 | 설명 | 타입 |
|------|------|------|
| `version` | API 버전 | int (날짜형) |
| `link` | 관련 알라딘 페이지 URL | string(URL) |
| `pubDate` | API 출력일 | string(날짜) |
| `query` | 조회 쿼리 | string |
| `itemOffStoreList` | 매장 정보 배열 | array |
| `itemOffStoreList.offCode` | 매장 offCode | string |
| `itemOffStoreList.offName` | 매장명 | string |
| `itemOffStoreList.link` | 매장 상품 링크 URL | string(URL) |

## 요청 예시

```
http://www.aladin.co.kr/ttb/api/ItemOffStoreList.aspx?ttbkey=[TTBKey]&itemIdType=ISBN13&ItemId=9788932917245&output=js
```

## 활용

매장 검색으로 얻은 `offCode`를 상품 조회 API의 `offCode` 파라미터에 전달하면 해당 매장의 상세 재고를 확인할 수 있다.
