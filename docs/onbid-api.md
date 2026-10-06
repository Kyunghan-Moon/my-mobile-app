# 온비드 공고목록 조회서비스 (OnbidPbancListSrvc) Open API 메모

차세대 국가자산처분시스템(온비드) 구축 사업 Open API 활용가이드 중 공고목록 조회서비스 정리.

## 서비스 개요

| 항목 | 내용 |
|---|---|
| 서비스ID | SVC-API-015 |
| 서비스명 | 온비드 공고목록 조회서비스 (`OnbidPbancListSrvc`) |
| 설명 | 온비드에 등록된 압류재산·국유재산 등 공매 물건의 공고 목록 조회 (물건유형, 재산유형, 공고기간 등) |
| 인증 | 서비스 Key (공공데이터포털 발급, URL-Encode 필요) |
| 인터페이스 | REST (GET), 교환데이터: XML / JSON |
| 전송 암호화 | 명세상 "없음"이나, 운영 URL은 https |
| 운영 URL | `https://apis.data.go.kr/B010003/OnbidPbancListSrvc2` |
| 개발 URL | N/A |
| 버전 | 2.0 (배포일 2026/04/06) |
| 성능 | 평균응답 500ms, 최대 10 tps |

### v2.0 변경 이력
- 처분방식코드(`dspsMthodCd`) 필수입력값에서 제외
- 입찰구분코드(`bidDivCd`) 필수입력값에서 제외
- 재산유형코드에 '파산재산' 포함
- 온비드공고번호(`onbidPbancNo`), 공매번호(`pbctNo`) 출력값 추가

## 오퍼레이션: 공고목록 정보조회 `getPbancList`

- 호출 URL: `https://apis.data.go.kr/B010003/OnbidPbancListSrvc2/getPbancList2`
- 유형: 조회(목록)

### 요청 파라미터 (필수 = 1, 옵션 = 0)

| 항목 | 국문 | 필수 | 크기 | 샘플 | 설명 |
|---|---|---|---|---|---|
| `serviceKey` | 서비스키 | 1 | 100 | 인증키(URL-Encode) | 공공데이터포털 발급 인증키 |
| `pageNo` | 페이지번호 | 1 | 2 | 10 | |
| `numOfRows` | 한 페이지 결과 수 | 1 | 5 | 1 | |
| `resultType` | 응답유형 | 1 | - | json | json / xml |
| `cltrTypeCd` | 물건유형코드 | 1 | 4 | 0001 | 0001 부동산, 0002 자동차, 0003 동산 |
| `prptDivCd` | 재산유형코드 | 1 | 4 | 0007,0005 | 복수는 쉼표 구분. 코드는 아래 코드표 |
| `opbdDtStart` | 개찰일 시작 | 1 | 8 | 20250301 | yyyyMMdd |
| `opbdDtEnd` | 개찰일 종료 | 1 | 8 | 20250331 | yyyyMMdd |
| `bidDivCd` | 입찰구분코드 | 0 | 4 | 0001 | 0001 인터넷, 0002 현장 |
| `dspsMthodCd` | 처분방식코드 | 0 | 4 | 0001 | 0001 매각, 0002 임대 |
| `pbancYmdStart` | 공고일 시작 | 0 | 8 | 20250301 | yyyyMMdd |
| `pbancYmdEnd` | 공고일 종료 | 0 | 8 | 20250331 | yyyyMMdd |
| `bidPrdYmdStart` | 입찰기간 시작 | 0 | 8 | 20250301 | yyyyMMdd |
| `bidPrdYmdEnd` | 입찰기간 종료 | 0 | 8 | 20250331 | yyyyMMdd |
| `onbidPbancNm` | 공고명 | 0 | 2000 | 경기도 광주시 … 토지 매각 | |
| `orgNm` | 공고기관명 | 0 | 80 | 대한적십자사 | |

### 응답 필드

공통: `resultCode`(00 정상), `resultMsg`, `pageNo`, `totalCount`, `numOfRows`

항목(item):

| 항목 | 국문 | 필수 | 샘플 |
|---|---|---|---|
| `pbancMngNo` | 공고관리번호 | 0 | 202406-21411-00 |
| `onbidPbancNo` | 온비드공고번호 | 1 | 871909 |
| `pbctNo` | 공매번호 | 1 | 10029488 |
| `pbancKindCd` / `pbancKindNm` | 공고유형 코드/명 | 0 | 0002 / 재공고 |
| `prptDivCd` / `prptDivNm` | 재산유형 코드/명 | 0 | 0005 / 기타일반재산 |
| `dspsMthodCd` / `dspsMthodNm` | 처분방식 코드/명 | 0 | 0001 / 매각 |
| `onbidPbancNm` | 공고명 | 0 | 경기도 광주시 남종면 … 토지 매각 |
| `orgNm` | 기관명 | 0 | 대한적십자사 |
| `rspbSbrNm` | 담당부점명 | 0 | 서울특별시지사 |
| `pbancYmd` | 공고일자 (yyyyMMdd) | 0 | 20250310 |
| `pbctNsq` | 회차 | 0 | 1 |
| `pbctsn` | 차수 (2026.1.1 이후 확정 공고는 표시 안 됨) | 0 | 1 |
| `cltrBidBgngDt` | 입찰시작일시 (yyyyMMddHHmm) | 0 | 202501011410 |
| `cltrBidEndDt` | 입찰종료일시 (yyyyMMddHHmm) | 0 | 202501101410 |
| `cltrOpbdDt` | 개찰일시 (yyyyMMddHHmm) | 0 | 202408061030 |
| `bidDivCd` / `bidDivNm` | 입찰구분 코드/명 | 0 | 0001 / 인터넷 |

