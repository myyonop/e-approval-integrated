# 전자결재 시스템 통합본

첫 번째 프로젝트(`approval-system-main`)의 로그인/회원가입 및 Spring Boot 인증 API를 두 번째 프로젝트(`e-approval-next`)의 Next.js 화면에 통합한 버전입니다. 기존 테스트용 이메일 로그인과 임시 회원가입은 제거했습니다.

## 구성
- `backend/`: Spring Boot 인증/회원가입/세션 API
- `e-approval-next/`: Next.js 화면
- 로그인: `/auth`
- 회원가입: `/signup`

## 실행 방법

### 1. 백엔드 실행
Java 17 이상이 필요합니다.

```bash
cd backend
./gradlew bootRun
```

Windows에서는 `gradlew.bat bootRun`을 실행하세요. 백엔드는 `http://localhost:8080`에서 실행됩니다.

초기 관리자 계정은 `backend/src/main/resources/application.yml`의 `app.admin.login-id`와 `app.admin.password` 설정을 확인하세요. 기본 비밀번호는 실제 사용 전에 반드시 변경하세요.

### 2. 프론트엔드 실행
별도 터미널에서:

```bash
cd e-approval-next
npm install
npm run dev
```

브라우저에서 `http://localhost:3000`을 엽니다. Next.js rewrite가 `/api/*` 요청을 `http://localhost:8080/api/*`로 전달합니다.

## 회원가입 정책
- 아이디: 영문/숫자/밑줄 4~20자
- 비밀번호: 8자 이상
- 사원: 가입 후 바로 로그인 가능
- 팀장/본부장/대표: 관리자 승인 후 로그인 가능

## 참고
전자결재 문서 목록과 결재 내역은 두 번째 프로젝트에 있던 화면용 예시 데이터를 유지하고 있습니다. 실제 문서 저장/결재 API와 연결된 것은 아닙니다.


## 전자결재 문서 기능 추가

`/documents` 문서함, `/documents/new` 문서 작성, `/documents/[id]` 상세 보기, `/documents/[id]/edit` 수정 경로를 추가했습니다. 기안서·지출결의서·휴가신청서, 결재선 설정, 첨부파일 업로드/다운로드 기능이 포함됩니다. 백엔드에는 `com.example.approval.document` API/서비스/도메인 계층을 추가했습니다.

프론트엔드는 Next.js App Router를 사용하도록 경로와 링크를 조정했으며, `/api/*` 요청은 `next.config.ts`의 rewrite를 통해 `localhost:8080` 백엔드로 전달됩니다.
