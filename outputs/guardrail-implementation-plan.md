# GuardRail 전체 구현 계획 (Phase 1 & 2)

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

---
---

# GuardRail Phase 2 — 백엔드 + 실제 동작 구현 계획

## Context

Phase 1에서 **목 데이터 기반 UI 전체**를 완성했다. Phase 2는 앱을 실제로 동작하게 만드는 단계:
- Auth 백엔드 (회원가입/로그인/세션)
- DB 연동 (모든 mock data → 실제 데이터)
- 보안 스캔 엔진 (핵심 기능)
- MCP 서버 + SDK 모니터링 (외부 연동)
- 결제 시스템 (Pro 플랜)
- 배포 + 법적 문서

**알림 시스템(F11)은 Phase 3으로 이관.**

**스택 추가:** Supabase (DB + Auth) + Stripe (결제) + Resend (이메일, Phase 3) + Vercel (배포 + Cron)

---

## 구현 순서 (12 Steps)

### Step 0: 인프라 세팅

**Supabase 프로젝트 생성:**
- Database (PostgreSQL)
- Auth (이메일+비밀번호, Google OAuth, GitHub OAuth)
- Row Level Security (RLS) 정책

**DB 스키마 (6개 테이블):**

```sql
-- User (Supabase Auth 내장 + 확장)
-- Supabase auth.users 자동 생성 + public.profiles 테이블로 확장
profiles
├── id (uuid, FK → auth.users)
├── name
├── avatar_url
├── provider (email / google / github)
├── github_access_token (encrypted, nullable)
├── github_username (nullable)
├── github_avatar_url (nullable)
├── github_connected_at (datetime, nullable)
├── plan (free / pro, default: free)
├── stripe_customer_id (nullable)
├── created_at

-- Project
projects
├── id (uuid)
├── user_id (FK → profiles)
├── name
├── url (nullable)
├── github_repo (nullable — "owner/repo" 또는 full URL)
├── github_repo_id (number, nullable)
├── github_repo_private (boolean)
├── project_key_hash (hashed)
├── project_key_prefix (gr_sk_xxxx, 표시용)
├── monitoring_enabled (boolean, default true)
├── sdk_connected (boolean, default false)
├── created_at

-- Scan
scans
├── id (uuid)
├── project_id (FK → projects)
├── source (web / mcp / auto)
├── target_url
├── grade (A~F)
├── total_checks
├── passed_checks
├── failed_checks
├── created_at

-- ScanItem
scan_items
├── id (uuid)
├── scan_id (FK → scans)
├── category (payment / auth / secrets / infra / legal)
├── check_key (stripe_key_exposed 등)
├── status (pass / fail / warn)
├── title_user
├── description_user
├── fix_guide (마크다운)

-- MonitoringLog
monitoring_logs
├── id (uuid)
├── project_id (FK → projects)
├── type (uptime / ssl / header / runtime_event)
├── status (ok / warning / critical)
├── message
├── metadata (jsonb)
├── created_at

-- RuntimeEvent (SDK에서 수신)
runtime_events
├── id (uuid)
├── project_id (FK → projects)
├── event_type (auth.login_failed / auth.login_success / api.request)
├── ip_address
├── geo_country
├── user_agent
├── metadata (jsonb)
├── created_at
```

**환경변수 (.env.local):**
```
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_ANON_KEY=
SUPABASE_SERVICE_ROLE_KEY=
GITHUB_CLIENT_ID=
GITHUB_CLIENT_SECRET=
STRIPE_SECRET_KEY=
STRIPE_WEBHOOK_SECRET=
STRIPE_PRO_PRICE_ID=
ENCRYPTION_KEY=          # GitHub token AES-256 암호화용
```

**API Rate Limiting:**
- 스캔 API: 분당 5회
- 이벤트 수집 API: 분당 100회
- 일반 API: 분당 60회

**CORS 설정:**
- `guardrail-sdk` → 우리 API 호출 허용
- `guardrail-mcp` → 우리 API 호출 허용

---

### Step 1: F1 — 회원가입/로그인 (Auth)

**Supabase Auth 연동:**

| 방법 | 구현 |
|------|------|
| 이메일+비밀번호 | `supabase.auth.signUp()`, `supabase.auth.signInWithPassword()` |
| Google OAuth | `supabase.auth.signInWithOAuth({ provider: 'google' })` |
| GitHub OAuth | `supabase.auth.signInWithOAuth({ provider: 'github' })` |

**세션 관리:**
- Supabase `getSession()` + `onAuthStateChange()`
- 미들웨어에서 보호 라우트 체크 (`/dashboard/*`, `/project/*`, `/settings/*`)
- 미인증 시 `/login`으로 리다이렉트

**이메일 인증 (Email Verification):**
- 이메일+비밀번호 가입 시 Supabase가 인증 메일 자동 발송
- 인증 완료 전 대시보드 접근 차단
- "이메일을 확인하세요" 안내 화면 필요

**비밀번호 찾기 (Forgot Password):**
- Login 페이지 "Forgot password?" 링크 → 이메일 입력 화면
- `supabase.auth.resetPasswordForEmail()` → 재설정 이메일 발송
- 재설정 링크 클릭 → 새 비밀번호 입력 화면

**프로필 관리:**
- 이름 수정: `profiles` 테이블 업데이트
- 비밀번호 변경: `supabase.auth.updateUser({ password })` (이메일 사용자만)
- OAuth 사용자: Provider 정보 표시 + 외부 Settings 링크

**계정 삭제 (Cascade):**
1. Stripe 구독 취소 (Pro인 경우)
2. 모든 프로젝트 삭제 (→ 스캔 히스토리, MonitoringLog, RuntimeEvent cascade)
3. GitHub OAuth token 삭제
4. `profiles` 행 삭제
5. Supabase Auth에서 유저 삭제