## 코드표

| 항목 | 코드 |
|---|---|
| `prptDivCd` 재산유형 | 0007 압류재산, 0010 국유재산, 0005 기타일반재산, 0004 불용품, 0002 공유재산, 0003 금융권담보재산, 0006 유입재산, 0008 수탁재산, 0011 공공개발재산, 0013 파산재산 |
| `dspsMthodCd` 처분방식 | 0001 매각, 0002 임대 |
| `bidMthodCd` 세부입찰방식 | 0001 최고가, 0002 호가, 0003 평가, 0004 추첨, 0005 시담, 0006 적정최고가, 0007 제한적최고가 |
| `cptnMthodCd` 입찰방식 | 0001 일반경쟁, 0002 제한경쟁, 0003 지명경쟁, 0004 수의계약 |
| `totalamtUnpcDivCd` 총액단가 | 0001 총액, 0002 단가 |
| `bidDivCd` 입찰구분 | 0001 인터넷, 0002 현장 |
| `cltrTypeCd` 물건유형 | 0001 부동산, 0002 자동차, 0003 동산 |
| `pbctStatCd` 입찰결과구분 | 0001 입찰준비중, 0002 입찰진행중, 0003 입찰마감, 0006 개찰중, 0009 수의계약가능, 0010 낙찰, 0011 유찰, 0012 취소 |
| `pbancKindCd` 공고유형 | 0001 일반, 0002 재공고, 0003 정정, 0004 연기, 0005 취소, 0006 긴급 |
| `exctStatCd` 집행상태 | 0001 개찰준비중, 0002 개찰중, 0003 개찰완료 |

## 결과코드 (`resultCode`)

| 코드 | 메시지 | 설명 |
|---|---|---|
| 00 | NORMAL_CODE | 정상 |
| 01 | APPLICATION_ERROR | 어플리케이션 에러 |
| 02 | DB_ERROR | 데이터베이스 에러 |
| 03 | NODATA_ERROR | 데이터 없음 |
| 04 | HTTP_ERROR | HTTP 에러 |
| 05 | SERVICETIMEOUT_ERROR | 서비스 연결실패 |
| 10 | INVALID_REQUEST_PARAMETER_ERROR | 잘못된 요청 파라미터 |
| 11 | NO_MANDATORY_REQUEST_PARAMETERS_ERROR | 필수 파라미터 없음 |
| 12 | NO_OPENAPI_SERVICE_ERROR | 해당 서비스 없음/폐기 |
| 20 | SERVICE_ACCESS_DENIED_ERROR | 서비스 접근거부 |
| 21 | TEMPORARILY_DISABLE_THE_SERVICEKEY_ERROR | 일시적으로 사용 불가한 키 |
| 22 | LIMITED_NUMBER_OF_SERVICE_REQUESTS_EXCEEDS_ERROR | 요청 제한 횟수 초과 |
| 30 | SERVICE_KEY_IS_NOT_REGISTERED_ERROR | 등록되지 않은 서비스키 |
| 31 | DEADLINE_HAS_EXPIRED_ERROR | 기한만료된 서비스키 |
| 32 | UNREGISTERED_IP_ERROR | 등록되지 않은 IP |
| 33 | UNSIGNED_CALL_ERROR | 서명되지 않은 호출 |
| 99 | UNKNOWN_ERROR | 기타 에러 |

## XML / JSON 응답 구조 차이

배열 구조에서 두 형식이 다르다. 가이드의 예시(다른 오퍼레이션인 물건 상세 응답 기준) 형태:

- 공통 골격: `response.header{resultCode,resultMsg}` + `response.body{items, numOfRows, pageNo, totalCount}`
- XML: 목록은 `<items><item>…</item></items>`, 하위 배열은 `<apslEvlClgList><listItem>…</listItem></apslEvlClgList>`
- JSON: `body.items.item` 은 배열, 하위 배열(`apslEvlClgList`)도 JSON 배열. 숫자 값(`apslEvlAmt`, `numOfRows` 등)은 숫자 타입

## 구현 시 유의사항 (메모)

- 가이드 4장 예시는 `cltrMngNo`, `onbidCltrNm` 등 물건 상세 응답이라 `getPbancList` 의 실제 응답과 필드가 다르다. 실제 호출 결과로 확인 후 모델 정의할 것.
- 1.2.4 표는 `resultCode` 를 최상위로 적었지만, 실제 응답은 `header` / `body` 로 감싸질 가능성이 높다. 실호출로 확인.
- 필수 파라미터: `serviceKey, pageNo, numOfRows, resultType, cltrTypeCd, prptDivCd, opbdDtStart, opbdDtEnd`.
- 최대 10 tps 이므로 앱에서 직접 호출 시 요청 빈도 제한/캐싱 고려.
- `serviceKey` 는 앱 소스나 저장소에 넣지 말 것. 앱에 직접 넣으면 APK 에서 추출되므로, 가능하면 서버(프록시)를 거치거나 최소한 빌드 시 주입(`--dart-define`)하고 커밋 금지.
