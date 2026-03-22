# 상품 리스트 API (ItemList)

## 엔드포인트

```
GET http://www.aladin.co.kr/ttb/api/ItemList.aspx
```

## 요청 파라미터

### 필수

| 파라미터 | 타입 | 설명 |
|---------|------|------|
| `ttbkey` | string | TTBKey |
| `QueryType` | string | 리스트 종류 (아래 참고) |

**QueryType 값:**
- `ItemNewAll` — 신간 전체 리스트
- `ItemNewSpecial` — 주목할 만한 신간 리스트
- `ItemEditorChoice` — 편집자 추천 리스트 (카테고리 조회만 가능, 국내도서/음반/외서)
- `Bestseller` — 베스트셀러
- `BlogBest` — 블로거 베스트셀러 (국내도서만)

### 옵션

| 파라미터 | 타입 | 기본값 | 설명 |
|---------|------|--------|------|
| `SearchTarget` | string | `Book` | 대상: `Book`, `Foreign`, `Music`, `DVD`, `Used`, `eBook`, `All` |
| `SubSearchTarget` | string | - | `SearchTarget=Used`일 때 서브몰: `Book`, `Music`, `DVD` |
| `Start` | int | `1` | 시작 페이지 |
| `MaxResults` | int | `10` | 페이지당 결과 수 (1~100) |
| `Cover` | string | `Mid` | 표지 크기 |
| `CategoryId` | int | `0` | 분야 제한 |
| `Output` | string | `xml` | 출력: `xml`, `js` |
| `Partner` | string | - | 파트너코드 |
| `includeKey` | int | `0` | 1이면 링크에 TTBKey 포함 |
| `InputEncoding` | string | `utf-8` | 인코딩 |
| `Version` | int | `20070901` | API 버전 (최신: `20131101`) |
| `outofStockfilter` | int | `0` | 1이면 품절/절판 제외 |
| `Year`, `Month`, `Week` | int | `0` | `Bestseller` 조회 시 주간 지정. 예: `Year=2022&Month=5&Week=3`. 생략 시 현재 주간 |
| `OptResult` | string | - | 부가정보: `ebookList`, `usedList`, `fileFormatList` |

## 요청 예시

```
http://www.aladin.co.kr/ttb/api/ItemList.aspx?ttbkey=[TTBKey]&QueryType=Bestseller&MaxResults=10&start=1&SearchTarget=Book&output=js&Version=20131101
```

## 제한

- 한 페이지 최대 100개
- 총 결과 200개까지만 조회 가능