**UI 연결:**
- Login/Signup 페이지 폼 → 실제 Supabase Auth 호출로 교체
- Profile Settings → 실제 profiles 테이블 조회/수정
- Change Password → 실제 비밀번호 변경 API
- Delete Account → cascade 삭제 실행

**파일:**
```
src/
├── lib/
│   └── supabase/
│       ├── client.ts              # 브라우저용 Supabase 클라이언트
│       ├── server.ts              # 서버용 Supabase 클라이언트
│       └── middleware.ts          # Auth 미들웨어 (보호 라우트)
├── app/
│   ├── middleware.ts              # Next.js 미들웨어 (세션 체크)
│   ├── auth/
│   │   ├── callback/
│   │   │   └── route.ts          # OAuth 콜백 처리
│   │   ├── forgot-password/
│   │   │   └── page.tsx          # 비밀번호 찾기 화면
│   │   └── reset-password/
│   │       └── page.tsx          # 비밀번호 재설정 화면
│   └── auth/confirm/
│       └── route.ts              # 이메일 인증 콜백
```

---

### Step 2: F2 + F3 — 프로젝트 CRUD + Project Key

**API 엔드포인트:**

| Method | 경로 | 기능 |
|--------|------|------|
| `GET` | `/api/projects` | 내 프로젝트 목록 |
| `POST` | `/api/projects` | 프로젝트 생성 (+ Key 생성) |
| `PATCH` | `/api/projects/[id]` | 프로젝트 수정 (이름, URL) |
| `DELETE` | `/api/projects/[id]` | 프로젝트 삭제 (cascade) |
| `POST` | `/api/projects/[id]/regenerate-key` | Project Key 재발급 |

**Project Key 생성:**
```
형식: gr_sk_ + 32자 랜덤 (a-z, 0-9)
저장: hash (bcrypt) + prefix (gr_sk_a1b2, 표시용)
발급 시 1회 전체 표시 → 이후 마스킹 (gr_sk_a1b2••••••••)
재발급 시 기존 키 즉시 무효화
```

**Free 플랜 제한:**
- 프로젝트 생성 시 현재 개수 확인
- Free: 최대 3개 초과 시 → "Upgrade to Pro for unlimited projects" 안내
- Pro: 무제한

**UI 연결:**
- Project Switcher → `GET /api/projects` 실제 조회
- New Project Dialog → `POST /api/projects` 실제 생성
- Project Settings → `PATCH /api/projects/[id]` 실제 수정
- Danger Zone → `DELETE /api/projects/[id]` 실제 삭제
- MCP Connection 카드 → 실제 project_key_prefix 표시 + Regenerate 동작

**파일:**
```
src/app/api/
├── projects/
│   ├── route.ts                   # GET (목록) + POST (생성)
│   └── [id]/
│       ├── route.ts               # PATCH (수정) + DELETE (삭제)
│       └── regenerate-key/
│           └── route.ts           # POST (키 재발급)
```

---

### Step 3: F2-1 — GitHub OAuth 연동 (Private Repo)

**API 엔드포인트:**

| Method | 경로 | 기능 |
|--------|------|------|
| `GET` | `/api/auth/github` | GitHub OAuth 페이지 리다이렉트 |
| `GET` | `/api/auth/github/callback` | code → access token 교환 + DB 저장 |
| `GET` | `/api/github/repos` | 연동 계정 repo 목록 (`?q=검색어`) |
| `GET` | `/api/github/status` | 연동 상태 확인 |
| `DELETE` | `/api/auth/github` | 연동 해제 |

**보안 (Feature Spec 4-1절 준수):**
- Access Token **AES-256 암호화** 후 DB 저장 (`ENCRYPTION_KEY` 환경변수)
- **CSRF 방지**: OAuth state 파라미터 생성 + 콜백 시 검증
- Token scope 최소화: `repo` + `read:user`만
- GitHub에서 revoke 시 401 → 재인증 유도 UI

**UI 연결:**
- New Project Dialog의 4-state UI (not_connected → connecting → connected → manual)
- "Connect GitHub" 버튼 → `/api/auth/github` 리다이렉트
- 연동 후 repo 검색 드롭다운 → `/api/github/repos?q=` 호출

**파일:**
```
src/
├── lib/
│   └── encryption.ts              # AES-256 encrypt/decrypt 유틸
├── app/api/
│   ├── auth/github/
│   │   ├── route.ts               # GET (OAuth 리다이렉트)
│   │   └── callback/
│   │       └── route.ts           # GET (콜백 처리)
│   └── github/
│       ├── repos/
│       │   └── route.ts           # GET (repo 목록)
│       └── status/
│           └── route.ts           # GET (연동 상태)
```

---

### Step 4: F4/F5 — 보안 스캔 엔진 (핵심)

**8개 보안 체크:**

| # | check_key | 체크 내용 | 구현 방법 |
|---|-----------|----------|----------|
| 1 | `stripe_key_exposed` | Stripe Secret Key 하드코딩 | GitHub API로 코드 fetch → `sk_live_`, `sk_test_` regex |
| 2 | `webhook_no_verify` | Webhook 서명 미검증 | Stripe webhook 핸들러에서 `constructEvent` 사용 여부 |
| 3 | `password_plaintext` | 비밀번호 평문 저장 | bcrypt/argon2 import 여부 확인 |
| 4 | `no_rate_limit` | 로그인 실패 횟수 미제한 | rate limiter 미들웨어 존재 여부 |
| 5 | `env_not_gitignored` | .env Git 미제외 | `.gitignore`에 `.env` 포함 여부 |
| 6 | `api_key_hardcoded` | API 키 하드코딩 | 하드코딩된 API key 패턴 검색 (높은 엔트로피 문자열) |
| 7 | `no_https` | HTTPS 미적용 | URL에 HTTP fetch → HTTPS 리다이렉트 확인 |
| 8 | `no_privacy_policy` | 개인정보처리방침 없음 | `/privacy` 라우트 또는 관련 파일 존재 확인 |

