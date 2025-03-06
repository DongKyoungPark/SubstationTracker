# 구독 관리 서비스 (Subscription Tracker)

## 프로젝트 개요

구독 관리 서비스는 사용자들이 자신의 다양한 구독 서비스를 추적하고 관리할 수 있는 웹 애플리케이션입니다. 이 서비스는 구독 갱신일 알림, 구독 비용 추적, 구독 관리 기능을 제공합니다.

## 주요 기능

- 사용자 계정 관리 (회원가입, 로그인, 로그아웃)
- 구독 서비스 추가 및 관리
- 구독 갱신일 자동 계산
- 이메일 알림 시스템 (갱신일 7일, 5일, 2일, 1일 전)
- 구독 상태 추적 (활성, 취소, 만료)

## 기술 스택

- **백엔드**: Node.js, Express.js
- **데이터베이스**: MongoDB, Mongoose
- **인증**: JWT (JSON Web Tokens)
- **이메일 서비스**: Nodemailer
- **작업 스케줄링**: Upstash Workflow
- **보안**: Arcjet (봇 감지, 속도 제한)

## 시작하기

### 필수 조건

- Node.js (v14 이상)
- MongoDB 계정
- Upstash 계정 (워크플로우 관리용)
- Gmail 계정 (이메일 알림용)

### 환경 설정

1. 프로젝트 클론:

   ```bash
   git clone [repository-url]
   cd subscription-tracker
   ```

2. 의존성 설치:

   ```bash
   npm install
   ```

3. 환경 변수 설정:
   `.env.development.local` 파일 생성:
   ```
   PORT=3000
   NODE_ENV=development
   DB_URI=mongodb+srv://[username]:[password]@[cluster]/[database]
   JWT_SECRET=[your-jwt-secret]
   JWT_EXPIRES_IN=30d
   ARCJET_KEY=[your-arcjet-key]
   ARCJET_ENV=development
   QSTASH_TOKEN=[your-qstash-token]
   QSTASH_URL=[your-qstash-url]
   EMAIL_PASSWORD=[your-gmail-app-password]
   SERVER_URL=http://localhost:3000
   ```

### 실행 방법

- 개발 모드:

  ```bash
  npm run dev
  ```

- 프로덕션 모드:
  ```bash
  npm start
  ```

## API 엔드포인트

### 인증

- `POST /api/v1/auth/sign-up`: 회원가입
- `POST /api/v1/auth/sign-in`: 로그인
- `POST /api/v1/auth/sign-out`: 로그아웃

### 사용자

- `GET /api/v1/users`: 모든 사용자 조회
- `GET /api/v1/users/:id`: 특정 사용자 조회

### 구독

- `POST /api/v1/subscriptions`: 새 구독 생성
- `GET /api/v1/subscriptions/user/:id`: 사용자의 모든 구독 조회
- `PUT /api/v1/subscriptions/:id`: 구독 정보 업데이트
- `DELETE /api/v1/subscriptions/:id`: 구독 삭제
- `PUT /api/v1/subscriptions/:id/cancel`: 구독 취소

### 워크플로우

- `POST /api/v1/workflows/subscription/reminder`: 구독 알림 워크플로우 트리거

## 프로젝트 구조

subscription-tracker/
├── app.js # 애플리케이션 진입점
├── config/ # 설정 파일
│ ├── arcjet.js # Arcjet 보안 설정
│ ├── env.js # 환경 변수 설정
│ ├── nodemailer.js # 이메일 설정
│ └── upstash.js # Upstash 워크플로우 설정
├── controllers/ # 컨트롤러
│ ├── auth.controller.js # 인증 관련 컨트롤러
│ ├── subscription.controller.js # 구독 관련 컨트롤러
│ ├── user.controller.js # 사용자 관련 컨트롤러
│ └── workflow.controller.js # 워크플로우 컨트롤러
├── database/
│ └── mongodb.js # 데이터베이스 연결
├── middlewares/ # 미들웨어
│ ├── arcjet.middleware.js # Arcjet 보안 미들웨어
│ ├── auth.middleware.js # 인증 미들웨어
│ └── error.middleware.js # 오류 처리 미들웨어
├── models/ # 데이터 모델
│ ├── subscription.model.js # 구독 모델
│ └── user.model.js # 사용자 모델
├── routes/ # 라우트
│ ├── auth.routes.js # 인증 라우트
│ ├── subscription.routes.js # 구독 라우트
│ ├── user.routes.js # 사용자 라우트
│ └── workflow.routes.js # 워크플로우 라우트
└── utils/ # 유틸리티
├── email-template.js # 이메일 템플릿
└── send-email.js # 이메일 발송 유틸리티

## 이메일 알림 시스템

이 서비스는 구독 갱신일 전에 다음과 같은 알림을 제공합니다:

- 갱신일 7일 전 알림
- 갱신일 5일 전 알림
- 갱신일 2일 전 알림
- 갱신일 1일 전 알림

## 보안 기능

- JWT 기반 인증
- 비밀번호 암호화 (bcrypt)
- Arcjet를 통한 봇 감지 및 속도 제한
