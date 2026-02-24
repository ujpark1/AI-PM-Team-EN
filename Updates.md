# Updates — 주요 업데이트 기록

PM Director가 관리하는 주요 업데이트 이력입니다.
모든 중요한 의사결정, 구현 변경, 스펙 수정 사항을 기록합니다.

---

## 2026-02-23 (Phase 2 — 백엔드 구현)

### — [인프라] Phase 2 Step 0: Supabase 인프라 세팅
- **작업**: Supabase 프로젝트 생성 + 유료 플랜 활성화
- **DB 스키마**: 6개 테이블 마이그레이션 (`001_initial_schema.sql`)
  - `profiles` — 유저 확장 정보 (name, plan, GitHub 연동 등)
  - `projects` — 프로젝트 (project_key_hash, monitoring 등)
  - `scans` — 스캔 결과 (grade, checks)
  - `scan_items` — 스캔 항목별 결과
  - `monitoring_logs` — URL 모니터링 로그
  - `runtime_events` — SDK 런타임 이벤트
- **보안**: RLS 정책 (user_id 기반 행 수준 보안) + Auth Trigger
- **환경변수**: `.env.local` + Vercel 동기화

### — [Auth] Phase 2 Step 1: 회원가입/로그인/인증 전체 구현
- **구현 범위**:
  - Supabase SSR 클라이언트 (`@supabase/ssr` — 쿠키 기반 세션)
  - Auth 미들웨어 (보호 라우트 체크)
  - 이메일+비밀번호 회원가입 → 이메일 인증 필수
  - 비밀번호 찾기 → 재설정 이메일 → 새 비밀번호 설정
  - OAuth 콜백 라우트 (Google/GitHub용 기반)
- **이슈 해결**:
  - 이메일 인증 메일 미도착 → Supabase Dashboard에서 Site URL + Redirect URLs 설정으로 해결
  - 이메일 Rate Limit (429) → Supabase 내장 이메일의 고정 제한. Custom SMTP 필요하지만 현재는 대기
- **배포**: Vercel에서 정상 동작 확인

### — [API] Phase 2 Step 2: 프로젝트 CRUD + Project Key
- **API 엔드포인트**:
  - `GET /api/projects` — 내 프로젝트 목록
  - `POST /api/projects` — 프로젝트 생성 (Free 3개 제한 체크)
  - `PATCH /api/projects/[id]` — 프로젝트 수정 (소유권 검증)
  - `DELETE /api/projects/[id]` — 프로젝트 삭제 (소유권 검증)
  - `POST /api/projects/[id]/regenerate-key` — Key 재발급
- **Project Key**: `gr_sk_` + 32자 랜덤, SHA-256 해시 저장, prefix 4자 표시용

### — [연동] Phase 2 Step 9 (부분): Dashboard 실제 데이터 연결
- **의사결정**: 스캔 엔진(Step 4) 전에 UI를 먼저 실제 데이터로 연결하여 Auth→Dashboard 플로우 검증
- **구현**:
  - `UserProvider` — Supabase Auth user + profiles 테이블 React Context
  - `ProjectProvider` — 프로젝트 목록 + 현재 선택 React Context
  - 사이드바 전체 → 실제 데이터 (유저 정보, 프로젝트 목록, 플랜 상태, 로그아웃)
  - New Project Dialog → POST /api/projects 실제 생성
  - Settings → 실제 Supabase 연동 (프로필 수정, 비밀번호 변경, 계정 삭제)
  - Dashboard 상태 분기 → 실제 프로젝트 유무 기반
- **배포**: Vercel 자동 배포 (`https://guardrail-seven.vercel.app`)
- **검증**: `npx next build` 성공

### — [버그 수정] 프로필 이름 저장 실패
- **증상**: Settings > Profile에서 이름 수정 시 "Failed to save profile" 에러
- **원인**: DB 스키마 컬럼명 `name` vs 코드에서 `full_name` 사용 불일치
- **수정 파일**: `user-provider.tsx`, `account-info.tsx`, `sidebar-user-footer.tsx`
- **교훈**: DB 스키마와 코드 간 컬럼명 일치 여부를 반드시 교차 검증

