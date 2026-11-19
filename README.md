# 네이버페이 결제형·인증형 API 테스트 자동화

네이버페이 개발자센터에서 제공하는 결제형·인증형 API를 대상으로, Postman과 Newman을 활용해 단건결제 및 자동결제의 예약·승인·적립·조회·취소 등 총 36개 API에 대한 테스트를 자동화한 프로젝트입니다.

결제 API는 예약, 승인, 적립, 조회, 취소 등으로 구성되어 있으며, 결제번호 등 공통 데이터를 기준으로 각 API가 서로 연계되어 동작합니다.

본 프로젝트는 실제 결제 시나리오를 기반으로 각 API를 구성하고, 이전 API에서 생성된 응답 데이터를 후속 API에서 자동으로 재사용하도록 구현하여 반복적인 API 테스트를 효율적으로 수행할 수 있는 환경을 구축했습니다.

> ⚠️ 본 프로젝트는 개인 학습 및 포트폴리오 목적으로 작성되었습니다.
> 실제 실행을 위해서는 별도의 테스트 환경 접근 권한과 환경변수 설정이 필요하며,
> 저장소에는 내부 URL, 계정 정보 및 테스트 데이터가 포함되어 있지 않습니다.
---
### 주요 구현 내용

- Postman Collection 기반 API 테스트 구성
- Postman Test Script(JavaScript) 기반 응답 검증
- Environment 및 Collection Variable 관리
- API 간 응답 데이터 자동 연계
- Newman 기반 CLI 테스트 실행
- HTML 테스트 리포트 자동 생성

---
## 테스트 자동화 성과

- 수동 테스트: API 36개 × 최소 3분 = 약 108분 소요
- 자동화 테스트: 총 36개 API 테스트 약 27초 만에 수행
- 테스트 수행 시간 약 99.6% 단축
- API 간 응답 데이터 자동 연계 및 검증으로 반복적인 수동 작업 최소화
- Newman 기반 일괄 실행으로 테스트 수행 효율 향상
---

## 기술 스택

- Postman
- JavaScript
- Newman
- Node.js
- Git
- GitHub
---


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
- 필수 응답 필드 존재 여부 및 정합성 검증
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

## 향후 개선 계획

- 실패 시나리오 및 예외 응답 검증 추가
- 결제유형별 코드 분리

## 실행 방법

### 1. 의존성 설치

프로젝트 루트 디렉터리에서 실행합니다.

```bash
npm install
```

### 2. 환경변수 설정

`postman/Environment.json` 파일에 API 호출에 필요한 환경변수를 설정합니다.

- API 서버 도메인
- Client ID / Client Secret
- Chain ID 등 인증 정보

### 3. API 테스트 실행

```bash
npx newman run "./postman/Collection.json" -e "./postman/Environment.json"
```

### 4. API 테스트 실행 및 HTML 리포트 생성

```bash
npx newman run "./postman/Collection.json" -e "./postman/Environment.json" -r cli,htmlextra --reporter-htmlextra-export "./reports/newman-report.html"
```

HTML 리포터가 설치되어 있지 않은 경우 다음 명령어로 설치합니다.

```bash
npm install --save-dev newman newman-reporter-htmlextra
```

### 5. HTML 리포트 확인

```bash
start "./reports/newman-report.html"
```

---

## 실행 결과 예시

Newman 실행 시 터미널에서 API별 테스트 결과와 전체 실행 통계를 확인할 수 있습니다.

HTML 리포트에서는 다음 항목을 확인할 수 있습니다.

- 전체 API 요청 및 테스트 수행 결과
- API별 응답 시간 및 HTTP 상태 코드
- 테스트 성공·실패 건수
- 실패한 테스트의 오류 메시지
- 전체 테스트 실행 시간


### Newman CLI

<img width="1117" height="971" alt="image" src="https://github.com/user-attachments/assets/44555c47-1bc8-4c8c-a178-948b12d14b5e" />



### HTML Report

<img width="859" height="904" alt="image" src="https://github.com/user-attachments/assets/f7849822-d3e6-41c8-8ced-efd838140fd9" />




