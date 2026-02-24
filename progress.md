# Progress — 개발 진행 기록

개발 시작 이후 주요 진행 사항을 추적합니다.

---

## 타임라인

### 2026-02-23

- [x] **Plan 승인** — Phase 1 전체 구현 계획 (8 Steps) 승인됨
- [x] **Step 0** — Learning.md, progress.md 생성
- [x] **Step 1** — 타입 확장 (`scan.ts`) + 목 데이터 (`mock-data.ts`)
- [x] **Step 2** — 루트 레이아웃 (`layout.tsx` — ThemeProvider, Toaster, metadata)
- [x] **Step 3** — Landing Page (Navbar, Hero, Features, Pricing, Footer)
- [x] **Step 4** — Auth 페이지 (Login, Sign Up)
- [x] **Step 5** — 대시보드 레이아웃 + 사이드바 (AppSidebar, ProjectSwitcher, SidebarNav, PlanCard, UserFooter, AccountMenu)
- [x] **Step 6** — 대시보드 페이지들 (전체 완료)
  - [x] 6a. Overview (3가지 상태: No Projects, No Scans, 정상)
  - [x] 6b. Scan Result (Summary + Issues + Passed Checks + Grade Guide + How to Fix 모달 7종)
  - [x] 6c. Scans History (테이블 6행)
  - [x] 6d. Project Settings (General, MCP Connection, Danger Zone)
  - [x] 6e. Profile Settings (Account Info, Appearance, Change Password, Delete Account)
  - [x] 6f. Plan & Billing (Free/Pro 카드 + Billing Info)
  - [x] Empty States (No Projects, No Scans, No Monitoring)
  - [x] 다이얼로그: New Project, Setup SDK, How to Fix (7종), Runtime Events (4종), Account Menu
- [x] **Step 7** — 검증: `npx next build` 성공 확인
- [x] **추가 구현** — GitHub OAuth UI (New Project Dialog 4-state)
- [x] **추가 구현** — Profile Edit/Cancel 패턴 (Account Info, Change Password)
- [x] **추가 구현** — OAuth 사용자 Change Password 분기 (GitHub/Google provider 처리)
- [x] **추가 구현** — 다크 모드 CSS Custom Properties (`globals.css` gr-* 토큰)
- [x] **문서 동기화** — 4개 outputs 문서 구현 결과와 비교 및 업데이트 완료
- [x] **프로세스** — AI UX Writer 전담 소유권 수립 + PM Director 위임 워크플로우 정의
- [x] **UX Writing** — 랜딩페이지 카피 개선 (전문 용어 제거, 사용자 혜택 중심 재작성) + UX Writing Updates.md 생성
- [x] **문서 동기화** — UX Writing 변경 반영: feature-spec, implementation-plan, figma-review 업데이트

### 2026-02-23 (Phase 2 — 백엔드 + 실제 동작)

- [x] **Phase 2 Step 0** — Supabase 인프라 세팅
  - Supabase 프로젝트 생성 (유료 플랜)
  - DB 스키마 6개 테이블 마이그레이션 (`001_initial_schema.sql`)
  - RLS 정책 설정 (user_id 기반)
  - Auth Trigger (auth.users → profiles 자동 생성)
  - 환경변수 설정 (.env.local + Vercel)
- [x] **Phase 2 Step 1** — Auth (회원가입/로그인/인증)
  - Supabase SSR 클라이언트 (`client.ts`, `server.ts`)
  - Auth 미들웨어 (보호 라우트)
  - Login/Signup → 실제 Supabase Auth 연동
  - 이메일 인증 (verify-email 화면 + confirm 콜백)
  - 비밀번호 찾기/재설정 (forgot-password + reset-password + reset-callback)
  - OAuth 콜백 라우트
  - Supabase Site URL + Redirect URLs 설정
- [x] **Phase 2 Step 2** — 프로젝트 CRUD + Project Key
  - API: GET/POST `/api/projects`, PATCH/DELETE `/api/projects/[id]`
  - Project Key: `gr_sk_` + 32자 랜덤, SHA-256 해시 저장
  - Free 플랜 3개 제한 체크
  - Key 재발급 API (`/api/projects/[id]/regenerate-key`)