### — [핵심 기능] Phase 2 Step 4: Security Scan Engine
- **의사결정**: GuardRail의 핵심 기능. GitHub 퍼블릭 레포 → 8개 보안 체크 → 등급(A-F) → DB 저장 → UI 실제 데이터 연결
- **아키텍처**: 4계층 (Scan Engine Core → API Routes → UI Wiring → DB)
- **신규 모듈** (`src/lib/scan-engine/` — 6개 파일):
  - `github-fetcher.ts` — GitHub REST API로 레포 파일 트리 + 내용 fetch. 관련 파일만 필터링 (최대 50개), 10개 동시 fetch. `GITHUB_PAT` 환경변수 옵션 지원
  - `checks.ts` — 8개 보안 체크 함수 (순수 문자열 매칭, <100ms)
    1. `stripe_key_exposed` (payment/critical) — Stripe 시크릿 키 노출
    2. `webhook_no_verify` (payment/warning) — Webhook 서명 미검증
    3. `password_plaintext` (auth/critical) — 비밀번호 평문 저장
    4. `no_rate_limit` (auth/warning) — 로그인 Rate Limit 없음
    5. `env_not_gitignored` (secrets/critical) — .env 파일 Git 미제외
    6. `api_key_hardcoded` (secrets/critical) — API 키 하드코딩
    7. `no_https` (infra/warning) — HTTPS 미설정
    8. `no_privacy_policy` (legal/warning) — 프라이버시 정책 없음
  - `grade-calculator.ts` — 등급 산출 (A=8/8, B=7/8+no critical, C=5-7, D=3-4, F=0-2 or ≥2 critical)
  - `fix-guides.ts` — check_key별 양면(pass/fail) 가이드 콘텐츠. How to Fix 모달에 표시되는 "왜 위험한지", "어떻게 고치는지", AI 프롬프트 포함
  - `transformer.ts` — DB 레코드(scans/scan_items) → UI 타입(ScanResult/ScanHistoryItem/DashboardStats) 변환
- **API Routes** (3개):
  - `POST /api/projects/[id]/scan` — 스캔 실행 파이프라인 (Auth → 소유권 → GitHub fetch → 체크 → 등급 → DB 저장 → 결과 반환)
  - `GET /api/projects/[id]/scans` — 스캔 이력 목록 (limit 지원)
  - `GET /api/projects/[id]/scans/[scanId]` — 스캔 상세 (scan_items + fix guides 포함)
- **UI 연결** (7개 기존 파일 수정):
  - `empty-no-scans.tsx` — URL 입력 + Scan 버튼 → API 연결, 로딩/에러 상태, 성공 시 결과 페이지 이동
  - `dashboard/page.tsx` — `hasScans = false` 하드코딩 제거 → 실제 스캔 카운트 체크, Run Scan 버튼 API 연결
  - `scan/[scanId]/page.tsx` — mock 데이터 → API fetch, Rescan 버튼 API 연결
  - `scans/[scanId]/page.tsx` — mock 데이터 → API fetch, 하드코딩 `proj_1` → 동적 params
  - `scans/page.tsx` — server component → client component, mock → API fetch
  - `recent-scans.tsx` — mockRecentScans import 제거 → props 기반
  - `stats-card.tsx` — mockStats import 제거 → props 기반 (Uptime/SSL은 "--" placeholder)
- **제한 사항**: Public repo만 스캔 가능. GitHub OAuth(Step 3) 미구현으로 private repo 접근 불가
- **다음 단계**: Step 3 (GitHub OAuth) 구현 → private repo 스캔 + 유저별 GitHub 연결
- **검증**: `npx next build` 성공

