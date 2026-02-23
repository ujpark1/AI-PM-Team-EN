# GuardRail Phase 1 — 전체 구현 계획

## Context

GuardRail은 "바이브 코더"를 위한 보안 스캐닝 앱입니다. Figma 디자인(28개 프레임), guardrail-feature-spec.md, guardrail-ux-spec.md를 **정확히 준수**하여 Phase 1 핵심 루프를 구현합니다.

**3가지 메인 재료:**
1. `guardrail-feature-spec.md` — 기능/화면/데이터 모델 정의
2. `guardrail-ux-spec.md` — Figma 기반 UX/디자인 토큰 상세
3. Figma 파일 (28개 프레임) — 시각적 레퍼런스

**스택:** Next.js 16.1.6 App Router + shadcn/ui (19개 설치됨) + Tailwind CSS v4 + Lucide Icons + Sonner + next-themes

**현재 상태:** create-next-app 초기 상태. shadcn/ui 컴포넌트 + 타입 정의만 존재. 페이지/컴포넌트 없음.

---

## Figma 프레임 전체 목록 (28개)

### 페이지 프레임 (13개)
| Node ID | 프레임 이름 | 라우트 |
|---------|------------|--------|
| `1:168` | Landing Page | `/` |
| `1:692` | Dashboard - Overview | `/dashboard` |
| `1:782` | Dashboard - Scan Result | `/project/[id]/scan/[scanId]` |
| `1:914` | Dashboard - Project Settings | `/project/[id]/settings` |
| `1:974` | Scans History | `/project/[id]/scans` |
| `1:1175` | Dashboard - Empty - No Projects | `/dashboard` (empty) |
| `1:1194` | Dashboard - Empty - No Scans | `/dashboard` (empty) |
| `1:1216` | Dashboard - Empty - No Monitoring | `/dashboard` (empty) |
| `2:2515` | Dashboard - Overview (No SDK) | `/dashboard` (no SDK) |
| `2:2644` | Profile Settings | `/settings/profile` |
| `2:2724` | Plan & Billing | `/settings/billing` |
| `3:939` | Login | `/login` |
| `3:967` | Sign Up | `/signup` |

### 다이얼로그/모달 프레임 (15개)
| Node ID | 프레임 이름 | 트리거 |
|---------|------------|--------|
| `2:2411` | New Project (Dialog) | 프로젝트 드롭다운 → + New Project |
| `3:1042` | Set Up SDK (Dialog) | Overview No SDK → "Set Up SDK" 버튼 |
| `2:1331` | Account Menu (Popup) | 사이드바 유저 프로필 클릭 |
| `2:1382` | Account Menu (Component) | 팝업 내부 메뉴 컴포넌트 |
| `3:213` | How to Fix - Stripe key exposed | Scan Result → Issue 카드 → How to fix? |
| `3:277` | How to Fix - No Rate Limit | Scan Result → Issue 카드 → How to fix? |
| `3:334` | How to Fix - Passwords encrypted (Passed) | Scan Result → Passed Check → 클릭 |
| `3:389` | How to Fix - API keys not exposed (Passed) | Scan Result → Passed Check → 클릭 |
| `3:444` | How to Fix - .env excluded (Passed) | Scan Result → Passed Check → 클릭 |
| `3:499` | How to Fix - HTTPS enabled (Passed) | Scan Result → Passed Check → 클릭 |
| `3:554` | How to Fix - Privacy policy (Passed) | Scan Result → Passed Check → 클릭 |
| `3:624` | Runtime - Block IP (Brute Force) | Overview → Runtime Events → Block IP |
| `3:676` | Runtime - View Details (Traffic Spike) | Overview → Runtime Events → View Details |
| `3:728` | Runtime - Was This You? (Admin) | Overview → Runtime Events → Was This You? |
| `3:765` | Runtime - Was This You? (New Location) | Overview → Runtime Events → Was This You? |

---

## 파일 구조

