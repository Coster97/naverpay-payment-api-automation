# 네이버페이 결제형 단건 API 테스트 자동화

Postman과 Newman을 활용하여 네이버페이 결제 API의 주요 시나리오를 자동화한 프로젝트입니다.

결제 API는 **예약 → 승인 → 적립 → 조회 → 취소**처럼 여러 API가 하나의 업무 흐름으로 연결됩니다.

본 프로젝트는 이러한 API 흐름을 자동화하고, 응답 데이터를 다음 API에서 재사용하도록 구성하여 반복적인 API 테스트를 효율적으로 수행할 수 있는 테스트 환경을 구현했습니다.

---

## 프로젝트 소개

API 테스트 자동화 환경을 구성하고, API 간 데이터 연계 및 응답 검증 자동화를 구현하기 위해 제작한 프로젝트입니다.

### 주요 구현 내용

- Postman Collection 기반 API 테스트 구성
- Postman Test Script(JavaScript) 기반 응답 검증
- Environment 및 Collection Variable 관리
- API 간 응답 데이터 자동 연계
- Newman 기반 CLI 테스트 실행
- HTML 테스트 리포트 자동 생성

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
포인트 적립 요청
    ↓
결제내역 조회 (특정 / 기간)
    ↓
현금영수증 금액 조회
    ↓
결제 취소
```

각 API의 응답 데이터를 Environment Variable 및 Collection Variable에 저장하여 후속 API에서 재사용하도록 구성했습니다.

예시)

```javascript
const response = pm.response.json();

pm.environment.set("paymentId", response.body.paymentId);
pm.collectionVariables.set("paymentId", response.body.paymentId);
```

---

## 프로젝트 특징

- API 간 응답 데이터를 자동으로 연계하여 테스트 수행
- Environment Variable 및 Collection Variable을 활용한 데이터 관리
- Newman 기반 CLI 테스트 실행
- HTML 테스트 리포트 자동 생성
- 반복 실행이 가능한 API 테스트 환경 구축

---

## 주요 검증 항목

- HTTP Status Code 검증
- 응답 성공 여부 검증
- 필수 응답 필드 존재 여부 검증
- 결제 금액 검증
- 결제 상태 검증
- paymentId 추출 및 후속 API 재사용
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

`Environment.sample.json`을 복사하여

`Environment.json`

파일을 생성한 뒤 필요한 인증 정보를 입력합니다.

인증 정보는 아래 경로에서 확인할 수 있습니다.

1. 네이버페이 Developers 로그인
2. **내 개발정보**
3. **결제형 > 내 인증값 확인하기**
4. 샌드박스 가맹점 정보 확인

---

### 3. Newman 실행

```bash
npm run test:api
```

실행 결과는 `reports/newman-report.html`에서 확인할 수 있습니다.

---

## 실행 환경

본 프로젝트는 네이버페이 샌드박스 및 내부 테스트 환경을 기반으로 작성되었습니다.

결제 승인 API는 내부 테스트 환경을 통해 paymentId(결제 승인 전인)를 수집하여 호출 가능한 API이므로 권한에 따라 실행이 어려울 수 있습니다.

---

## 향후 개선 계획

- 실패 시나리오 자동화 추가
- Data Driven Testing 적용
- Collection 모듈화
- GitHub Actions 기반 CI 자동 실행
- 테스트 케이스 확장

---

## 실행 결과

### Newman CLI

<img width="800" alt="CLI Result" src="https://github.com/user-attachments/assets/b7e8c573-20a2-4702-947e-b09c033f2202" />

### HTML Report

<img width="800" alt="HTML Report" src="https://github.com/user-attachments/assets/1287cbc9-9347-4d8a-840b-10c866f7b5be" />
