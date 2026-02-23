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

### API (`src/app/api/`)
| 경로 | 메서드 | 설명 |
|------|--------|------|
| `/api/projects` | GET, POST | 프로젝트 목록 조회, 생성 (Free 3개 제한) |
| `/api/projects/[id]` | PATCH, DELETE | 프로젝트 수정, 삭제 (소유권 검증) |
| `/api/projects/[id]/regenerate-key` | POST | Project Key 재발급 |

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
| `middleware.ts` | Auth 세션 갱신 유틸 |

### Middleware
| 파일 | 설명 |
|------|------|
| `src/middleware.ts` | Auth 미들웨어 (보호 라우트 체크) |

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
| `guardrail-feature-spec.md` | 2026-02-23 | 구현 반영 완료 (F2, Screen 6, PM-Dev 테이블) |
| `guardrail-implementation-plan.md` | 2026-02-23 | Phase 2 구현 현황 업데이트 (Step 0~2, Step 9 부분 완료 + 버그 수정 기록) |
| `figma-review-28frames.md` | 2026-02-23 | 섹션 6 (구현 대비 차이점) 추가 |
| `guardrail-feature-spec.md` (root) | 2026-02-23 | GitHub OAuth + Profile OAuth 정책 포함 |
| `progress.md` | 2026-02-23 | Phase 2 타임라인 + 컴포넌트 목록 업데이트 |
| `Updates.md` | 2026-02-23 | Phase 2 주요 업데이트 기록 추가 |
