# 네이버페이 결제형 단건 API 테스트 자동화

Postman과 Newman을 활용하여 네이버페이 결제 API의 주요 시나리오를 자동화한 프로젝트입니다.

결제 예약, 승인, 적립, 결제내역 조회, 현금영수증 금액 조회, 결제 취소까지의 API 흐름을 하나의 테스트 시나리오로 구성하였으며, 응답 데이터를 다음 API에서 재사용할 수 있도록 환경 변수를 활용했습니다.

---

## 프로젝트 소개

본 프로젝트는 API 테스트 자동화를 학습하고 실무와 유사한 테스트 환경을 구성하기 위해 제작되었습니다.

주요 구현 내용은 다음과 같습니다.

- Postman Collection 기반 API 테스트 구성
- JavaScript를 활용한 응답 검증
- 환경 변수(Environment) 및 Collection Variable 관리
- API 간 데이터 연계 자동화
- Newman 기반 CLI 실행
- HTML 테스트 리포트 생성

---

## 기술 스택

- Postman
- JavaScript
- Newman
- Node.js
- Git
- GitHub

---

## 테스트 시나리오

```
결제 예약
    ↓
결제 승인
    ↓
포인트 적립요청
    ↓
결제내역조회 (특정/기간)
    ↓
현금영수증 금액 조회
    ↓
결제 취소
```

각 API의 응답값을 환경 변수에 저장하여 다음 API에서 재사용하도록 구성했습니다.

예시)

```javascript
const response = pm.response.json();

pm.environment.set("paymentId", response.body.paymentId);
pm.collectionVariables.set("paymentId", response.body.paymentId);
```

---

## 주요 검증 항목

- HTTP Status Code 검증
- 응답 성공 여부 검증
- 필수 응답 필드 존재 여부 검증
- 결제 금액 검증
- 결제 상태 검증
- paymentId 동적 저장 및 재사용
- 조건부 응답 필드 검증
- API 간 데이터 연계 검증

---

## 프로젝트 구조

```
.
├── README.md
├── package.json
├── .gitignore
├── postman
│   ├── Collection.json
│   ├── Environment.sample.json
│   └── Environment.json (Git 제외)
└── reports
```

---

## 실행 방법

### 1. 패키지 설치

```bash
npm install
```

### 2. Environment 파일 생성

```
Environment.sample.json
```

파일을 복사하여

```
Environment.json
```

으로 변경한 뒤 필요한 인증 정보를 입력합니다.
인증 정보는 아래 경로에서 확인 가능합니다.

1. 네이버페이센터 https://developers.pay.naver.com/ > 가입/로그인 
2. 내 개발정보 https://developers.pay.naver.com/user/merchant/auth 진입
3. 결제형 > 내 인증값 확인하기 > 네이버페이 샌드박스 가맹점 정보 확인가능


### 3. Newman 실행

```bash
npx newman run postman/Collection.json \
-e postman/Environment.json \
-r cli,htmlextra \
--reporter-htmlextra-export reports/report.html
```

---

## 실행 환경

본 프로젝트는 네이버페이 샌드박스 및 내부 테스트 환경을 기반으로 작성되었습니다.

일부 API(결제 요청)은 접근 권한이 필요한 테스트 환경에서만 실행 가능하며, 저장소에는 테스트 구조와 자동화 코드만 포함되어 있습니다.

---

## 향후 개선 계획

- 실패 시나리오 자동화 추가
- 데이터 기반 테스트(Data Driven Testing) 적용
- GitHub Actions를 활용한 자동 실행
- 테스트 리포트 자동 업로드
- 테스트 케이스 확장