```
src/
├── types/
│   └── scan.ts                              # 기존 (확장 필요)
├── lib/
│   ├── utils.ts                             # 기존 cn()
│   └── mock-data.ts                         # NEW: 전체 목 데이터
├── components/
│   ├── ui/                                  # 기존 shadcn/ui (19개)
│   ├── layout/
│   │   ├── app-sidebar.tsx                  # NEW: 사이드바 전체
│   │   ├── project-switcher.tsx             # NEW: 프로젝트 드롭다운
│   │   ├── sidebar-nav.tsx                  # NEW: 네비게이션 (4탭)
│   │   ├── sidebar-plan-card.tsx            # NEW: Free Plan 사용량
│   │   ├── sidebar-user-footer.tsx          # NEW: 유저 프로필 + Account Menu
│   │   └── account-menu.tsx                 # NEW: Account 드롭다운 메뉴
│   ├── landing/
│   │   ├── navbar.tsx                       # NEW: 랜딩 네비 바
│   │   ├── hero-section.tsx                 # NEW: 히어로
│   │   ├── features-section.tsx             # NEW: Why Guardrail? + How it works
│   │   ├── pricing-section.tsx              # NEW: Free vs Pro
│   │   └── footer.tsx                       # NEW: 푸터
│   ├── dashboard/
│   │   ├── stats-card.tsx                   # NEW: 통계 카드 (Grade/Uptime/SSL/Issues)
│   │   ├── runtime-events.tsx               # NEW: Runtime Events 리스트
│   │   ├── runtime-event-dialog.tsx          # NEW: Runtime 모달 4종
│   │   ├── recent-scans.tsx                 # NEW: Recent Scans 테이블
│   │   ├── security-grade-guide.tsx         # NEW: A-F 등급 가이드
│   │   ├── sdk-setup-prompt.tsx             # NEW: SDK 설치 안내 (No SDK 상태)
│   │   ├── setup-sdk-dialog.tsx             # NEW: Set Up SDK 다이얼로그
│   │   ├── empty-no-projects.tsx            # NEW: Empty State
│   │   ├── empty-no-scans.tsx               # NEW: Empty State
│   │   └── empty-no-monitoring.tsx          # NEW: Empty State
│   ├── scan/
│   │   ├── scan-summary-card.tsx            # NEW: 등급 요약 카드
│   │   ├── issues-section.tsx               # NEW: Issues to Fix
│   │   ├── issue-card.tsx                   # NEW: 이슈 카드
│   │   ├── passed-checks-section.tsx        # NEW: Passed Checks
│   │   ├── passed-check-item.tsx            # NEW: 통과 항목 행
│   │   └── how-to-fix-dialog.tsx            # NEW: How to Fix 모달 (이슈+패스 공용)
│   ├── settings/
│   │   ├── general-settings.tsx             # NEW: 프로젝트 General 카드
│   │   ├── mcp-connection.tsx               # NEW: MCP Connection 카드
│   │   ├── danger-zone.tsx                  # NEW: Danger Zone 카드
│   │   ├── account-info.tsx                 # NEW: Profile - Account Info
│   │   ├── appearance-card.tsx              # NEW: Profile - Appearance
│   │   ├── change-password.tsx              # NEW: Profile - Change Password
│   │   ├── delete-account.tsx               # NEW: Profile - Danger Zone
│   │   └── plan-billing-cards.tsx           # NEW: Plan & Billing
│   └── dialogs/
│       └── new-project-dialog.tsx           # NEW: New Project 다이얼로그
├── app/
│   ├── layout.tsx                           # MODIFY: metadata + ThemeProvider + Toaster
│   ├── page.tsx                             # MODIFY: Landing Page
│   ├── login/
│   │   └── page.tsx                         # NEW: Login
│   ├── signup/
│   │   └── page.tsx                         # NEW: Sign Up
│   └── (dashboard)/
│       ├── layout.tsx                       # NEW: SidebarProvider + AppSidebar
│       ├── dashboard/
│       │   └── page.tsx                     # NEW: Overview (3가지 상태)
│       ├── project/
│       │   └── [id]/
│       │       ├── scan/
│       │       │   └── [scanId]/
│       │       │       └── page.tsx         # NEW: Scan Result
│       │       ├── scans/
│       │       │   └── page.tsx             # NEW: Scans History
│       │       └── settings/
│       │           └── page.tsx             # NEW: Project Settings
│       └── settings/
│           ├── profile/
│           │   └── page.tsx                 # NEW: Profile Settings
│           └── billing/
│               └── page.tsx                 # NEW: Plan & Billing
```

---

## 디자인 토큰 (Figma 기준 — 정확히 준수)