- [x] **Phase 2 Step 9 (부분)** — Dashboard 실제 데이터 연결
  - `UserProvider` — Supabase Auth user + profiles 테이블 Context
  - `ProjectProvider` — 프로젝트 목록 + 현재 선택 Context
  - 사이드바 전체 → 실제 데이터 (유저 정보, 프로젝트 목록, 플랜, 로그아웃)
  - New Project Dialog → POST /api/projects 실제 생성
  - Settings (Account Info, Change Password, Delete Account) → 실제 Supabase 연동
  - Dashboard 상태 분기 → 실제 프로젝트 기반
  - Vercel 배포 완료 (`https://guardrail-seven.vercel.app`)
- [x] **버그 수정** — 프로필 이름 저장 실패 (DB 컬럼 `name` vs 코드 `full_name` 불일치)
- [x] **Phase 2 Step 4** — Security Scan Engine (핵심 기능)
  - Scan Engine Core (`src/lib/scan-engine/` 6개 파일)
    - `github-fetcher.ts` — GitHub URL 파싱 + 레포 파일 fetch (비인증 API, GITHUB_PAT 옵션)
    - `checks.ts` — 8개 보안 체크 함수 (stripe_key, webhook, password, rate_limit, env_gitignore, api_key, https, privacy)
    - `grade-calculator.ts` — 등급 산출 (A-F)
    - `fix-guides.ts` — check_key별 양면(pass/fail) 가이드 콘텐츠 (How to Fix 모달용)
    - `transformer.ts` — DB 레코드 → ScanResult/ScanHistoryItem/DashboardStats 변환
    - `index.ts` — Barrel export
  - API Routes 3개
    - `POST /api/projects/[id]/scan` — 스캔 실행 (GitHub fetch → 체크 → 등급 → DB 저장)
    - `GET /api/projects/[id]/scans` — 스캔 이력 목록
    - `GET /api/projects/[id]/scans/[scanId]` — 스캔 상세
  - UI 연결 (7개 파일 수정)
    - `empty-no-scans.tsx` — URL 입력 + Scan 버튼 → API 연결, 로딩 상태
    - `dashboard/page.tsx` — `hasScans = false` → 실제 스캔 카운트, Run Scan 버튼
    - `scan/[scanId]/page.tsx` — mock → API fetch, Rescan 연결
    - `scans/[scanId]/page.tsx` — mock → API fetch, 하드코딩 `proj_1` 수정
    - `scans/page.tsx` — mock → API fetch (server→client component 전환)
    - `recent-scans.tsx` — mockRecentScans → props 기반
    - `stats-card.tsx` — mockStats → props 기반 (Uptime/SSL은 "--" placeholder)
  - `npx next build` 성공
  - **제한 사항**: Public repo만 스캔 가능 (GitHub OAuth 미구현 → Step 3에서 해결됨)
- [x] **Phase 2 Step 3** — GitHub OAuth (Connect GitHub for Private Repo Scanning)
  - 암호화 유틸리티 (`src/lib/encryption.ts` — AES-256-GCM)
  - OAuth API Routes 5개
    - `GET /api/auth/github` — OAuth 시작 (GitHub 리다이렉트, CSRF state)
    - `GET /api/auth/github/callback` — 콜백 처리 (코드→토큰 교환, 암호화 저장)
    - `GET /api/auth/github/popup-close` — 팝업 닫기 (postMessage)
    - `POST /api/auth/github/disconnect` — GitHub 연결 해제
    - `GET /api/github/repos` — 사용자 레포 목록 (토큰 만료 자동 감지)
  - UI 연결 (6개 파일 수정)
    - `user-provider.tsx` — `github_username`, `github_connected_at`, `isGitHubConnected` 추가
    - `connected-accounts.tsx` — 설정 페이지 GitHub 연결 카드 (신규)
    - `settings/profile/page.tsx` — ConnectedAccounts 컴포넌트 추가
    - `new-project-dialog.tsx` — mock OAuth/repos → 실제 팝업 OAuth + API 레포 목록
    - `github-fetcher.ts` — `token?` 파라미터 추가 (모든 함수에 전파)
    - `scan/route.ts` — 사용자 토큰 복호화 → fetchRepoFiles에 전달
  - `npx next build` 성공
  - Vercel 프로덕션 배포 완료 (`https://guardrail-seven.vercel.app`)