**스캔 흐름:**
```
1. GitHub URL → GitHub API로 repo 파일 트리 fetch
2. 관련 파일 내용 fetch (package.json, .gitignore, src/**/*.ts 등)
3. 8개 체크 순차 실행
4. 등급 계산 (아래 알고리즘)
5. Scan + ScanItem DB 저장
6. 결과 반환
```

**등급 계산 알고리즘:**
```
Critical 이슈 = status가 "fail"인 항목 중 category가 payment/secrets
Warning 이슈 = status가 "fail"인 항목 중 나머지

A = 8/8 pass (이슈 0개)
B = 7/8 pass + Critical 0개
C = 5~7 pass + Critical 1개 이하
D = 3~4 pass
F = 0~2 pass 또는 Critical 2개 이상
```

**타임아웃 처리:**
- 스캔 전체 2분 타임아웃
- 타임아웃 시 "스캔 시간이 초과됐어요. 다시 시도해주세요." 에러
- 정상 소요: 30초 이내

**스캔 로딩 UI:**
- "앱을 살펴보고 있어요..." 화면
- 8개 체크 항목이 순서대로 체크되는 애니메이션
- 완료 시 결과 페이지로 자동 이동

**스캔 소스:**
| 소스 | 트리거 | 방식 |
|------|--------|------|
| `web` | 대시보드에서 직접 실행 | GitHub URL → GitHub API → 서버에서 분석 |
| `mcp` | Claude Code에서 `scan_project` | 로컬 코드 분석 → 결과 API 전송 |
| `auto` | Cron 자동 재스캔 (Step 8) | 마지막 스캔 대상 GitHub URL 재스캔 |

**API 엔드포인트:**

| Method | 경로 | 기능 |
|--------|------|------|
| `POST` | `/api/scans` | 스캔 실행 (project_id + target_url) |
| `GET` | `/api/scans/[scanId]` | 스캔 결과 조회 (+ scan_items) |
| `GET` | `/api/projects/[id]/scans` | 프로젝트별 스캔 히스토리 |

**UI 연결:**
- Empty State "No Scans" → URL 입력 + Scan 버튼 → `POST /api/scans`
- Scan Result 페이지 → `GET /api/scans/[scanId]` 실제 데이터
- Scans History → `GET /api/projects/[id]/scans` 실제 데이터
- "Rescan" 버튼 → 동일 target_url로 `POST /api/scans` 재실행
- Dashboard Recent Scans → 최근 3개 실제 조회
- Dashboard Stats → 최신 스캔의 등급/이슈 수 표시

**파일:**
```
src/
├── lib/
│   ├── scan-engine/
│   │   ├── index.ts               # 스캔 오케스트레이터
│   │   ├── github-fetcher.ts      # GitHub API로 코드 fetch
│   │   ├── checks/
│   │   │   ├── stripe-key.ts      # 체크 1: Stripe 키 노출
│   │   │   ├── webhook-verify.ts  # 체크 2: Webhook 서명
│   │   │   ├── password-hash.ts   # 체크 3: 비밀번호 암호화
│   │   │   ├── rate-limit.ts      # 체크 4: Rate limit
│   │   │   ├── env-gitignore.ts   # 체크 5: .env 제외
│   │   │   ├── api-key.ts         # 체크 6: API 키 하드코딩
│   │   │   ├── https.ts           # 체크 7: HTTPS
│   │   │   └── privacy-policy.ts  # 체크 8: 개인정보처리방침
│   │   └── grade-calculator.ts    # 등급 계산 (A~F)
├── app/api/
│   └── scans/
│       ├── route.ts               # POST (스캔 실행)
│       └── [scanId]/
│           └── route.ts           # GET (결과 조회)
├── components/
│   └── scan/
│       └── scan-loading.tsx       # NEW: 스캔 로딩 애니메이션 화면
```

---

### Step 5: F7 — MCP 서버 (`guardrail-mcp` npm 패키지)

**별도 npm 패키지** — Claude Code에서 `npx guardrail-mcp@latest`로 실행.

**7개 Tool:**

| Tool | 설명 | 입력 | 출력 |
|------|------|------|------|
| `authenticate` | 브라우저 OAuth 팝업 → 계정 연결 | 없음 | 인증 상태 + 유저 정보 |
| `create_project` | 새 프로젝트 생성 | name, url (선택) | 프로젝트 정보 + project_key |
| `scan_project` | 로컬 디렉토리 보안 스캔 | project_key (선택) | 등급 + 항목별 결과 |
| `get_scan_result` | 마지막 스캔 결과 조회 | project_key (선택) | 최근 스캔 결과 |
| `get_fix_guide` | 이슈 수정 가이드 | issue_id | 수정 방법 + 코드 예시 |
| `list_projects` | 내 프로젝트 목록 | 없음 | 프로젝트 리스트 |
| `get_monitoring_status` | 모니터링 현황 | project_key | Uptime, SSL, 최근 이벤트 |

**MCP 스캔 흐름 (Web과 다른 점):**
- Web: GitHub API로 코드 fetch → 서버 분석
- MCP: **로컬 디렉토리 직접 분석** → 결과를 우리 API로 전송 + 저장

**패키지 구조:**
```
packages/guardrail-mcp/
├── package.json
├── src/
│   ├── index.ts                   # MCP 서버 엔트리
│   ├── tools/
│   │   ├── authenticate.ts
│   │   ├── create-project.ts
│   │   ├── scan-project.ts        # 로컬 파일 스캔 + 결과 API 전송
│   │   ├── get-scan-result.ts
│   │   ├── get-fix-guide.ts
│   │   ├── list-projects.ts
│   │   └── get-monitoring-status.ts
│   └── lib/
│       ├── api-client.ts          # 우리 서버 API 호출
│       └── local-scanner.ts       # 로컬 파일 시스템 스캔 로직
```