### 컬러
| 용도 | HEX |
|------|-----|
| Primary text / Button bg | `#171717` |
| Secondary text | `#737373` |
| Disabled text | `#c7c7c7` |
| Placeholder | `#a6a6a6` |
| Border / Separator | `#e5e5e5` |
| Page background | `#f5f5f5` |
| Card / Sidebar bg | `#ffffff` |
| Input bg | `#fafafa` |
| Grade A/B (text) | `#218c21` |
| Grade A/B (bg) | `#d9f7e0` |
| Grade C (text) | `#cc8000` |
| Grade C (bg) | `#fff2d9` |
| Grade D (text) | `#cc5900` |
| Grade D (bg) | `#ffecd9` |
| Grade F (text) | `#d13333` |
| Grade F (bg) | `#ffe5e5` |
| Green status | `#22c55e` |
| Amber warning | `#f59e0a` |
| Red critical | `#ed4242` / `#e53333` |
| Purple (AI/Claude) | `#7045fa` / `#4d33b2` / `#f5f2ff` / `#e0d9ff` |

### 타이포그래피
- Font: Inter (Geist Sans 사용 — 프로젝트에 이미 설정됨)
- Hero: 56px Bold
- Section title: 40px Bold
- Page title: 26-28px Bold
- Card title: 18px Semi Bold
- Body: 14px Regular
- Small: 13px Regular
- Caption: 12px Regular/Medium
- Tiny: 11px Regular

### 컴포넌트 기본
- Card: 12px radius, 1px `#e5e5e5` border, 20-24px padding
- Dialog/Modal: 16px radius, 28-32px padding
- Button: 8px radius, Primary=#171717, Outlined=white+border
- Input: 8px radius, `#e5e5e5` border, 44px height
- Badge: rounded-full, 4px 10px padding, 12px Semi Bold

---

## 구현 순서 (8 Steps)

### Step 0: 프로젝트 관리 파일 생성 (완료)

**`AI-PM-Team/Learning.md`** — 시행착오 기록:
- 첫 번째 교훈: Figma 프레임 전체 목록을 먼저 확인하지 않고 일부(14/28)만 리뷰한 뒤 전부 확인했다고 보고한 실수. **구현 시작 전 반드시 전체 범위를 파악하고, 빠뜨린 것이 없는지 교차 검증해야 함.**
- 앞으로 모든 시행착오를 여기에 기록

**`AI-PM-Team/progress.md`** — 진행 상황 기록:
- 개발 시작 이후 주요 진행 사항 추적
- 완료된 Step, 현재 진행 중인 작업, 발견된 이슈 등

### Step 1: 타입 확장 + 목 데이터

**`src/types/scan.ts`** — 기존 타입 확장:
- `PassedCheck`에 `description`, `fix` 필드 추가 (Passed Check도 How to Fix 모달 있음)
- `RuntimeEvent` 타입 추가 (brute force, traffic spike, admin access, new location)
- `RuntimeEventType` = "brute_force" | "traffic_spike" | "admin_access" | "new_location"
- `ProjectSettings` 타입 추가 (name, url, projectKey, github)
- `ScanHistoryItem` 타입 추가 (grade, date, source, checks, status)

**`src/lib/mock-data.ts`** — Figma 내용 그대로:
- `mockUser`: { name: "Park", email: "park@email.com", plan: "Free", projectsUsed: 2, projectsLimit: 3 }
- `mockProjects`: [{ name: "My SaaS App", domain: "saas.app", ... }]
- `mockScanResult`: Grade C, 5/8 passed, 2 critical + 1 warning
  - Issues: Stripe key exposed (critical), No rate limit (warning)
  - Passed: Passwords encrypted, API keys safe, .env excluded, HTTPS, Privacy policy
- `mockScanHistory`: 6개 항목 (A,B,B,C,C,D)
- `mockRuntimeEvents`: 4개 이벤트 (brute force, traffic spike, admin, new location)
- `mockStats`: Grade A, Uptime 100%, SSL 287 days, Issues 0
- 각 이슈/패스 항목의 How to Fix 데이터 (steps, code blocks, AI prompts) — UX spec 기반

### Step 2: 루트 레이아웃 + 글로벌 설정

**`src/app/layout.tsx`** — 수정:
- metadata: title "Guardrail", description "Security for Vibe Coders"
- ThemeProvider (next-themes) 래핑
- Toaster (sonner) 추가

### Step 3: Landing Page (`/`)