### — [모니터링] Phase 2 Step 6: URL 모니터링 — Vercel Cron
- **의사결정**: Dashboard의 Uptime/SSL 카드가 "--"로 표시 중이던 것을 실제 데이터로 채움. Vercel Cron으로 6시간마다 모니터링, 매일 자동 재스캔.
- **아키텍처**: Vercel Cron → API Route → Admin Supabase Client (RLS 우회) → monitoring_logs 테이블
- **신규 파일** (8개):
  - `vercel.json` — Cron 스케줄 (monitor 6시간, rescan 매일 03:00 UTC)
  - `src/lib/supabase/admin.ts` — Service Role Key Supabase 클라이언트 (Cron은 유저 세션 없으므로 필수)
  - `src/lib/monitoring/uptime.ts` — HTTP HEAD 체크 (상태코드 200-399 = up, 8초 타임아웃)
  - `src/lib/monitoring/ssl.ts` — `https.request()` + `getPeerCertificate()` → 인증서 만료일 파싱
  - `src/lib/monitoring/headers.ts` — 보안 헤더 6종 체크 (HSTS, CSP, X-Frame, X-Content-Type, Referrer-Policy, Permissions-Policy), 점수 0-100
  - `src/app/api/cron/monitor/route.ts` — 통합 모니터링 (Uptime+SSL+Headers, 프로젝트별 3행 batch insert)
  - `src/app/api/cron/rescan/route.ts` — 자동 재스캔 (기존 scan engine 재사용, source="auto", private repo 토큰 복호화)
  - `src/app/api/projects/[id]/monitoring/route.ts` — 프로젝트별 최신 모니터링 데이터 조회
- **수정 파일** (2개):
  - `dashboard/page.tsx` — 모니터링 API fetch 추가, Uptime/SSL 실제 데이터 표시
  - `stats-card.tsx` — Warning(주황)/Critical(빨강) 상태별 색상 추가
- **보안**: `CRON_SECRET` Bearer 토큰으로 Cron 엔드포인트 인증
- **환경변수**: `CRON_SECRET` — `.env.local` + Vercel (production, preview, development)
- **검증**: `npx next build` 성공, Vercel 프로덕션 배포 완료

### — [핵심 기능] Phase 2 Step 5: MCP Server (`guardrail-mcp` npm 패키지)
- **의사결정**: Claude Code에서 직접 보안 스캔을 실행하고 결과를 확인할 수 있는 MCP 서버 구현. MVP는 Project Key 인증 (`GUARDRAIL_PROJECT_KEY` 환경변수 1개로 즉시 사용 가능)
- **아키텍처**: Claude Code ←stdio→ guardrail-mcp (로컬 Node.js) → 로컬 파일 스캔 → 결과 POST → GuardRail API (Vercel)
- **핵심 결정**:
  - 로컬 스캔: GitHub가 아닌 로컬 디렉토리를 스캔 (push 전 체크 가능)
  - 체크 로직 복사: scan-engine의 checks.ts, grade-calculator.ts를 MCP 패키지에 독립 복사 (npm publish 용이)
  - API는 결과 저장만: MCP가 로컬에서 분석 완료 → API에 결과만 POST
- **MCP 패키지** (`packages/guardrail-mcp/` — 9개 신규 파일):
  - `index.ts` — McpServer + StdioServerTransport 진입점
  - 4개 Tool: `guardrail_scan` (로컬 스캔), `guardrail_status` (상태), `guardrail_issues` (이슈), `guardrail_fix` (수정 가이드)
  - `api-client.ts` — Bearer Project Key 인증 HTTP 클라이언트
  - `local-scanner.ts` — fs.readdir recursive, 파일 필터링 (같은 로직), 최대 50개
  - `checks.ts` + `grade.ts` — 8개 보안 체크 + 등급 계산 (scan-engine 복사)
