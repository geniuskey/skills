# 상품 조회 API (ItemLookUp)

## 엔드포인트

```
GET http://www.aladin.co.kr/ttb/api/ItemLookUp.aspx
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
| `itemIdType` | string | `ISBN` | `ISBN`(10자리), `ISBN13`(13자리), `ItemId`(알라딘 고유). **가급적 ISBN13 사용 권장** |
| `Cover` | string | `Mid` | 표지 크기 |
| `Output` | string | `xml` | 출력: `xml`, `js` |
| `Partner` | string | - | 파트너코드 |
| `Version` | int | `20070901` | API 버전 (최신: `20131101`) |
| `includeKey` | int | `0` | 1이면 링크에 TTBKey 포함 |
| `offCode` | string | - | 중고매장 offCode (매장별 조회) |
| `OptResult` | string | - | 부가정보 (콤마 구분, 아래 목록 참고) |

### OptResult 부가정보 목록

| 값 | 설명 |
|----|------|
| `ebookList` | 해당 종이책의 전자책 정보 |
| `usedList` | 중고상품 정보 |
| `fileFormatList` | 전자책 포맷/용량 |
| `c2binfo` | 중고 C2B 매입 여부 및 매입가 |
| `packing` | 판형, 포장 정보 |
| `b2bSupply` | 전자책 B2B 납품 가능 여부 |
| `subbarcode` | 부가기호 |
| `cardReviewImgList` | 카드리뷰 이미지 |
| `ratingInfo` | 별점, 100자평/마이리뷰 개수 |
| `bestSellerRank` | 주간 베스트셀러 순위 |
| `previewImgList` | 미리보기 이미지 (별도 협의) |
| `eventList` | 관련 이벤트 (별도 협의) |
| `authors` | 저자/아티스트 상세 (별도 협의) |
| `reviewList` | 리뷰 목록 (별도 협의) |
| `fulldescription` | 상품 설명 + 출판사 소개 (별도 협의) |
| `fulldescription2` | 출판사 제공 소개 |
| `Toc` | 목차 (별도 협의) |
| `Story` | 줄거리 (별도 협의) |
| `categoryIdList` | 전체 분야 (별도 협의) |
| `mdrecommend` | 편집장의 선택 (별도 협의) |
| `phraseList` | 책속에서 (별도 협의, 최대 3개) |

> "별도 협의" 표시 항목은 일반 API key로는 제공되지 않을 수 있다.

## 요청 예시

```
http://www.aladin.co.kr/ttb/api/ItemLookUp.aspx?ttbkey=[TTBKey]&itemIdType=ISBN13&ItemId=9788932917245&output=js&Version=20131101&OptResult=ebookList,usedList,ratingInfo
```