- [x] **Phase 2 Step 6** — URL 모니터링 (Vercel Cron: Uptime/SSL/Headers/Auto-Rescan)
  - Vercel Cron 설정 (`vercel.json` — monitor 6시간, rescan 매일 03:00 UTC)
  - Admin Supabase 클라이언트 (`src/lib/supabase/admin.ts` — Service Role Key, RLS 우회)
  - 모니터링 체크 함수 3종 (`src/lib/monitoring/`)
    - `uptime.ts` — HTTP HEAD 체크 (상태코드 + 응답시간)
    - `ssl.ts` — HTTPS 인증서 만료일 파싱 (`https.request` + `getPeerCertificate`)
    - `headers.ts` — 보안 헤더 6종 체크 (HSTS, CSP, X-Frame 등, 점수 0-100)
  - Cron API Routes 2개
    - `GET /api/cron/monitor` — Uptime+SSL+Headers 통합 (프로젝트별 3행 batch insert)
    - `GET /api/cron/rescan` — GitHub 자동 재스캔 (scan engine 재사용, source="auto")
  - 모니터링 데이터 API
    - `GET /api/projects/[id]/monitoring` — type별 최신 로그 + uptime 가용률(%)
  - Dashboard 연결
    - `dashboard/page.tsx` — 모니터링 API fetch + Uptime/SSL 실제 데이터 표시
    - `stats-card.tsx` — Warning/Critical 상태별 색상 (빨강/주황/초록)
  - `npx next build` 성공
  - Vercel 프로덕션 배포 완료 + Cron Jobs 등록
- [x] **Phase 2 Step 5** — MCP Server (`guardrail-mcp` npm 패키지)
  - MCP 패키지 (`packages/guardrail-mcp/` — 9개 파일)
    - `index.ts` — MCP 서버 진입점 (McpServer + StdioServerTransport)
    - `tools/scan.ts` — `guardrail_scan` 로컬 디렉토리 보안 스캔
    - `tools/status.ts` — `guardrail_status` 프로젝트 상태 조회
    - `tools/issues.ts` — `guardrail_issues` 이슈 목록
    - `tools/fix.ts` — `guardrail_fix` 이슈별 수정 가이드
    - `lib/api-client.ts` — GuardRail API HTTP 클라이언트 (Bearer Project Key 인증)
    - `lib/local-scanner.ts` — 로컬 파일시스템 스캔 (fs.readdir recursive)
    - `lib/checks.ts` — 8개 보안 체크 (scan-engine에서 복사, 독립 실행)
    - `lib/grade.ts` — 등급 계산 (scan-engine에서 복사)
  - API 엔드포인트 (4개 파일)
    - `src/lib/mcp-auth.ts` — Project Key SHA-256 해시 검증 미들웨어
    - `POST /api/mcp/scan` — MCP 스캔 결과 수신 + DB 저장 (source: "mcp")
    - `GET /api/mcp/status` — 프로젝트 상태 (최신 등급 + 모니터링)
    - `GET /api/mcp/issues` — 이슈 목록 + ?check_key=xxx fix guide
  - UI 연결 (2개 파일 수정)
    - `mcp-connection.tsx` — 실제 project key prefix 표시 + 재생성 + 동적 MCP config JSON
    - `settings/page.tsx` — ProjectProvider 컨텍스트에서 projectId/keyPrefix 전달
  - `npm run build` (MCP 패키지) + `npx next build` (Next.js) 모두 성공