**서버 API (MCP용):**

| Method | 경로 | 기능 |
|--------|------|------|
| `POST` | `/api/mcp/auth` | MCP 인증 (OAuth token 교환) |
| `POST` | `/api/mcp/scans` | MCP 스캔 결과 수신 + 저장 |
| `GET` | `/api/mcp/projects` | 프로젝트 목록 (API key 인증) |

---

### Step 6: F9 — URL 기반 모니터링

**Cron Job 기반 — Vercel Cron 또는 별도 스케줄러:**

| 항목 | 주기 | 구현 | 결과 저장 |
|------|------|------|----------|
| M1: Uptime 체크 | 5분마다 | `fetch(projectUrl)` → HTTP 상태 코드 확인 | MonitoringLog (type: uptime) |
| M2: SSL 만료 | 1일 1회 | Node.js `tls.connect()` → 인증서 만료일 파싱 | MonitoringLog (type: ssl) |
| M3: 보안 헤더 | 1일 1회 | HTTP 응답 헤더 분석 (HSTS, CSP, X-Frame-Options 등) | MonitoringLog (type: header) |
| M4: 자동 재스캔 | 1일 1회 | 마지막 스캔 대상 GitHub URL 재스캔 + 이전 결과 비교 | Scan (source: auto) |

**API 엔드포인트 (Cron 호출용):**

| Method | 경로 | 기능 |
|--------|------|------|
| `GET` | `/api/cron/uptime` | 모든 활성 프로젝트 Uptime 체크 |
| `GET` | `/api/cron/ssl-check` | 모든 활성 프로젝트 SSL 체크 |
| `GET` | `/api/cron/security-headers` | 모든 활성 프로젝트 헤더 체크 |
| `GET` | `/api/cron/auto-rescan` | 모든 활성 프로젝트 자동 재스캔 |

**Vercel Cron 설정 (`vercel.json`):**
```json
{
  "crons": [
    { "path": "/api/cron/uptime", "schedule": "*/5 * * * *" },
    { "path": "/api/cron/ssl-check", "schedule": "0 3 * * *" },
    { "path": "/api/cron/security-headers", "schedule": "0 4 * * *" },
    { "path": "/api/cron/auto-rescan", "schedule": "0 5 * * *" }
  ]
}
```

**UI 연결:**
- Dashboard Overview → Stats 카드 (Uptime %, SSL 만료일, Open Issues) 실제 데이터
- Dashboard Overview → SDK 미설치 시 URL 모니터링 데이터만 표시

**파일:**
```
src/app/api/cron/
├── uptime/
│   └── route.ts
├── ssl-check/
│   └── route.ts
├── security-headers/
│   └── route.ts
└── auto-rescan/
    └── route.ts
```

---

### Step 7: F10 — SDK 런타임 모니터링 (`guardrail-sdk` npm 패키지)

**별도 npm 패키지** — 사용자 앱에 설치되는 미들웨어.

**설치 & 사용:**
```typescript
// Express
import { guardrail } from 'guardrail-sdk'
app.use(guardrail({ projectKey: process.env.GUARDRAIL_PROJECT_KEY }))

// Next.js middleware
export { guardrailMiddleware as middleware } from 'guardrail-sdk/next'
```

**SDK가 수집하는 이벤트:**

| 이벤트 | 수집 데이터 | 용도 |
|--------|-----------|------|
| `auth.login_failed` | IP, timestamp, user_agent | 브루트포스 감지 |
| `auth.login_success` | IP, geo, timestamp, user_id | 야간/이상 위치 감지 |
| `api.request` | endpoint, method, IP, timestamp | 트래픽 급증 감지 |

**SDK가 수집하지 않는 것 (프라이버시):**
- 요청/응답 바디, 비밀번호, 쿠키/세션, 개인 식별 정보

**이벤트 수집 API:**

| Method | 경로 | 기능 |
|--------|------|------|
| `POST` | `/api/events` | SDK 이벤트 수신 (batch, project_key 인증) |

**Runtime Event 액션 API:**

| Method | 경로 | 기능 |
|--------|------|------|
| `POST` | `/api/runtime/block-ip` | IP 차단 요청 (event_id, ip_address → SDK에 차단 명령 전달) |
| `POST` | `/api/runtime/acknowledge` | 이벤트 확인/거부 (event_id, action: "confirm" / "deny") |
| `GET` | `/api/runtime/events` | Runtime Events 목록 조회 (project_id, 페이지네이션) |

**액션 동작 방식:**
- **Block IP** (브루트포스 모달): `block-ip` API 호출 → runtime_events 상태를 `blocked`로 업데이트 + SDK 설정에 차단 IP 추가 (다음 SDK 폴링 시 반영)
- **Was This You? → Yes**: `acknowledge` API에 `action: "confirm"` → 이벤트 상태를 `acknowledged`로 업데이트
- **Was This You? → No**: `acknowledge` API에 `action: "deny"` → 이벤트 상태를 `suspicious`로 업데이트 + 해당 세션 강제 종료 권고
- **View Details** (트래픽 급증): 조회 전용. 별도 API 불필요 (기존 event 데이터 표시)

> MVP에서는 SDK 폴링 기반 명령 전달. 실시간 WebSocket 연동은 Phase 3 이후 고도화.

**서버 분석 로직 (이벤트 → RuntimeEvent 생성):**

| 감지 유형 | 조건 | RuntimeEvent type |
|----------|------|-------------------|
| 브루트포스 | 같은 IP, 10분 내 `login_failed` 10회+ | `brute_force` |
| 야간 관리자 | 새벽 1~5시 관리자 `login_success` | `admin_access` |
| 트래픽 급증 | 평소 대비 `api.request` 500%+ | `traffic_spike` |
| 이상 위치 | 기존에 없던 국가 IP `login_success` | `new_location` |