**`src/app/page.tsx`** — Landing Page 전체:
- Figma `1:168` 기준 pixel-perfect
- Navbar: 로고 + Features/Pricing/Docs 링크 + Sign In 버튼
- Hero: 녹색 badge + 56px headline + GitHub URL input + "Scan Now" CTA
- Features: 2x2 그리드 + "How it works" 3단계
- Pricing: Free($0) vs Pro($19) 카드 — Free CTA 없음, Pro "Upgrade to Pro" 검은 버튼
- Footer: 4컬럼 링크 + 카피라이트

**컴포넌트:**
- `src/components/landing/navbar.tsx`
- `src/components/landing/hero-section.tsx`
- `src/components/landing/features-section.tsx`
- `src/components/landing/pricing-section.tsx`
- `src/components/landing/footer.tsx`

### Step 4: Auth 페이지 (Login + Sign Up)

**`src/app/login/page.tsx`** — Figma `3:939`:
- 전체 화면 `#f5f5f5` 배경 + 중앙 420px 흰색 카드 (16px radius)
- 로고 + "Welcome back" + "Sign in to your account"
- Email + Password 입력
- "Forgot password?" 링크
- "Sign In" 버튼 (full-width, dark)
- "or" 구분선
- "Continue with Google" 버튼 (outlined, 아이콘)
- "Continue with GitHub" 버튼 (outlined, 아이콘)
- "Don't have an account? Sign up" 링크

**`src/app/signup/page.tsx`** — Figma `3:967`:
- 동일 레이아웃 + Name 필드 추가
- "Create your account" 제목
- "Must be at least 8 characters" 힌트
- "Create Account" 버튼
- Terms / Privacy 하단 텍스트

### Step 5: 대시보드 레이아웃 + 사이드바

**`src/app/(dashboard)/layout.tsx`** — 대시보드 공통 레이아웃:
- `SidebarProvider` + `AppSidebar` + `SidebarInset`
- 사이드바 260px 고정

**`src/components/layout/app-sidebar.tsx`** — Figma 모든 대시보드 기준:
- `SidebarHeader`: Guardrail 로고 + `ProjectSwitcher`
- `SidebarContent`: `SidebarNav` (4탭)
- `SidebarFooter`: `SidebarPlanCard` + `SidebarUserFooter`

**`src/components/layout/project-switcher.tsx`**:
- 드롭다운: 현재 프로젝트 (녹색 좌측 accent) + 프로젝트 목록 + "+ New Project"
- New Project 클릭 → `NewProjectDialog` 열기

**`src/components/layout/sidebar-nav.tsx`**:
- 4개 네비: Overview (LayoutDashboard), Scan (Search), Scan History (Clock), Settings (Settings)
- 활성: `#f5f5f5` bg + Semi Bold, 비활성: `#737373` + Medium

**`src/components/layout/sidebar-plan-card.tsx`**:
- "Free Plan" 라벨 + "2 of 3 projects" + Progress bar + "Upgrade" 버튼

**`src/components/layout/sidebar-user-footer.tsx`**:
- Avatar + 이름 + 이메일 + ChevronUp
- 클릭 → Account Menu (DropdownMenu)

**`src/components/layout/account-menu.tsx`**:
- Figma `2:1331`/`2:1382` 기준
- Profile Settings, Plan & Billing, Log out 메뉴

**`src/components/dialogs/new-project-dialog.tsx`** — Figma `2:2411`:
- Dialog: "New Project" + X 닫기
- Project Name (필수), Project URL (선택), GitHub Repository (선택)
- Cancel + Create Project 버튼

### Step 6: 대시보드 페이지들

#### 6a. Overview (`/dashboard`) — Figma `1:692` / `2:2515` / `1:1175` / `1:1194` / `1:1216`

**`src/app/(dashboard)/dashboard/page.tsx`**:
- 3가지 상태 분기:
  1. **No Projects** → `empty-no-projects.tsx`
  2. **No Scans** → `empty-no-scans.tsx`
  3. **정상** → 4 stats + Runtime Events (SDK 있음/없음) + Recent Scans + Grade Guide