- [x] **Phase 2 Step 7** — SDK 런타임 모니터링 (`guardrail-sdk` npm 패키지)
  - SDK 패키지 (`packages/guardrail-sdk/` — 6개 파일)
    - `src/types.ts` — SdkEvent, GuardrailConfig, FlushResponse 타입
    - `src/collector.ts` — 이벤트 큐 + 30초 배치 전송 + blocked IP 관리
    - `src/index.ts` — Express 미들웨어 (auth 엔드포인트 자동 감지, IP 차단)
    - `src/next.ts` — Next.js 미들웨어
  - UI 데이터 연결 (3개 파일 수정)
    - `project-provider.tsx` — `ProjectData`에 `sdk_connected: boolean` 추가
    - `runtime-events.tsx` — mock 데이터 → `GET /api/runtime/events` 실제 API 연결
    - `dashboard/page.tsx` — `sdk_connected` 조건부 렌더링 (RuntimeEvents vs SdkSetupPrompt)
  - SDK 설치 타이밍 프롬프트 (4개 파일 수정)
    - `scan/[scanId]/page.tsx` — 스캔 결과 하단에 SdkSetupPrompt 추가 (sdk_connected=false)
    - `mcp-auth.ts` — `sdk_connected` 필드 select 추가
    - `api/mcp/scan/route.ts` — 응답에 `sdkConnected` 필드 추가
    - `guardrail-mcp/tools/scan.ts` — SDK 미연결 시 안내 문구 출력
  - `npm run build` (SDK + MCP + Next.js) 모두 성공

### 2026-02-24