**UI 연결:**
- Dashboard Runtime Events → runtime_events 테이블 실제 조회
- Runtime 모달 4종 → 실제 이벤트 데이터 표시
- "Block IP" / "Enable Rate Limiting" 버튼 → 실제 액션 (SDK 명령 전송)
- SDK 연결 상태 → `projects.sdk_connected` 플래그

**SDK 설치 안내 타이밍 (구현 위치별):**

| 위치 | 조건 | 구현 파일 | 안내 방식 |
|------|------|----------|----------|
| 웹 스캔 결과 페이지 하단 | `sdk_connected = false` | `scan/[scanId]/page.tsx` | SDK setup prompt 카드 |
| MCP `guardrail_scan` 결과 끝 | `sdk_connected = false` | `tools/scan.ts` | 텍스트: "Want real-time protection? Run guardrail_setup_sdk" |
| Dashboard Overview | `sdk_connected = false` | `dashboard/page.tsx` | 기존 `sdk-setup-prompt.tsx` 카드 |
| Project Settings | 항상 | `settings/page.tsx` | 기존 `setup-sdk-dialog.tsx` 섹션 |

- 스캔 결과 확인 직후가 SDK 전환율 최고 타이밍 (보안 인식 피크)
- 비침투적 카드/텍스트 형태만 사용 (모달 팝업 금지)
- MCP에서는 `guardrail_setup_sdk` Tool 추가 필요 (SDK 설치 가이드 반환)

**패키지 구조:**
```
packages/guardrail-sdk/
├── package.json
├── src/
│   ├── index.ts                   # Express 미들웨어
│   ├── next.ts                    # Next.js 미들웨어
│   ├── collector.ts               # 이벤트 수집 + 배치 전송
│   └── types.ts                   # 이벤트 타입 정의
```

**서버 분석 파일:**
```
src/lib/
└── event-analyzer/
    ├── index.ts                   # 분석 오케스트레이터
    ├── brute-force.ts             # 브루트포스 감지
    ├── admin-access.ts            # 야간 관리자 감지
    ├── traffic-spike.ts           # 트래픽 급증 감지
    └── new-location.ts            # 이상 위치 감지
```

---

### Step 8: Stripe 결제 연동

**Stripe 설정:**
- Stripe Dashboard에서 Product 생성: "GuardRail Pro" ($19/월)
- 연간 플랜: $190/년 ($15.8/월, 2개월 무료)

**API 엔드포인트:**

| Method | 경로 | 기능 |
|--------|------|------|
| `POST` | `/api/stripe/checkout` | Stripe Checkout Session 생성 |
| `POST` | `/api/stripe/portal` | Stripe Customer Portal 리다이렉트 |
| `POST` | `/api/webhooks/stripe` | Webhook 수신 (구독 상태 동기화) |

**Checkout 흐름:**
```
"Start Free Trial" 클릭
  → POST /api/stripe/checkout (trial_period_days: 14)
  → Stripe Checkout 페이지 리다이렉트
  → 결제 완료 → Webhook 수신
  → profiles.plan = 'pro' + stripe_customer_id 저장
  → 프로젝트 개수 제한 해제
```

**Webhook 처리 이벤트:**
| 이벤트 | 액션 |
|--------|------|
| `checkout.session.completed` | plan → pro, stripe_customer_id 저장 |
| `customer.subscription.deleted` | plan → free |
| `customer.subscription.updated` | 플랜 상태 동기화 |
| `invoice.payment_failed` | 결제 실패 안내 (Phase 3에서 이메일) |

**Webhook 보안:** `STRIPE_WEBHOOK_SECRET`으로 서명 검증 필수

**UI 연결:**
- Landing Pricing "Upgrade to Pro" → Stripe Checkout
- Plan & Billing "Start Free Trial" → Stripe Checkout (14일 체험)
- Plan & Billing "Add Payment Method" → Stripe Customer Portal
- Sidebar Plan Card → 실제 plan 상태 표시 (Free/Pro)
- 프로젝트 생성 시 plan별 제한 적용

**파일:**
```
src/app/api/
├── stripe/
│   ├── checkout/
│   │   └── route.ts               # POST (Checkout Session 생성)
│   └── portal/
│       └── route.ts               # POST (Customer Portal)
└── webhooks/
    └── stripe/
        └── route.ts               # POST (Webhook 수신)
```

---

### Step 9: UI → 실제 데이터 연결 (Mock 교체)

Phase 1의 모든 목 데이터(`src/lib/mock-data.ts`)를 실제 API 호출로 교체:

| 화면 | mock → real |
|------|-------------|
| Dashboard Stats | `mockStats` → 최신 스캔 등급 + MonitoringLog 조회 |
| Runtime Events | `mockRuntimeEvents` → runtime_events 테이블 |
| Recent Scans | `mockScanHistory` (3행) → scans 테이블 최근 3개 |
| Scan Result | `mockScanResult` → scans + scan_items 조회 |
| Scans History | 하드코딩 6행 → scans 테이블 전체 |
| Project Settings | 하드코딩 → projects 테이블 조회/수정 |
| Profile Settings | "Park" 하드코딩 → profiles 테이블 |
| Plan & Billing | "Free" 하드코딩 → profiles.plan |
| Project Switcher | 하드코딩 → projects 테이블 (user_id 필터) |
| Sidebar Plan Card | "2 of 3" 하드코딩 → projects count 실제 조회 |
| Account Menu | 하드코딩 → 세션에서 유저 정보 |

**Empty State 분기 (실제 데이터 기반):**
- No Projects → `projects` count === 0
- No Scans → 선택된 프로젝트의 `scans` count === 0
- No Monitoring → `monitoring_enabled` === false 또는 SDK 미연결