- **API 엔드포인트** (4개 신규 파일):
  - `mcp-auth.ts` — Project Key SHA-256 해시 검증 (Admin Supabase Client, RLS 우회)
  - `POST /api/mcp/scan` — 스캔 결과 수신 + scans/scan_items DB 저장 (source: "mcp")
  - `GET /api/mcp/status` — 최신 등급 + 모니터링 데이터
  - `GET /api/mcp/issues` — 실패 항목 + `?check_key=xxx` fix guide 상세
- **UI 연결** (2개 수정):
  - `mcp-connection.tsx` — 실제 project key prefix + 재생성 API + 동적 MCP config JSON
  - `settings/page.tsx` — ProjectProvider에서 projectId/keyPrefix 전달
- **사용자 플로우**: GuardRail 웹에서 프로젝트 생성 → Key 복사 → Claude Code에 알려주기(1회) → 이후 대화로 스캔/수정 무한반복
- **이슈 해결**: MCP SDK `Server` vs `McpServer` — SDK v1.27.0에서 `.tool()` 메서드는 `McpServer` 클래스에만 존재
- **검증**: MCP 패키지 빌드 + Next.js 빌드 모두 성공

### — [핵심 기능] Phase 2 Step 7: SDK 런타임 모니터링 (`guardrail-sdk` npm 패키지)
- **목적**: 사용자 앱이 실행되는 동안 실시간 위협(브루트포스, 이상 로그인, 트래픽 급증 등) 감지
- **아키텍처**:
  ```
  사용자 앱 (guardrail-sdk 미들웨어)
    → auth.login_failed / auth.login_success / api.request 이벤트 수집
    → 30초마다 배치 POST /api/events
    → Event Analyzer (서버) → 위협 감지 시 monitoring_logs에 alert 생성
    → Dashboard에서 실시간 확인 + 액션 (Block IP, Was this you?)
  ```
- **이미 구현되어 있던 것 (이전 스텝에서):**
  - API: `POST /api/events`, `GET /api/runtime/events`, `POST /api/runtime/block-ip`, `POST /api/runtime/acknowledge`, `GET /api/runtime/blocked-ips`, `POST /api/runtime/unblock-ip`
  - Event Analyzer: `src/lib/event-analyzer/` (4개 감지기 + 오케스트레이터)
  - DB: `runtime_events` 테이블, `projects.sdk_connected` 필드
  - UI 컴포넌트 (mock): `runtime-events.tsx`, `runtime-event-dialog.tsx`, `sdk-setup-prompt.tsx`, `setup-sdk-dialog.tsx`
- **이번에 구현한 것:**
  - **SDK 패키지** (`packages/guardrail-sdk/` — 6개 파일):
    - `types.ts` — SdkEvent, GuardrailConfig, FlushResponse 타입
    - `collector.ts` — 이벤트 큐 + 30초 배치 전송 + blocked IP 관리 (API 실패 시 큐 유지)
    - `index.ts` — Express 미들웨어 (`guardrail()`) — auth 엔드포인트 자동 감지, 차단 IP 403
    - `next.ts` — Next.js 미들웨어 (`guardrailMiddleware()`)
  - **UI 데이터 연결** (3개 수정):
    - `project-provider.tsx` — `ProjectData`에 `sdk_connected: boolean` 추가
    - `runtime-events.tsx` — mock 데이터 제거 → `GET /api/runtime/events` 실제 API 연결
    - `dashboard/page.tsx` — `sdk_connected` 조건부 렌더링 (RuntimeEvents vs SdkSetupPrompt)
  - **SDK 설치 타이밍 프롬프트** (4개 수정):
    - `scan/[scanId]/page.tsx` — 스캔 결과 하단에 SdkSetupPrompt 추가 (sdk_connected=false일 때)
    - `mcp-auth.ts` — `sdk_connected` 필드 select 추가 (McpProject 인터페이스 확장)
    - `api/mcp/scan/route.ts` — 응답에 `sdkConnected` 필드 추가
    - `guardrail-mcp/tools/scan.ts` — SDK 미연결 시 설치 안내 문구 출력