- [x] **배포** — npm 패키지 2종 + Vercel 웹 앱 프로덕션 배포
  - `guardrail-sdk@0.1.0` → npm publish 완료 (https://www.npmjs.com/package/guardrail-sdk)
  - `guardrail-mcp@0.1.0` → npm publish 완료 (https://www.npmjs.com/package/guardrail-mcp)
  - Vercel 프로덕션 배포 완료 (https://guardrail-seven.vercel.app)
- [x] **Phase 2 Step 8 (부분)** — Stripe 결제 연동 (Checkout)
  - Stripe Dashboard에서 Product + Price 생성 (테스트 모드)
  - `POST /api/stripe/checkout` — Checkout Session 생성 (14일 무료 체험)
    - Stripe SDK 대신 **직접 `fetch` API** 사용 (Vercel 서버리스 환경 호환)
    - Stripe Customer 자동 생성 + `stripe_customer_id` DB 저장
  - `src/lib/stripe.ts` — `Stripe.createFetchHttpClient()` 옵션 적용 (webhook/portal용)
  - Plan & Billing "Start Free Trial" → Stripe Checkout 페이지 리다이렉트 동작 확인
  - Vercel 환경변수: `STRIPE_SECRET_KEY`, `NEXT_PUBLIC_STRIPE_PRO_PRICE_ID` 설정
  - **미구현**: Portal, Webhook (구독 상태 동기화)
- [x] **버그 수정** — 회원가입 중복 이메일 처리
  - 증상: 이미 존재하는 이메일로 가입 시도 시 에러 없이 이메일 인증 페이지로 이동
  - 원인: Supabase `signUp()`이 이미 존재하는 이메일에 대해 에러를 반환하지 않음 (이메일 열거 공격 방지)
  - 수정: `data.user?.identities?.length === 0` 체크 → "An account with this email already exists" 에러 표시
  - 수정 파일: `src/app/signup/page.tsx`
- [x] **학습 기록** — `CLAUDE.md` 생성 + `Learning.md` 통합
  - Vercel 환경변수 (`echo` → `printf`), Stripe 통합, Supabase Auth, 배포 교훈 등 기록
  - `AI-PM-Team/Learning.md` 내용을 `guardrail/CLAUDE.md`에 통합
- [x] **Vercel 프로덕션 배포** — 다수 배포 (Stripe 수정 + 중복 가입 수정 포함)

---

## 구현 완료 컴포넌트 목록

### 레이아웃 (`src/components/layout/`)
| 파일 | 설명 |
|------|------|
| `app-sidebar.tsx` | 전체 사이드바 |
| `project-switcher.tsx` | 프로젝트 드롭다운 + New Project |
| `sidebar-nav.tsx` | 4개 네비게이션 |
| `sidebar-plan-card.tsx` | Free Plan 사용량 카드 |
| `sidebar-user-footer.tsx` | 유저 프로필 + Account Menu |
| `account-menu.tsx` | Account 드롭다운 메뉴 |

### 랜딩 (`src/components/landing/`)
| 파일 | 설명 |
|------|------|
| `navbar.tsx` | 랜딩 네비게이션 |
| `hero-section.tsx` | 히어로 섹션 |
| `features-section.tsx` | Why Guardrail? + How it works |
| `pricing-section.tsx` | Free vs Pro 카드 |
| `footer.tsx` | 푸터 |

### 대시보드 (`src/components/dashboard/`)
| 파일 | 설명 |
|------|------|
| `stats-card.tsx` | 통계 카드 4종 |
| `runtime-events.tsx` | Runtime Events 리스트 |
| `runtime-event-dialog.tsx` | Runtime 모달 4종 |
| `recent-scans.tsx` | Recent Scans 테이블 |
| `security-grade-guide.tsx` | A-F 등급 가이드 |
| `sdk-setup-prompt.tsx` | SDK 설치 안내 |
| `setup-sdk-dialog.tsx` | Setup SDK 다이얼로그 |
| `empty-no-projects.tsx` | Empty State — No Projects |
| `empty-no-scans.tsx` | Empty State — No Scans |
| `empty-no-monitoring.tsx` | Empty State — No Monitoring |

### 스캔 (`src/components/scan/`)
| 파일 | 설명 |
|------|------|
| `scan-summary-card.tsx` | 등급 요약 카드 |
| `issues-section.tsx` | Issues to Fix |
| `issue-card.tsx` | 이슈 카드 |
| `passed-checks-section.tsx` | Passed Checks |
| `passed-check-item.tsx` | 통과 항목 행 |
| `how-to-fix-dialog.tsx` | How to Fix 모달 (7종 공용) |

### 설정 (`src/components/settings/`)
| 파일 | 설명 |
|------|------|
| `general-settings.tsx` | 프로젝트 General |
| `mcp-connection.tsx` | MCP Connection |
| `danger-zone.tsx` | Danger Zone |
| `account-info.tsx` | Profile — Account Info (Edit/Cancel 패턴) |
| `appearance-card.tsx` | Profile — Appearance (Light/Dark) |
| `change-password.tsx` | Profile — Change Password (OAuth 분기) |
| `delete-account.tsx` | Profile — Danger Zone |
| `plan-billing-cards.tsx` | Plan & Billing |

### 다이얼로그 (`src/components/dialogs/`)
| 파일 | 설명 |
|------|------|
| `new-project-dialog.tsx` | New Project 다이얼로그 (GitHub OAuth 4-state) |

### 페이지 (`src/app/`)
| 경로 | 파일 | 설명 |
|------|------|------|
| `/` | `page.tsx` | Landing Page |
| `/login` | `login/page.tsx` | Login (Supabase Auth 연동) |
| `/signup` | `signup/page.tsx` | Sign Up (Supabase Auth 연동) |
| `/dashboard` | `(dashboard)/dashboard/page.tsx` | Overview (실제 프로젝트 데이터) |
| `/project/[id]/scan/[scanId]` | 해당 `page.tsx` | Scan Result |
| `/project/[id]/scans` | 해당 `page.tsx` | Scans History |
| `/project/[id]/settings` | 해당 `page.tsx` | Project Settings |
| `/settings/profile` | 해당 `page.tsx` | Profile Settings (실제 프로필 수정) |
| `/settings/billing` | 해당 `page.tsx` | Plan & Billing |
| `(dashboard)` | `layout.tsx` | 대시보드 레이아웃 (UserProvider + ProjectProvider) |

### Auth (`src/app/auth/`)
| 경로 | 파일 | 설명 |
|------|------|------|
| `/auth/callback` | `callback/route.ts` | OAuth 콜백 처리 |
| `/auth/confirm` | `confirm/route.ts` | 이메일 인증 콜백 |
| `/auth/reset-callback` | `reset-callback/route.ts` | 비밀번호 재설정 콜백 |
| `/auth/forgot-password` | `forgot-password/page.tsx` | 비밀번호 찾기 화면 |
| `/auth/reset-password` | `reset-password/page.tsx` | 비밀번호 재설정 화면 |
| `/auth/verify-email` | `verify-email/page.tsx` | 이메일 인증 대기 화면 |

### MCP 패키지 (`packages/guardrail-mcp/`)
| 파일 | 설명 |
|------|------|
| `src/index.ts` | MCP 서버 진입점 (McpServer + StdioServerTransport) |
| `src/tools/scan.ts` | `guardrail_scan` — 로컬 보안 스캔 |
| `src/tools/status.ts` | `guardrail_status` — 프로젝트 상태 조회 |
| `src/tools/issues.ts` | `guardrail_issues` — 이슈 목록 |
| `src/tools/fix.ts` | `guardrail_fix` — 수정 가이드 |
| `src/lib/api-client.ts` | GuardRail API HTTP 클라이언트 |
| `src/lib/local-scanner.ts` | 로컬 파일시스템 스캔 |
| `src/lib/checks.ts` | 8개 보안 체크 (독립 실행) |
| `src/lib/grade.ts` | 등급 계산 (독립 실행) |

### SDK 패키지 (`packages/guardrail-sdk/`)
| 파일 | 설명 |
|------|------|
| `src/types.ts` | SdkEvent, GuardrailConfig, FlushResponse 타입 |
| `src/collector.ts` | 이벤트 큐 + 30초 배치 전송 + blocked IP 관리 |
| `src/index.ts` | Express 미들웨어 (auth 자동 감지, IP 차단) |
| `src/next.ts` | Next.js 미들웨어 |

### Event Analyzer (`src/lib/event-analyzer/`)
| 파일 | 설명 |
|------|------|
| `index.ts` | 이벤트 분석 오케스트레이터 (raw 저장 + 위협 감지 + sdk_connected 갱신) |
| `brute-force.ts` | 같은 IP 5분 내 5+ 실패 감지 |
| `admin-access.ts` | 비정상 시간대 로그인 감지 |
| `traffic-spike.ts` | 요청량 5x 이상 급증 감지 |
| `new-location.ts` | 미확인 국가 최초 로그인 감지 |

### MCP Auth (`src/lib/`)
| 파일 | 설명 |
|------|------|
| `mcp-auth.ts` | Project Key SHA-256 해시 검증 미들웨어 |

### API (`src/app/api/`)
| 경로 | 메서드 | 설명 |
|------|--------|------|
| `/api/mcp/scan` | POST | MCP 스캔 결과 수신 + DB 저장 |
| `/api/mcp/status` | GET | 프로젝트 상태 (등급 + 모니터링) |
| `/api/mcp/issues` | GET | 이슈 목록 + fix guide |
| `/api/projects` | GET, POST | 프로젝트 목록 조회, 생성 (Free 3개 제한) |
| `/api/projects/[id]` | PATCH, DELETE | 프로젝트 수정, 삭제 (소유권 검증) |
| `/api/projects/[id]/regenerate-key` | POST | Project Key 재발급 |
| `/api/projects/[id]/scan` | POST | 보안 스캔 실행 (GitHub fetch → 8체크 → 등급 → DB 저장) |
| `/api/projects/[id]/scans` | GET | 스캔 이력 목록 (limit 지원) |
| `/api/projects/[id]/scans/[scanId]` | GET | 스캔 상세 (scan + scan_items → ScanResult 변환) |
| `/api/projects/[id]/monitoring` | GET | 프로젝트 모니터링 데이터 (uptime/ssl/headers 최신 + 가용률%) |
| `/api/cron/monitor` | GET | Cron: Uptime+SSL+Headers 통합 모니터링 (6시간마다) |
| `/api/cron/rescan` | GET | Cron: GitHub 자동 재스캔 (매일 03:00 UTC) |
| `/api/events` | POST | SDK 이벤트 수신 (Project Key 인증, 위협 분석, DB 저장) |
| `/api/runtime/events` | GET | 런타임 이벤트 목록 (유저 인증, monitoring_logs 조회) |
| `/api/runtime/block-ip` | POST | IP 차단 (유저 인증) |
| `/api/runtime/acknowledge` | POST | 이벤트 확인/거부 (confirm/deny) |
| `/api/runtime/blocked-ips` | GET | 차단된 IP 목록 |
| `/api/runtime/unblock-ip` | POST | IP 차단 해제 |
| `/api/stripe/checkout` | POST | Stripe Checkout Session 생성 (14일 무료 체험) |

### Providers (`src/providers/`)
| 파일 | 설명 |
|------|------|
| `user-provider.tsx` | Supabase Auth user + profiles Context |
| `project-provider.tsx` | 프로젝트 목록 + 현재 선택 Context |

### Supabase (`src/lib/supabase/`)
| 파일 | 설명 |
|------|------|
| `client.ts` | 브라우저용 Supabase 클라이언트 |
| `server.ts` | 서버용 Supabase 클라이언트 (쿠키 기반) |
| `admin.ts` | Admin 클라이언트 (Service Role Key, RLS 우회 — Cron용) |
| `middleware.ts` | Auth 세션 갱신 유틸 |

### Middleware
| 파일 | 설명 |
|------|------|
| `src/middleware.ts` | Auth 미들웨어 (보호 라우트 체크) |

### Monitoring (`src/lib/monitoring/`)
| 파일 | 설명 |
|------|------|
| `uptime.ts` | HTTP HEAD 가용성 체크 (상태코드 + 응답시간) |
| `ssl.ts` | SSL 인증서 만료일 체크 (https.request + getPeerCertificate) |
| `headers.ts` | 보안 헤더 6종 체크 (점수 0-100) |

### Scan Engine (`src/lib/scan-engine/`)
| 파일 | 설명 |
|------|------|
| `github-fetcher.ts` | GitHub URL 파싱 + 레포 파일 트리/내용 fetch |
| `checks.ts` | 8개 보안 체크 함수 (순수 문자열 매칭) |
| `grade-calculator.ts` | A-F 등급 산출 알고리즘 |
| `fix-guides.ts` | check_key별 양면(pass/fail) 가이드 콘텐츠 |
| `transformer.ts` | DB 레코드 → ScanResult/ScanHistoryItem/DashboardStats 변환 |
| `index.ts` | Barrel export |

### DB Migration
| 파일 | 설명 |
|------|------|
| `supabase/migrations/001_initial_schema.sql` | 6개 테이블 + RLS + Auth Trigger |

---

## Figma 대비 주요 차이점

| 항목 | Figma 원안 | 실제 구현 | 사유 |
|------|-----------|----------|------|
| New Project Dialog | 단순 3필드 | GitHub OAuth 4-state + 레포 검색 | Private repo 접근 |
| Account Info | 항상 편집 | Edit/Cancel 토글 패턴 | 의도하지 않은 수정 방지 |
| Change Password | 3필드 + Update | Email: Edit/Cancel; OAuth: Provider 정보 | OAuth 사용자 대응 |
| How to Fix Footer | Close + Rescan (+ Mark as Fixed) | Close만 | MVP 기능 범위 |
| 다크 모드 | 미정의 | CSS Custom Properties 완전 구현 | Appearance 카드 지원 |

---

## 발견된 이슈

- **how-to-fix-dialog.tsx 외부 수정**: Footer 버튼이 Close만 남도록 수정됨 (Rescan, Mark as Fixed 제거). 미사용 `Severity` import 제거, `toast` import 추가.

---

## 문서 동기화 현황

| 문서 | 최종 업데이트 | 상태 |
|------|-------------|------|
| `guardrail-ux-spec.md` | 2026-02-23 | 구현 반영 완료 (섹션 9.4, 13, 14, Changelog) |
| `guardrail-feature-spec.md` | 2026-02-24 | Phase 2 Step 8 Stripe Checkout 구현 반영 |
| `guardrail-implementation-plan.md` | 2026-02-24 | Phase 2 구현 현황 전체 업데이트 (Step 3~8 완료 반영) |
| `figma-review-28frames.md` | 2026-02-24 | 섹션 6.7 Stripe Checkout 구현 차이점 추가 |
| `guardrail-feature-spec.md` (root) | 2026-02-23 | GitHub OAuth + Profile OAuth 정책 포함 |
| `progress.md` | 2026-02-24 | Phase 2 Step 8 + 버그 수정 + 학습 기록 추가 |
| `Updates.md` | 2026-02-24 | Phase 2 Step 8 Stripe + 버그 수정 업데이트 기록 추가 |
| `CLAUDE.md` | 2026-02-24 | 프로젝트 교훈 통합 문서 생성 (Learning.md 병합) |