---

### Step 10: Vercel 배포

| 항목 | 내용 |
|------|------|
| 프로젝트 연결 | GitHub repo → Vercel 프로젝트 연결 |
| 환경변수 | Vercel Dashboard에서 모든 `.env.local` 값 설정 |
| 도메인 | `guardrail.dev` (또는 `guardrail.io`) 커스텀 도메인 |
| Cron Jobs | `vercel.json`에 4개 cron 설정 (Step 6) |
| Build 설정 | Next.js 자동 감지, `npm run build` |
| Preview | PR마다 Preview 배포 자동 생성 |

**배포 전 체크리스트:**
1. `npm run build` 로컬 성공 확인
2. 모든 환경변수 Vercel에 설정
3. Supabase에서 Vercel URL을 OAuth redirect URL에 추가
4. Stripe Webhook URL을 프로덕션 URL로 업데이트
5. GitHub OAuth App callback URL을 프로덕션 URL로 업데이트

---

### Step 11: 법적 문서

| 페이지 | 경로 | 내용 |
|--------|------|------|
| 이용약관 | `/terms` | 서비스 이용 조건, 면책 사항 |
| 개인정보처리방침 | `/privacy` | 수집 정보, 사용 목적, 보관 기간, GDPR 대응 |
| 면책조항 | 이용약관에 포함 | 스캔 결과는 참고용, 법적 보안 감사 대체 불가 |

**수집하는 정보 명시:**
- 회원: 이메일, 이름 (선택), OAuth 프로필
- 스캔: GitHub URL, 코드 분석 결과 (코드 자체 저장 안 함)
- SDK: IP, user_agent, geo, 요청 메타데이터 (개인 정보 아님)

**파일:**
```
src/app/
├── terms/
│   └── page.tsx                   # 이용약관
└── privacy/
    └── page.tsx                   # 개인정보처리방침
```

---

## Phase 2 파일 구조 총정리

```
src/
├── lib/
│   ├── supabase/
│   │   ├── client.ts
│   │   ├── server.ts
│   │   └── middleware.ts
│   ├── encryption.ts
│   ├── scan-engine/
│   │   ├── index.ts
│   │   ├── github-fetcher.ts
│   │   ├── checks/  (8개 파일)
│   │   └── grade-calculator.ts
│   └── event-analyzer/
│       ├── index.ts
│       ├── brute-force.ts
│       ├── admin-access.ts
│       ├── traffic-spike.ts
│       └── new-location.ts
├── app/
│   ├── middleware.ts
│   ├── auth/
│   │   ├── callback/route.ts
│   │   ├── confirm/route.ts
│   │   ├── forgot-password/page.tsx
│   │   └── reset-password/page.tsx
│   ├── terms/page.tsx
│   ├── privacy/page.tsx
│   └── api/
│       ├── projects/
│       │   ├── route.ts
│       │   └── [id]/
│       │       ├── route.ts
│       │       ├── regenerate-key/route.ts
│       │       └── scans/route.ts
│       ├── scans/
│       │   ├── route.ts
│       │   └── [scanId]/route.ts
│       ├── events/route.ts
│       ├── auth/github/
│       │   ├── route.ts
│       │   └── callback/route.ts
│       ├── github/
│       │   ├── repos/route.ts
│       │   └── status/route.ts
│       ├── stripe/
│       │   ├── checkout/route.ts
│       │   └── portal/route.ts
│       ├── webhooks/stripe/route.ts
│       ├── mcp/
│       │   ├── auth/route.ts
│       │   ├── scans/route.ts
│       │   └── projects/route.ts
│       ├── runtime/
│       │   ├── events/route.ts          # GET (Runtime Events 목록)
│       │   ├── block-ip/route.ts        # POST (IP 차단)
│       │   └── acknowledge/route.ts     # POST (이벤트 확인/거부)
│       └── cron/
│           ├── uptime/route.ts
│           ├── ssl-check/route.ts
│           ├── security-headers/route.ts
│           └── auto-rescan/route.ts

packages/  (monorepo 또는 별도 repo)
├── guardrail-mcp/                 # MCP 서버 npm 패키지
└── guardrail-sdk/                 # SDK npm 패키지
```

---

## Phase 2 검증 방법

1. 회원가입 (이메일) → 이메일 인증 → 로그인 성공
2. Google/GitHub OAuth 로그인 성공
3. 비밀번호 찾기 → 재설정 이메일 → 새 비밀번호 설정
4. 프로젝트 생성 (3개) → 4번째 생성 시 Free 제한 안내
5. GitHub 연동 → Private repo 검색/선택
6. 스캔 실행 → 로딩 화면 → 결과 페이지 (실제 등급)
7. 스캔 히스토리 실제 데이터 확인
8. MCP 서버: Claude Code에서 `authenticate` → `scan_project` 성공
9. SDK 설치 → 이벤트 수신 → Runtime Events 표시
10. URL 모니터링: Uptime/SSL/헤더 체크 결과 Dashboard 반영
11. Stripe "Start Free Trial" → 결제 → Pro 전환 확인
12. 계정 삭제 → 모든 데이터 cascade 삭제 확인
13. `npm run build` 성공
14. Vercel 배포 → 프로덕션 전체 플로우 확인

---

## Phase 2 구현 현황 (2026-02-23 업데이트)