- **설계 결정**:
  - SDK 의존성 0개 (Node.js 내장 `fetch` 사용)
  - Express: `res.end` 래핑으로 응답 상태코드 기반 login_failed/success 자동 감지
  - Next.js: 미들웨어 특성상 응답 접근 불가 → API route에서 직접 이벤트 Push 권장
  - SDK 안내 타이밍: 스캔 결과 확인 직후 = 보안 인식 최고조 시점
- **검증**: SDK 빌드 + MCP 빌드 + Next.js 빌드 모두 성공

### — [배포] npm 패키지 2종 + Vercel 프로덕션 배포 (2026-02-24)
- **guardrail-sdk@0.1.0**: npm publish 완료
  - Express 미들웨어 + Next.js 미들웨어
  - 런타임 보안 모니터링 (브루트포스, 이상 로그인, 트래픽 급증 감지)
  - URL: https://www.npmjs.com/package/guardrail-sdk
- **guardrail-mcp@0.1.0**: npm publish 완료
  - Claude Code MCP 서버 (4개 Tool: scan, status, issues, fix)
  - `npx guardrail-mcp`로 즉시 실행 가능
  - URL: https://www.npmjs.com/package/guardrail-mcp
- **Vercel 웹 앱**: 프로덕션 배포 완료
  - URL: https://guardrail-seven.vercel.app
  - Step 7 UI 변경사항 포함 (RuntimeEvents 실제 데이터, SDK 안내 프롬프트)

### — [핵심 기능] Phase 2 Step 3: GitHub OAuth (Private Repo 스캔 지원)
- **의사결정**: 가입 방법(Google/Email/GitHub)과 무관하게 누구나 "Connect GitHub"로 private repo 스캔 가능
- **아키텍처**: Supabase Auth의 GitHub 로그인과 별도 — 자체 GitHub OAuth App으로 repo 접근 토큰 발급 → AES-256-GCM 암호화 저장
- **신규 파일** (7개):
  - `src/lib/encryption.ts` — AES-256-GCM 암호화/복호화 (ENCRYPTION_KEY 환경변수)
  - `src/app/api/auth/github/route.ts` — OAuth 시작 (CSRF state + GitHub 리다이렉트)
  - `src/app/api/auth/github/callback/route.ts` — 콜백 (코드→토큰 교환 → 암호화 저장)
  - `src/app/api/auth/github/popup-close/route.ts` — 팝업 닫기 (postMessage)
  - `src/app/api/auth/github/disconnect/route.ts` — GitHub 연결 해제
  - `src/app/api/github/repos/route.ts` — 사용자 레포 목록 (토큰 만료 자동 감지/삭제)
  - `src/components/settings/connected-accounts.tsx` — 설정 페이지 GitHub 연결 카드
- **수정 파일** (6개):
  - `user-provider.tsx` — `github_username`, `github_connected_at`, `isGitHubConnected` 추가
  - `new-project-dialog.tsx` — mock OAuth/repos → 실제 팝업 OAuth + API 레포 목록
  - `github-fetcher.ts` — `token?` 파라미터 추가 (유저 토큰 > env PAT > 비인증 우선순위)
  - `scan/route.ts` — 사용자 토큰 복호화 → fetchRepoFiles에 전달
  - `settings/profile/page.tsx` — ConnectedAccounts 컴포넌트 추가
  - `projects/route.ts` — `github_repo_id`, `github_repo_private` 필드 수용
- **두 가지 OAuth 플로우**:
  - Settings 페이지: 풀페이지 리다이렉트 (단순)
  - New Project Dialog: 팝업 (다이얼로그 상태 유지, postMessage 통신)
- **보안**: CSRF state 쿠키, AES-256-GCM 토큰 암호화, HttpOnly 쿠키, 토큰 만료 자동 감지
- **Vercel 배포**: `ENCRYPTION_KEY` 환경변수 추가, 프로덕션 배포 완료
- **검증**: `npx next build` 성공
- **참고**: `GITHUB_CLIENT_ID`, `GITHUB_CLIENT_SECRET`는 GitHub OAuth App 생성 후 설정 필요