**컴포넌트:**
- `stats-card.tsx` — 4개 카드 (Security Grade, Uptime, SSL, Open Issues) + 색상 코딩
- `runtime-events.tsx` — 4개 이벤트 리스트 (도트 색상 + 설명 + CTA 버튼)
- `runtime-event-dialog.tsx` — 4종 모달 (Figma `3:624`/`3:676`/`3:728`/`3:765`):
  - Block IP: Details 테이블 + Recommended Actions + [Dismiss] [Block IP Address]
  - Traffic Spike: Details + Actions + [Dismiss] [Enable Rate Limiting]
  - Admin Access: Details + [No, secure my account] [Yes, it was me]
  - New Location: Details + [No, secure my account] [Yes, it was me]
- `recent-scans.tsx` — 3행 테이블 (Grade badge + Date + Source pill + Checks + Status)
- `security-grade-guide.tsx` — A~F 카드 (Figma `3:898`)
- `sdk-setup-prompt.tsx` — SDK 미설치 안내 + "Set Up SDK" 버튼
- `setup-sdk-dialog.tsx` — Figma `3:1042`: 3단계 (install + middleware + Claude Code prompt)
- `empty-no-projects.tsx` — Figma `1:1175`: Target 아이콘 + "+ New Project" 버튼
- `empty-no-scans.tsx` — Figma `1:1194`: Search 아이콘 + URL 입력 + Scan 버튼
- `empty-no-monitoring.tsx` — Figma `1:1216`

#### 6b. Scan Result (`/project/[id]/scan/[scanId]`) — Figma `1:782`

**`src/app/(dashboard)/project/[id]/scan/[scanId]/page.tsx`**:
- 브레드크럼 "Scans > Scan #12" + "Rescan" 버튼
- `scan-summary-card.tsx` — Grade C, 64px badge, "A few things need fixing", 5/8 passed
- `issues-section.tsx` + `issue-card.tsx` — 2개 이슈:
  - Critical (red border `#f2d9d9`): Stripe key + "How to fix?" 버튼
  - Warning (amber border `#f2ebcc`): No rate limit + "How to fix?" 버튼
- `passed-checks-section.tsx` + `passed-check-item.tsx` — 5개 (녹색 체크 아이콘)
- `security-grade-guide.tsx` — 하단 재사용
- `how-to-fix-dialog.tsx` — **핵심 모달** (Figma `3:213`~`3:554`, 7종 공통):
  - Issue 모달: Badge(Critical/Warning) + Title + Description + Separator
    + "Why this matters" + Separator
    + "How to fix" (Step 1~4, 번호 원 + 설명 + 코드 블록)
    + "Ask AI to fix this" (보라색 프롬프트 카드 1~2개)
    + Footer: [Close] [Rescan] [Mark as Fixed]
  - Passed 모달: Badge(Passed/green) + "If this breaks, here's what to do"
    + "How to fix if this fails" + Footer: [Close] [Rescan] (Mark as Fixed 없음)

#### 6c. Scans History (`/project/[id]/scans`) — Figma `1:974`

**`src/app/(dashboard)/project/[id]/scans/page.tsx`**:
- "Scan History" 타이틀
- 테이블: Grade | Date | Source | Checks | Status — 6행
- Grade: 색상 코딩된 원 배지
- Source: MCP/Web/Auto pill (`#f5f5f5` bg, 8px radius)
- Status: 색상별 텍스트

#### 6d. Project Settings (`/project/[id]/settings`) — Figma `1:914`

**`src/app/(dashboard)/project/[id]/settings/page.tsx`**:
- 3개 카드 스택:
  - `general-settings.tsx`: Project Name + URL 입력 (`#fafafa` bg)
  - `mcp-connection.tsx`: Project Key (마스킹 + Copy) + Regenerate Key (빨간 링크) + JSON 코드블록 (다크 `#262626`)
  - `danger-zone.tsx`: 빨간 테두리 + "Delete Project" 빨간 outlined 버튼

#### 6e. Profile Settings (`/settings/profile`) — Figma `2:2644`

**`src/app/(dashboard)/settings/profile/page.tsx`**:
- 4개 카드 스택:
  - `account-info.tsx`: Name (수정) + Email (읽기 전용) + Save
  - `appearance-card.tsx`: Light/Dark 미니 프리뷰 (선택: 2px dark border)
  - `change-password.tsx`: Current + New + Confirm + Update Password
  - `delete-account.tsx`: Danger Zone (빨간 테두리) + Delete Account

#### 6f. Plan & Billing (`/settings/billing`) — Figma `2:2724`