- [x] **Step 0**: 인프라 세팅 (Supabase + DB 스키마 6개 테이블 + RLS 정책 + 환경변수)
- [x] **Step 1**: Auth (이메일+비밀번호 회원가입/로그인, 이메일 인증, 비밀번호 찾기/재설정, Auth 미들웨어, 보호 라우트)
- [x] **Step 2**: 프로젝트 CRUD + Project Key (GET/POST/PATCH/DELETE API + 키 재발급 + Free 플랜 3개 제한)
- [x] **Step 9 (부분)**: Dashboard + 사이드바 + Settings → 실제 데이터 연결 (UserProvider, ProjectProvider, 프로필 수정, 비밀번호 변경, 계정 삭제, 프로젝트 생성/전환, 로그아웃)
- [ ] **Step 3**: GitHub OAuth 연동 (Private Repo)
- [ ] **Step 4**: 보안 스캔 엔진 (8개 체크 + 등급 계산 + 로딩 UI)
- [ ] **Step 5**: MCP 서버 (`guardrail-mcp` npm 패키지)
- [ ] **Step 6**: URL 기반 모니터링 (Cron — Uptime/SSL/헤더/재스캔)
- [ ] **Step 7**: SDK 런타임 모니터링 (`guardrail-sdk` npm 패키지)
- [ ] **Step 8**: Stripe 결제 연동 (Checkout/Portal/Webhook)
- [ ] **Step 9 (나머지)**: Scan Result, Scans History, Runtime Events, Stats 카드 → 실제 데이터 (스캔 엔진 완성 후)
- [ ] **Step 10**: Vercel 배포 (초기 배포 완료, 최종 배포 대기)
- [ ] **Step 11**: 법적 문서 (이용약관 + 개인정보처리방침)

**Phase 3 — 이메일 알림:**
- [ ] **Step 12**: 이메일 알림 (Resend 연동, DB 마이그레이션, 이메일 템플릿, Runtime + Monitoring 트리거, Settings 토글)

### Phase 2 구현 상세 — 완료된 항목

**Step 0 — Supabase 인프라:**
- Supabase 프로젝트 생성 + 유료 플랜 활성화
- `supabase/migrations/001_initial_schema.sql` — 6개 테이블 (profiles, projects, scans, scan_items, monitoring_logs, runtime_events)
- RLS 정책 — user_id 기반 행 수준 보안
- Auth Trigger — `auth.users` → `profiles` 자동 행 생성
- `.env.local` — Supabase URL, Anon Key, Service Role Key 설정
- Vercel 환경변수 동기화

**Step 1 — Auth:**
- `src/lib/supabase/client.ts` — 브라우저용 Supabase 클라이언트
- `src/lib/supabase/server.ts` — 서버용 Supabase 클라이언트 (쿠키 기반)
- `src/middleware.ts` — Auth 미들웨어 (세션 체크 + 보호 라우트)
- `src/app/auth/callback/route.ts` — OAuth 콜백 처리
- `src/app/auth/confirm/route.ts` — 이메일 인증 콜백
- `src/app/auth/reset-callback/route.ts` — 비밀번호 재설정 콜백
- `src/app/login/page.tsx` — 실제 Supabase Auth 로그인
- `src/app/signup/page.tsx` — 실제 Supabase Auth 회원가입
- `src/app/auth/forgot-password/page.tsx` — 비밀번호 재설정 이메일 발송
- `src/app/auth/reset-password/page.tsx` — 새 비밀번호 설정
- `src/app/auth/verify-email/page.tsx` — 이메일 인증 대기 화면
- Supabase Dashboard에서 Site URL (`https://guardrail-seven.vercel.app`) + Redirect URLs 설정

**Step 2 — Project CRUD + Key:**
- `src/app/api/projects/route.ts` — GET (목록) + POST (생성, Free 3개 제한)
- `src/app/api/projects/[id]/route.ts` — PATCH (수정) + DELETE (삭제, 소유권 검증)
- `src/app/api/projects/[id]/regenerate-key/route.ts` — POST (키 재발급)
- Project Key 형식: `gr_sk_` + 32자 랜덤, SHA-256 해시 저장, prefix 4자 표시용

**Step 9 (부분) — Dashboard 실제 데이터 연결:**
- `src/providers/user-provider.tsx` — React Context (Supabase Auth user + profiles 테이블)
- `src/providers/project-provider.tsx` — React Context (프로젝트 목록 + 현재 선택 프로젝트)
- `src/app/(dashboard)/layout.tsx` — UserProvider + ProjectProvider 래핑
- `src/components/layout/sidebar-user-footer.tsx` — 실제 유저 데이터 + 로그아웃
- `src/components/layout/sidebar-plan-card.tsx` — 실제 플랜/프로젝트 수
- `src/components/layout/project-switcher.tsx` — 실제 프로젝트 목록 + FolderOpen 아이콘
- `src/components/dialogs/new-project-dialog.tsx` — POST /api/projects 실제 생성
- `src/components/settings/account-info.tsx` — profiles 테이블 조회/수정
- `src/components/settings/change-password.tsx` — supabase.auth.updateUser()
- `src/components/settings/delete-account.tsx` — 2단계 확인 + signOut
- `src/app/(dashboard)/dashboard/page.tsx` — 실제 프로젝트 기반 상태 분기
- Vercel 배포 완료 (`https://guardrail-seven.vercel.app`)

---
---

# GuardRail Phase 3 — 이메일 알림 구현 계획

## Context

Phase 2에서 Runtime Event 감지(event-analyzer)와 URL 모니터링(Cron)이 완성되었다. 현재 위협/이상은 대시보드에서만 확인 가능. Phase 3에서는 **critical/warning 이벤트 발생 시 이메일 알림**을 추가하여 사용자가 대시보드를 보지 않아도 즉시 인지할 수 있게 한다.

**스택 추가:** Resend (이메일 API — API 키 `.env.local`에 설정 완료)

---

## 구현 순서 (Step 12)

### Step 12: F11 — 이메일 알림

#### 12-1: Resend 패키지 설치

```bash
npm install resend
```

#### 12-2: DB 마이그레이션

**`supabase/migrations/002_email_notifications.sql`:**