---

## 2026-02-23 (Phase 2 착수 전 리뷰)

### 20:00 — [리뷰] Phase 2 착수 전 PM Director 정합성 리뷰 (Feature Spec v1.2 → v1.3)
- **트리거**: Phase 2 개발 착수 전 Feature Spec과 Implementation Plan 간 갭 분석
- **방법**: PM Director(Agent C) + Agent A(전략) + Agent B(검증) 3자 더블 체크
- **발견된 갭 7개 + 해결 내역**:

| # | 갭 | 심각도 | 해결 |
|---|-----|--------|------|
| 1 | F11 Phase 분류 불일치 (Spec=Phase2, Plan=Phase3) | 높음 | Feature Spec에서 Phase 3으로 이관 확정. Phase 2/3 범위 재정의 |
| 2 | Monitoring Dashboard 별도 화면 vs Overview 통합 | 중간 | Overview 통합 확정. Screen 5에 PM Director 결정 사유 기록 |
| 3 | Forgot/Reset Password 화면 미정의 | 중간 | Screen L4, L5 화면 정의 + 와이어프레임 추가 |
| 4 | Email Verification 대기 화면 미정의 | 중간 | Screen L3 화면 정의 + 와이어프레임 추가 |
| 5 | Runtime Event 액션 API 미정의 | 중간 | Implementation Plan Step 7에 3개 API + 동작 방식 추가 |
| 6 | Scan History 화면 와이어프레임 없음 | 낮음 | Screen 3-1 와이어프레임 추가 |
| 7 | Agent C MCP Phase 1 당기기 권고 미적용 | 낮음 | 미적용 사유 기록 (Phase 2 Step 5 우선순위 높이는 것으로 타협) |

- **수정된 문서**:
  - `guardrail-feature-spec.md` v1.2 → **v1.3** (Phase 구분, 빌드 순서, 화면 정의 3개, 라우팅 테이블, Scan History 와이어프레임)
  - `guardrail-implementation-plan.md` (Runtime Event 액션 API 3개 + 파일 구조)
- **추가된 화면**: L3 (Email Verification), L4 (Forgot Password), L5 (Reset Password), 3-1 (Scan History)
- **추가된 라우트**: `/auth/verify-email`, `/auth/forgot-password`, `/auth/reset-password`, `/terms`, `/privacy`
- **결론**: 7개 갭 모두 해소. Phase 2 개발 착수 준비 완료.

---

## 2026-02-23 (Phase 1)

### 09:00 — [구현] Phase 1 프론트엔드 전체 구현 완료
- **범위**: Step 0 ~ Step 7 (타입/목데이터 → 레이아웃 → Landing → Auth → 사이드바 → 대시보드 → 검증)
- **결과**: 28개 Figma 프레임 기반 전체 UI 구현. `npx next build` 성공.
- **스택**: Next.js 16.1.6 App Router + Tailwind CSS v4 + shadcn/ui + next-themes + sonner

### 11:00 — [기능 추가] New Project Dialog — GitHub OAuth 연동
- **의사결정**: Private repo 접근을 위해 GitHub OAuth 연동 방식 채택
- **대안 검토**: 1) GitHub OAuth (선택됨) 2) URL + Personal Token 3) Hybrid
- **구현**: 4-state UI (not_connected → connecting → connected → manual)
  - Connected 상태: 레포 검색/선택 드롭다운 (private 포함)
  - Manual 폴백: public repo URL 직접 입력
- **문서**: `guardrail-feature-spec.md` (root) 신규 생성, outputs 문서 업데이트

### 13:00 — [기능 추가] OAuth 사용자 Change Password 분기
- **변경**: GitHub/Google OAuth 로그인 사용자는 비밀번호 변경 대신 Provider 정보 표시
- **구현**: `authProvider` prop ("email" | "github" | "google")으로 분기
  - Email: 기존 Edit → 3개 입력 → Update Password
  - GitHub: "You signed in with GitHub." + github.com/settings/security 링크
  - Google: "You signed in with Google." + myaccount.google.com/security 링크