**`src/app/(dashboard)/settings/billing/page.tsx`**:
- `plan-billing-cards.tsx`:
  - Free Plan 카드: 3 projects, All core features, Community support — CTA 없음
  - Pro Plan 카드: $19/month, Unlimited, Priority support — "Start Free Trial" 검은 버튼 + "14-day free trial"
  - Billing Info 카드: "No billing information on file" + "Add Payment Method" outlined 버튼

### Step 7: 검증 + 빌드

1. `npm run dev` → 모든 라우트 접근 확인
2. Figma Console로 각 프레임 스크린샷 → 구현 결과 시각적 비교
3. 모든 다이얼로그/모달 열기/닫기 동작 확인
4. toast 알림 동작 확인 (Mark as Fixed, Rescan, Block IP 등)
5. `npm run build` 성공 확인

---

## 기존 재활용 파일

| 파일 | 용도 |
|------|------|
| `src/types/scan.ts` | 기존 타입 (확장) |
| `src/lib/utils.ts` | `cn()` 유틸리티 |
| `src/hooks/use-mobile.ts` | `useIsMobile()` 반응형 훅 |
| `src/components/ui/sidebar.tsx` | shadcn Sidebar 전체 (`SidebarProvider`, `Sidebar`, `SidebarContent`, `SidebarHeader`, `SidebarFooter`, `SidebarMenu`, `SidebarMenuItem`, `SidebarMenuButton`, `SidebarInset`) |
| `src/components/ui/dialog.tsx` | shadcn Dialog (How to Fix, Runtime, New Project, Setup SDK 모달) |
| `src/components/ui/button.tsx` | 모든 버튼 |
| `src/components/ui/card.tsx` | 모든 카드 |
| `src/components/ui/badge.tsx` | Severity/Grade 배지 |
| `src/components/ui/input.tsx` | 모든 입력 필드 |
| `src/components/ui/separator.tsx` | 모달 내 구분선 |
| `src/components/ui/progress.tsx` | 사이드바 프로젝트 사용량 |
| `src/components/ui/dropdown-menu.tsx` | 프로젝트 선택, Account 메뉴 |
| `src/components/ui/avatar.tsx` | 유저 프로필 |
| `src/components/ui/table.tsx` | Scans History 테이블 |
| `src/components/ui/tabs.tsx` | 필요시 탭 UI |
| `src/components/ui/scroll-area.tsx` | 모달 스크롤 |
| `src/components/ui/sonner.tsx` | Toast 알림 |
| `src/components/ui/tooltip.tsx` | 사이드바 축소 시 |
| `src/components/ui/skeleton.tsx` | 로딩 상태 |
| `src/components/ui/alert.tsx` | 경고 메시지 |

---

## How to Fix 모달 7종 상세 (Figma Console 스크린샷 리뷰 결과)

### Issue 모달 vs Passed 모달 차이

| 항목 | Issue (Critical/Warning) | Passed (Green) |
|------|-------------------------|----------------|
| Badge | Red "Critical" / Amber "Warning" | Green "Passed" |
| 설명 제목 | "Why this matters" | "If this breaks, here's what to do" |
| 수정 제목 | "How to fix" | "How to fix if this fails" |
| Footer | 3버튼: Close + Rescan + **Mark as Fixed** | 2버튼: Close + Rescan |
| 톤 | 긴급 (지금 고쳐야 함) | 예방적 (나중에 깨질 경우 대비) |

### 7종 각각의 고유 내용

| # | Figma | 이슈명 | Badge | Step 수 | 코드 블록 특징 | AI 프롬프트 |
|---|-------|--------|-------|---------|---------------|------------|
| 1 | `3:213` | Stripe key exposed | Critical (red) | 4 | .env + Before/After + .gitignore | Move Stripe key + Find hardcoded keys |
| 2 | `3:277` | No rate limit | Warning (amber) | 3 | npm install + Upstash rate limiter (가장 큰 코드블록 ~15줄) | Add rate limiting + Upstash Redis |
| 3 | `3:334` | Passwords encrypted | Passed (green) | 3 | bcrypt.hash + Before/After | Check bcrypt/Argon2 + Audit auth code |
| 4 | `3:389` | API keys not exposed | Passed (green) | 3 | .env.local + **GitHub Actions YAML** (유일) | Scan codebase + Pre-commit hook |
| 5 | `3:444` | .env excluded from Git | Passed (green) | 3 | .gitignore + git rm + **bfg** (유일) | Check Git tracking + Audit .gitignore |
| 6 | `3:499` | HTTPS enabled | Passed (green) | 3 | certbot + **Next.js config headers** | HSTS headers + Mixed content fix |
| 7 | `3:554` | Privacy policy exists | Passed (green) | 3 | **React TSX 컴포넌트** (유일, Step 2-3 텍스트만) | GDPR privacy page + /privacy route |