```sql
-- 프로젝트별 이메일 알림 on/off (기본 ON)
alter table public.projects
  add column email_alerts_enabled boolean default true;

-- 이메일 발송 기록 (쿨다운 체크용)
alter table public.monitoring_logs
  add column email_sent_at timestamptz;
```

#### 12-3: 이메일 유틸리티 생성

**`src/lib/email/send-alert.ts`:**

```
sendAlertEmail(projectId, logEntry) 함수:

1. Admin Supabase 클라이언트로 조회:
   - projects 테이블 → email_alerts_enabled, name
   - projects.user_id → profiles 테이블 → auth.users.email

2. email_alerts_enabled가 false면 → return (발송 안 함)

3. 쿨다운 체크:
   - monitoring_logs에서 동일 project_id + 동일 type + email_sent_at이
     1시간 이내인 행이 있으면 → return (중복 방지)

4. Resend API 호출:
   - from: "GuardRail <alerts@guardrail.dev>"
   - to: 사용자 이메일
   - subject: "[프로젝트명] 알림 제목"
   - html: buildAlertEmailHtml(데이터)

5. 발송 성공 시:
   - 해당 monitoring_logs 행의 email_sent_at = now() 업데이트
```

**`src/lib/email/templates.ts`:**

```
buildAlertEmailHtml(data) 함수:

입력:
- projectName: string
- alertType: string (brute_force, traffic_spike, admin_access, uptime, ssl, header)
- severity: "critical" | "warning"
- message: string
- details: Record<string, string> (IP, 시간, 위치 등)
- dashboardUrl: string

출력: 인라인 CSS HTML 이메일 문자열

디자인:
- 다크 헤더 (#18181b) + Guardrail 로고 (텍스트)
- 심각도 배지: Critical = 빨강 #ed4242, Warning = 주황 #f59e0a
- 프로젝트명 + 알림 제목
- 상세 메시지 (무슨 일이 일어났는지)
- Details 테이블 (key-value 쌍)
- "View in Dashboard" CTA 버튼 (다크 배경)
- 하단: "이 알림은 프로젝트 설정에서 끌 수 있습니다" 안내
```

#### 12-4: 기존 코드에 이메일 트리거 연동

**A) Runtime Event Analyzer — `src/lib/event-analyzer/index.ts`:**

위치: line ~158 (`await supabase.from("monitoring_logs").insert(logs)` 직후)

```typescript
// 이메일 알림 발송 (critical/warning만)
for (const log of logs) {
  if (log.status === "critical" || log.status === "warning") {
    await sendAlertEmail(log.project_id, log);
  }
}
```

대상 이벤트:
- brute_force (critical) → 이메일 O
- admin_access (warning) → 이메일 O
- traffic_spike (warning) → 이메일 O
- new_location (ok) → 이메일 X (status "ok"이므로 자동 제외)

**B) Monitoring Cron — `src/app/api/cron/monitor/route.ts`:**

위치: line ~100 (`await supabase.from("monitoring_logs").insert(logs)` 직후)

```typescript
// 이메일 알림 발송 (critical/warning만)
for (const log of logs) {
  if (log.status === "critical" || log.status === "warning") {
    await sendAlertEmail(project.id, log);
  }
}
```

대상:
- 사이트 다운 (critical) → 이메일 O
- SSL 무효 (critical) → 이메일 O
- SSL 만료 임박 (warning) → 이메일 O
- 보안 헤더 미흡 (warning/critical) → 이메일 O
- 정상 (ok) → 이메일 X

#### 12-5: Project Settings UI — Email Alerts 토글

**`src/components/settings/general-settings.tsx`:**

General Settings 카드 하단에 토글 추가:

```
Email Alerts          [ON/OFF 토글]
Get notified when we detect
security threats or monitoring issues.
```

동작:
- 토글 변경 → `PATCH /api/projects/[id]` (email_alerts_enabled 필드)
- 기존 name, url 저장과 동일 패턴

#### 12-6: 프로젝트 API 수정

**`src/app/api/projects/[id]/route.ts`:**

PATCH 핸들러에 `email_alerts_enabled` 필드 추가 (기존 name, url과 동일하게 처리)

---

## Phase 3 파일 구조

```
src/lib/email/
├── send-alert.ts              # NEW: 이메일 발송 유틸리티
└── templates.ts               # NEW: HTML 이메일 템플릿

supabase/migrations/
└── 002_email_notifications.sql # NEW: DB 컬럼 추가

수정 파일:
├── package.json                        # resend 패키지 추가
├── src/lib/event-analyzer/index.ts     # 이메일 트리거 추가
├── src/app/api/cron/monitor/route.ts   # 이메일 트리거 추가
├── src/components/settings/general-settings.tsx  # 토글 UI
└── src/app/api/projects/[id]/route.ts  # PATCH에 email_alerts_enabled
```

---

## Phase 3 검증 방법

1. Supabase에서 마이그레이션 적용 확인 (projects 테이블에 email_alerts_enabled 컬럼)
2. Project Settings에서 Email Alerts 토글 ON/OFF → DB 반영 확인
3. 테스트: Runtime Event 발생 시 (brute_force) → Resend 대시보드에서 이메일 발송 로그 확인
4. 테스트: Monitoring Cron 실행 시 (사이트 다운) → 이메일 발송 확인
5. 쿨다운 테스트: 1시간 내 동일 이벤트 → 이메일 미발송 확인
6. 토글 OFF 테스트: email_alerts_enabled = false → 이메일 미발송 확인
7. `npm run build` 성공 확인

---

### Phase 2 발견된 버그 및 수정

| 버그 | 원인 | 수정 |
|------|------|------|
| 프로필 이름 저장 실패 ("Failed to save profile") | DB 컬럼명 `name` vs 코드에서 `full_name` 사용 | `user-provider.tsx`, `account-info.tsx`, `sidebar-user-footer.tsx`에서 `full_name` → `name`으로 수정 |