### 14:00 — [기능 추가] Profile Settings — Edit/Cancel 패턴
- **변경**: Account Info + Change Password에 Edit 버튼 토글 패턴 적용
- **이전**: 항상 편집 가능한 입력 필드 + Save/Update 버튼
- **이후**: 읽기 모드(텍스트 + Edit) → 편집 모드(입력 + Cancel/Save)
- **효과**: 의도하지 않은 수정 방지, 깔끔한 기본 화면

### 15:00 — [UX 변경] How to Fix 모달 Footer 간소화
- **변경**: Footer 버튼을 "Close"만 남기고 Rescan/Mark as Fixed 제거
- **사유**: MVP 프론트엔드에서 해당 기능 미구현. 동작하지 않는 버튼은 사용자 혼란 유발.

### 15:30 — [인프라] 다크 모드 CSS Custom Properties 구축
- **변경**: `globals.css`에 GuardRail 전용 디자인 토큰 시스템 구축
- **토큰**: `--gr-page`, `--gr-card`, `--gr-text`, `--gr-border`, `--gr-btn` 등 12개
- **모드**: Light/Dark 모드별 값 정의 완료

### 16:00 — [문서] Output 문서 4종 구현 반영 업데이트
- `guardrail-ux-spec.md`: 섹션 9.4 Profile, 섹션 13 OAuth, 섹션 14 Edit/Cancel, How to Fix footer, Changelog
- `guardrail-feature-spec.md`: F2 GitHub 연동, Screen 6 Profile, PM-Dev 합의 테이블
- `guardrail-implementation-plan.md`: 구현 현황 + Figma 차이점 섹션 추가
- `figma-review-28frames.md`: 섹션 6 (구현 대비 차이점) 추가

### 17:00 — [프로세스] PM Director 역할 확장
- **추가 역할**: Progress.md 지속 추적/업데이트, Updates.md 주요 변경 기록
- **문서**: `roles/agent-c-director.md` 업데이트

### 18:00 — [프로세스] AI UX Writer 전담 소유권 및 PM 협업 워크플로우 수립
- **의사결정**: 제품 내 모든 UX 카피는 AI UX Writer가 전담. PM Director/Developer는 카피를 직접 수정하지 않고 UX Writer에게 위임.
- **문서**:
  - `AI-UX-Writer/AI UX Writer.md`: 전담 영역(Ownership), 팀 협업 워크플로우, UX Writing Updates.md 관리 섹션 추가
  - `AI-PM-Team/roles/agent-c-director.md`: R&R에 "UX 카피 위임" 추가, "상시 역할: UX Writer 협업" 섹션 추가

### 18:00 — [UX Writing] 랜딩페이지 카피 UX Writing 원칙 기반 개선
- **작업 주체**: AI UX Writer
- **진단**: 전문 용어(MCP, SDK runtime guard, SSL certificates, brute force) 사용이 Vibe Coders 타겟에 부적합
- **변경 범위**: `features-section.tsx` (Feature 카드 4종 + How it works Step 1), `pricing-section.tsx` (Free/Pro 플랜 기능 목록)
- **주요 변경**:
  - Feature 카드: "MCP Integration" → "Built for Claude Code", "Dashboard" → "Security Dashboard"
  - 설명: 전문 용어 제거, 사용자 혜택 중심 재작성
  - Pricing: "SDK runtime guard" → "Live app protection", "Same..." 반복 제거
- **검증**: `npx next build` 성공
- **기록**: `AI-UX-Writer/UX Writing Updates.md` 생성 — 7개 변경 항목 상세 기록
- **문서 반영**: `guardrail-feature-spec.md`, `guardrail-implementation-plan.md`, `figma-review-28frames.md` 업데이트