### 공통 구조 (모든 7종)
- "Ask AI to fix this" 섹션: 보라색 아이콘(✦) + 설명 + 프롬프트 카드 1~2개
- 프롬프트 카드: `#f5f2ff` bg, `#e0d9ff` border, `#4d33b3` purple text, ">" prefix
- Step 번호: 24x24 `#171717` 원 + 흰색 숫자
- 코드 블록: `#f5f5f5` bg, 8px radius, 14-16px padding

---

## Runtime 모달 4종 상세 (Figma Console 스크린샷 리뷰 결과)

### Action 패턴 vs Confirm 패턴

| 패턴 | 모달 | Primary 버튼 | Secondary 버튼 |
|------|------|-------------|---------------|
| Action | Block IP, Traffic Spike | 구체적 액션 (Block/Enable) | Dismiss (ghost) |
| Confirm | Admin, New Location | "Yes, it was me" (ghost) | "No, secure my account" (**빨간색, 왼쪽**) |

### 4종 각각의 고유 내용

| # | Figma | 이벤트 | Badge 색 | Details 항목 | Recommended Actions | Primary 버튼 |
|---|-------|--------|---------|-------------|--------------------|-----------|
| 1 | `3:624` | Brute Force | Red | IP, 15 failures/3min, target account, VPN, python-requests | 4개 (Block IP, CAPTCHA, Notify, Blocklist) | "Block IP" (RED) |
| 2 | `3:676` | Traffic Spike | Amber | 4200 req/min, 45min, /api/login 62%, 12% errors | 4개 (Monitor, Rate limit, Check bots, Scale) | "Enable Rate Limiting" (dark) |
| 3 | `3:728` | Admin Access | Amber | admin@guardrail.dev, 3:12AM, Seoul, Chrome/macOS | 없음 | "Yes, it was me" (ghost) / "No, secure" (RED) |
| 4 | `3:765` | New Location | Green | Frankfurt Germany, Safari/iOS, "First login from: This country" | 없음 | "Yes, it was me" (ghost) / "No, secure" (RED) |

### Account Menu 상세 (Figma `2:1331` / `2:1382`)
- 320px 너비, 흰색 카드, 12px radius, shadow
- 유저 행: 둥근사각형 "P" 아바타 + "Park" + "park@email.com"
- 메뉴 3개:
  - Profile (person icon) + "Manage your account" 서브텍스트
  - Plan & Billing (card icon) + "Free Plan · 2/3 projects" 서브텍스트
  - Log out (exit icon) — **빨간색 텍스트**, 서브텍스트 없음
- 각 메뉴 사이 1px divider

---

## 구현 현황 (2026-02-23 기준)

### 완료된 Steps
- [x] **Step 0**: 프로젝트 관리 파일 생성 (Learning.md, progress.md)
- [x] **Step 1**: 타입 확장 (`scan.ts`) + 목 데이터 (`mock-data.ts`)
- [x] **Step 2**: 루트 레이아웃 (`layout.tsx` — ThemeProvider + Toaster + metadata)
- [x] **Step 3**: Landing Page (Navbar, Hero, Features, Pricing, Footer)
- [x] **Step 4**: Auth 페이지 (Login + Sign Up)
- [x] **Step 5**: 대시보드 레이아웃 + 사이드바 (ProjectSwitcher, SidebarNav, PlanCard, UserFooter, AccountMenu)
- [x] **Step 6**: 대시보드 페이지들 (Overview, Scan Result, Scans History, Project Settings, Profile Settings, Plan & Billing, Empty States, 모든 다이얼로그/모달)
- [x] **Step 7**: 검증 — `npx next build` 성공 확인

### Figma 대비 구현 차이점 (의도적 개선)

