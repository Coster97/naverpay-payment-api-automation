# 네이버페이 결제형 및 인증형 단건/자동 API 테스트 자동화

네이버페이 개발자센터에서 제공하는 결제 API를 대상으로, 
Postman과 Newman을 활용하여 결제 예약·승인·적립·조회·취소 등 주요 결제 시나리오를 자동화한 프로젝트입니다.

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

## 기술 스택

- Postman
- JavaScript
- Newman
- Node.js
- Git
- GitHub
---

## 단건 테스트 시나리오

```
결제 예약
    ↓
결제 승인
    ↓
포인트 적립 요청
    ↓
결제내역 조회 (특정)
    ↓
현금영수증 금액 조회
    ↓
결제 취소
    ↓
결제내역 조회 (기간)
```

## 자동 테스트 시나리오

```
등록 예약
    ↓
등록 완료
    ↓
등록내역 조회
    ↓
결제 예약
    ↓
결제 승인
    ↓
포인트 적립 요청
    ↓
결제내역 조회 (기간)
    ↓
현금영수증 금액 조회
    ↓
결제 취소
    ↓
결제내역 조회 (특정)
    ↓
등록 해지
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
- 응답 스키마 기반 검증 강화
- Collection 구조 개선

---

## 실행 결과 예시

### Newman CLI

<img width="800" height="1289" alt="image" src="https://github.com/user-attachments/assets/b74a54ec-2106-4cfc-8c5c-63e661453314" />


### HTML Report

<img width="800" height="1205" alt="image" src="https://github.com/user-attachments/assets/5f7d4dca-ec18-4d1e-8cdd-314875aa3d90" />