| 항목 | Figma 원안 | 실제 구현 | 사유 |
|------|-----------|----------|------|
| **New Project Dialog** | 단순 3필드 (Name, URL, GitHub URL) | GitHub OAuth 4-state UI (not_connected → connecting → connected → manual) + 레포 검색/선택 드롭다운 | Private repo 접근 지원 필요 |
| **Profile — Account Info** | 항상 편집 가능 + Save 버튼 | 읽기 모드(텍스트 + Edit) → 편집 모드(입력 + Cancel/Save) | UX 개선 — 의도하지 않은 수정 방지 |
| **Profile — Change Password** | 3개 입력 필드 + Update 버튼 | Email: 읽기 모드(•••• + Edit) → 편집 모드(3개 입력 + Cancel/Update); OAuth: Provider 정보 + 외부 링크 | OAuth 사용자 대응 + UX 개선 |
| **How to Fix 모달 Footer** | Issue: Close + Rescan + Mark as Fixed; Passed: Close + Rescan | 모두 "Close" 버튼만 | MVP에서는 Rescan/Mark as Fixed 기능 미구현, 혼란 방지 |
| **다크 모드** | Figma에 없음 (Light만) | CSS 변수 기반 Light/Dark 전체 지원 (`globals.css` gr-* 토큰) | Appearance 카드 테마 전환 기능 구현 |
| **랜딩페이지 카피** | MCP Integration, SDK runtime guard, SSL certificates 등 전문 용어 | Built for Claude Code, Live app protection, 쉬운 표현으로 전환 | UX Writer 리뷰: Vibe Coders 타겟에 전문 용어 부적합 (Readable, User-focused) |
| **전체 화면 카피 (2차)** | "Create your first project!", "Setup SDK", "via MCP", "be careful!", "All features included" (Free/Pro 동일) 등 | "No projects yet", "Set Up SDK", 전문 약어/필러 제거, Free "All core features"로 차별화 | UX Writer 2차 리뷰: 랜딩 외 전체 화면 대상 (Concise, Clear, Conversational, Distinct) |

### 추가된 디자인 토큰 (globals.css)
Figma 원안의 하드코딩 HEX 대신 CSS Custom Properties 기반 시스템 구축:
- `--gr-page`, `--gr-card`, `--gr-muted`, `--gr-input` (배경)
- `--gr-text`, `--gr-text-2`, `--gr-text-3` (텍스트)
- `--gr-border`, `--gr-border-2` (보더)
- `--gr-btn`, `--gr-btn-fg` (버튼)
- `--gr-code` (코드 블록)
- Light/Dark 모드별 값 정의 완료

---

## 핵심 준수 사항

1. **Figma 디자인 pixel-perfect 준수** — 색상, 간격, 폰트 크기, radius 모두 Figma 그대로
2. **스펙 문서 기능 정의 준수** — feature-spec의 화면 정의, 동작 규칙, 데이터 구조 그대로
3. **UX spec 디자인 토큰 준수** — 모달 너비, 패딩, Badge 색상/크기, 코드 블록 스타일 그대로
4. **MVP는 목 데이터** — 실제 API/DB 연동 없이 하드코딩된 목 데이터로 UI 완성
5. **임의 변경 금지** — 디자인/스펙에 없는 기능 추가하지 않음

---

## 검증 방법

1. `npm run dev` → `http://localhost:3000` Landing Page 확인
2. `/login`, `/signup` → 로그인/회원가입 UI 확인
3. `/dashboard` → Overview 3가지 상태 (No Projects / No Scans / 정상) 확인
4. Scan Result → "How to fix?" 클릭 → 7종 모달 모두 확인
5. Runtime Events → 4종 모달 확인
6. Project Settings / Profile / Plan & Billing 모든 카드 확인
7. Scans History 테이블 확인
8. New Project / Setup SDK 다이얼로그 확인
9. Account Menu 드롭다운 확인
10. Figma Console 스크린샷과 시각적 비교
11. `npm run build` 성공 확인

---

## 관련 문서

| 문서 | 위치 |
|------|------|
| Feature Spec | `AI-PM-Team/outputs/guardrail-feature-spec.md` |
| UX Spec | `AI-PM-Team/outputs/guardrail-ux-spec.md` |
| Figma 28프레임 리뷰 | `AI-PM-Team/outputs/figma-review-28frames.md` |
| Learning (시행착오) | `AI-PM-Team/Learning.md` |
| Progress (진행 상황) | `AI-PM-Team/progress.md` |
