# GuardRail UX Spec (After Design)

Figma 디자인 iteration 과정에서 결정된 UX/UI 상세 스펙을 기록합니다.
Figma 파일: https://www.figma.com/design/7tCelpMbaoPpReJkwBhnBg/Guardrail

---

## 1. "How to Fix?" Modal Dialog

**결정일**: 2026-02-22
**Figma 노드**: Dashboard - How to Fix Modal (Scan Result 하단에 위치)

### 1.1 UX 방식 결정

- **선택**: Modal Dialog (모달 다이얼로그)
- **후보**: 인라인 확장, 사이드 패널, 모달, 별도 페이지
- **선택 이유**: 집중도가 높고, 기존 맥락을 유지하면서 상세 가이드를 보여줄 수 있음

### 1.2 모달 구조 (모든 체크 항목 공통)

```
┌─────────────────────────────────────────────┐
│  [Badge: Critical/Warning/Passed]       [✕] │
│  Issue Title (20px Bold)                    │
│  Short description (14px Regular, gray)     │
│─────────────────────────────────────────────│
│  Why this matters / If this breaks          │
│  (설명 텍스트, 13px Regular)                  │
│─────────────────────────────────────────────│
│  How to fix                                 │
│  ① Step label + code block                 │
│  ② Step label + code block                 │
│  ③ Step label (코드 없는 경우도 있음)          │
│─────────────────────────────────────────────│
│  ✦ Ask AI to fix this                      │
│  설명 텍스트                                  │
│  ┌─ Prompt Card 1 ─────────────────────┐   │
│  └─────────────────────────────────────┘   │
│  ┌─ Prompt Card 2 ─────────────────────┐   │
│  └─────────────────────────────────────┘   │
│─────────────────────────────────────────────│
│                                    [Close]  │
└─────────────────────────────────────────────┘
```

### 1.3 모달 디자인 토큰

| 속성 | 값 |
|------|-----|
| 모달 너비 | 620px (fixed) |
| 높이 | Auto (콘텐츠에 따라 변동) |
| Corner radius | 16px |
| Padding | 32px (all sides) |
| Section spacing | 24px |
| Background | #FFFFFF |
| Overlay | #000000 50% opacity |
| Shadow | 0 8px 32px rgba(0,0,0,0.12) |
| Font | Inter |

### 1.4 Badge 종류

| 유형 | 색상 | 텍스트 |
|------|------|--------|
| Critical | #EB3838 (red) | "Critical" |
| Warning | #D9A621 (amber) | "Warning" |
| Passed | #21B873 (green) | "Passed" |

Badge: rounded-full, padding 4px 10px, 12px Semi Bold, white text

### 1.5 Step Number Circle

- Size: 24x24px
- Background: #171717 (dark)
- Text: 12px Semi Bold, white
- Corner radius: 100 (full circle)

### 1.6 Code Block

- Background: #F5F5F5
- Corner radius: 8px
- Padding: 16px
- Font: 12px Regular, #404040
- Line height: 20px

---

## 2. "Ask AI to fix this" Section

**결정일**: 2026-02-22

### 2.1 개요

모든 "How to Fix?" 모달 하단(Footer 바로 위)에 AI 프롬프트 섹션을 추가.
사용자가 Claude Code 또는 다른 AI 어시스턴트에 복사-붙여넣기할 수 있는 프롬프트를 제공.

### 2.2 구조

- **AI Icon**: 20x20px, purple (#7040FA) background, "✦" 심볼
- **Section title**: "Ask AI to fix this" (15px Semi Bold)
- **설명**: "Copy a prompt and paste it into your AI coding tool to auto-fix this." (13px Regular, gray)
- **Prompt Cards**: 1-2개

### 2.3 Prompt Card 디자인

| 속성 | 값 |
|------|-----|
| Background | #F5F2FF (light purple) |
| Border | 1px #E0D9FF |
| Corner radius | 8px |
| Padding | 12px 14px |
| Text color | #4D33B3 (dark purple) |
| Font | 13px Medium |
| Prefix | "›" character (16px Semi Bold, purple) |

### 2.4 각 항목별 AI 프롬프트

**Issues:**

| 이슈 | 프롬프트 |
|------|---------|
| Stripe secret key exposed | "Move my Stripe secret key to .env.local and update all references" |
| | "Find all hardcoded API keys in my project and move them to environment variables" |
| No rate limit on login | "Add rate limiting to my login API endpoint — max 5 attempts per minute per IP" |
| | "Set up Upstash Redis rate limiter with progressive delays for failed logins" |

**Passed Checks:**

| 항목 | 프롬프트 |
|------|---------|
| Passwords encrypted | "Check that all passwords in my database are hashed with bcrypt or Argon2" |
| | "Audit my authentication code for any plaintext password storage" |
| API keys not exposed | "Scan my entire codebase for hardcoded API keys, tokens, and secrets" |
| | "Set up a pre-commit hook to prevent committing secrets" |
| .env excluded from Git | "Check if any .env files are tracked by Git and remove them from history" |
| | "Audit my .gitignore to make sure all secret files are excluded" |
| HTTPS enabled | "Enable HTTPS and add Strict-Transport-Security headers to my Next.js app" |
| | "Find and fix any mixed content issues (HTTP resources on HTTPS pages)" |
| Privacy policy exists | "Generate a GDPR-compliant privacy policy page for my Next.js app" |
| | "Create a /privacy route with all legally required sections" |

---

## 3. Passed Check 모달 (vs Issue 모달 차이점)

**결정일**: 2026-02-22

### 3.1 차이점

| 항목 | Issue 모달 | Passed Check 모달 |
|------|-----------|------------------|
| Badge | Critical (red) / Warning (amber) | Passed (green) |
| "Why" 섹션 제목 | "Why this matters" | "If this breaks, here's what to do" |
| "Fix" 섹션 제목 | "How to fix" | "How to fix if this fails" |
| Footer 버튼 | Close | Close |
| 톤 | 긴급 - 지금 고쳐야 함 | 예방적 - 나중에 깨질 경우 대비 |

### 3.2 설계 의도

Passed Check 항목도 "How to fix?" 모달을 제공하는 이유:
- 현재는 통과했지만 **나중에 깨질 수 있는 상황**에 대비
- 사용자가 각 체크 항목이 **왜 중요한지** 이해할 수 있도록 교육적 목적
- 문제 발생 시 **즉시 해결 방법을 찾을 수 있도록** 사전 제공

---

## 4. Figma 프레임 목록

디자인 과정에서 추가된 프레임들:

| 프레임 이름 | 위치 | 설명 |
|------------|------|------|
| Dashboard - How to Fix Modal | x:3144 y:1857 | Stripe key 이슈 모달 (+ Ask AI) |
| How to Fix - No Rate Limit | x:4601 y:1857 | Rate limit 이슈 모달 |
| How to Fix - Passwords are securely encrypt | x:6058 y:1857 | Passed: 비밀번호 암호화 |
| How to Fix - API keys are not exposed in co | x:7514 y:1857 | Passed: API 키 미노출 |
| How to Fix - .env file is excluded from Git | x:8971 y:1857 | Passed: .env 제외 |
| How to Fix - HTTPS is enabled | x:10427 y:1857 | Passed: HTTPS 활성화 |
| How to Fix - Privacy policy page exists | x:11884 y:1857 | Passed: 개인정보처리방침 |
| Runtime - Block IP (Brute Force) | x:1688 y:3582 | 런타임: IP 차단 모달 |
| Runtime - View Details (Traffic Spike) | x:3144 y:3582 | 런타임: 트래픽 급증 상세 |
| Runtime - Was This You? (Admin) | x:4601 y:3582 | 런타임: 관리자 접근 확인 |
| Runtime - Was This You? (New Location) | x:6058 y:3582 | 런타임: 새 위치 로그인 확인 |

- How to Fix 프레임들: Scan Result 프레임(1:782) 아래 행에 가로로 배치
- Runtime 프레임들: How to Fix 프레임들 아래 행에 가로로 배치 (Overview 1:692 기반)

---

## 5. Runtime Events 모달

**결정일**: 2026-02-22
**Figma 기준 노드**: Overview 페이지 (1:692) - Runtime Events 섹션의 CTA 버튼들

### 5.1 개요

Dashboard Overview 페이지의 Runtime Events 섹션에서 각 이벤트의 CTA 버튼을 클릭했을 때 표시되는 모달들.
Scan Result의 "How to Fix" 모달과는 다른 구조 — 코드 블록/단계별 가이드 대신 **이벤트 상세 정보 + 즉각적 액션**에 초점.

### 5.2 모달 구조 (Runtime 공통)

```
┌─────────────────────────────────────────────┐
│  [Event Badge]                         [✕] │
│  Event Title (20px Bold)                   │
│  Description (14px Regular, gray)          │
│─────────────────────────────────────────────│
│  📋 Details                                │
│  Label        Value                        │
│  Label        Value                        │
│  Label        Value                        │
│  Label        Value                        │
│─────────────────────────────────────────────│
│  ⚡ Recommended Actions (일부 모달만)       │
│  • Action item 1                           │
│  • Action item 2                           │
│  • Action item 3                           │
│─────────────────────────────────────────────│
│           [Secondary CTA]  [Primary CTA]   │
└─────────────────────────────────────────────┘
```

### 5.3 모달 디자인 토큰 (Runtime)

| 속성 | 값 |
|------|-----|
| 모달 너비 | 520px (How to Fix보다 좁음) |
| 높이 | Auto |
| Corner radius | 16px |
| Padding | 28px |
| Section spacing | 20px |
| Background | #FFFFFF |
| Shadow | 0 8px 32px rgba(0,0,0,0.12) |

### 5.4 Details 테이블 레이아웃

| 속성 | 값 |
|------|-----|
| Label 너비 | 140px (fixed) |
| Label 색상 | #737373 (gray) |
| Label 폰트 | 13px Regular |
| Value 색상 | #171717 (dark) |
| Value 폰트 | 13px Medium |
| Row spacing | 12px |

### 5.5 Event Badge 종류

| 이벤트 | Badge 텍스트 | Badge 색상 |
|--------|-------------|-----------|
| Brute Force | "Brute Force" | #EB3838 (red) |
| Traffic Spike | "Traffic Spike" | #D9A621 (amber) |
| Suspicious Login (Admin) | "Suspicious" | #D9A621 (amber) |
| New Location Login | "New Location" | #21B873 (green) |

Badge 스타일: How to Fix 모달과 동일 (rounded-full, 4px 10px padding, 12px Semi Bold, white text)

### 5.6 각 모달 상세

#### 5.6.1 Block IP (Brute Force Detected)

- **Badge**: Brute Force (red)
- **Title**: "Brute force attack detected"
- **Description**: "Multiple failed login attempts detected from a single IP address. This may indicate a brute force attack."
- **Details**:
  | Label | Value |
  |-------|-------|
  | Source IP | 192.168.1.42 |
  | Failed attempts | 847 in last hour |
  | Target | /api/auth/login |
  | First detected | Today, 2:34 AM |
- **Recommended Actions**:
  - Block this IP address immediately
  - Enable rate limiting on auth endpoints
  - Review access logs for other suspicious IPs
- **Footer**: [Dismiss] (outline) + [Block IP Address] (destructive/red)

#### 5.6.2 View Details (Traffic Spike)

- **Badge**: Traffic Spike (amber)
- **Title**: "Unusual traffic spike detected"
- **Description**: "Traffic volume has increased significantly beyond normal patterns. This could indicate a DDoS attempt or viral content."
- **Details**:
  | Label | Value |
  |-------|-------|
  | Current RPS | 2,847 req/s |
  | Normal RPS | ~150 req/s |
  | Duration | 23 minutes |
  | Top endpoint | /api/products |
- **Recommended Actions**:
  - Enable rate limiting if not already active
  - Scale up server resources temporarily
  - Monitor for malicious patterns in requests
- **Footer**: [Dismiss] (outline) + [Enable Rate Limiting] (primary)

#### 5.6.3 Was This You? (Admin Access at Unusual Hour)

- **Badge**: Suspicious (amber)
- **Title**: "Admin access at unusual hour"
- **Description**: "Someone accessed the admin dashboard outside of your normal working hours."
- **Details**:
  | Label | Value |
  |-------|-------|
  | User | admin@guardrail.dev |
  | Time | Today, 3:42 AM |
  | IP Address | 10.0.0.15 |
  | Action | Viewed user database |
- **Recommended Actions**: 없음 (Was this you? 패턴이므로)
- **Footer**: [No, secure my account] (destructive/red) + [Yes, it was me] (outline)

#### 5.6.4 Was This You? (Login from New Location)

- **Badge**: New Location (green)
- **Title**: "Login from a new location"
- **Description**: "A login was detected from a location that hasn't been used before."
- **Details**:
  | Label | Value |
  |-------|-------|
  | Location | Seoul, South Korea |
  | IP Address | 203.0.113.42 |
  | Device | Chrome on macOS |
  | Time | Today, 9:15 AM |
- **Recommended Actions**: 없음
- **Footer**: [No, secure my account] (destructive/red) + [Yes, it was me] (outline)

### 5.7 Footer 버튼 패턴

Runtime 모달은 두 가지 Footer 패턴이 있음:

| 패턴 | 사용 모달 | Primary 버튼 | Secondary 버튼 |
|------|----------|-------------|---------------|
| **Action 패턴** | Block IP, Traffic Spike | 구체적 액션 (Block/Enable) | Dismiss (outline) |
| **Confirm 패턴** | Was This You? x2 | "Yes, it was me" (outline) | "No, secure my account" (destructive) |

### 5.8 How to Fix 모달 vs Runtime 모달 비교

| 항목 | How to Fix 모달 | Runtime 모달 |
|------|----------------|-------------|
| 모달 너비 | 620px | 520px |
| Padding | 32px | 28px |
| 주요 콘텐츠 | 단계별 코드 가이드 | 이벤트 상세 정보 테이블 |
| 코드 블록 | 있음 | 없음 |
| Step 번호 | 있음 (1-4단계) | 없음 |
| Ask AI 섹션 | 있음 | 없음 |
| Recommended Actions | 없음 | 있음 (일부) |
| Footer 버튼 수 | 3개 (Close/Rescan/Mark) | 2개 |
| 목적 | 코드 수정 가이드 | 즉각적 대응/확인 |

---

---

## 6. 사이드바 네비게이션 (4탭 구조)

**결정일**: 2026-02-22

### 6.1 변경 내용

기존 3탭(Overview, Scan History, Settings)에서 **4탭**으로 변경.

| # | 탭 | 아이콘 | 설명 |
|---|-----|-------|------|
| 1 | Overview | LayoutDashboard | 프로젝트 전체 현황판 |
| 2 | Scan | Search | 보안 스캔 실행 + 결과 |
| 3 | Scan History | Clock | 스캔 히스토리 전체 목록 |
| 4 | Settings | Settings | 프로젝트 설정 |

### 6.2 적용 범위

Landing Page를 제외한 **모든 대시보드 프레임**에 동일하게 적용:
- Dashboard - Overview (1:692)
- Dashboard - Scan Result (1:782)
- Dashboard - Project Settings (1:914)
- Scans History (1:974)
- Dashboard - Empty (No Projects / No Scans / No Monitoring)
- Dashboard - Overview (No SDK) (2:2515)
- Profile Settings (2:2644)
- Plan & Billing (2:2724)

---

## 7. Pricing 디자인 변경 (Starter → Pro)

**결정일**: 2026-02-22
**Figma 노드**: Plan & Billing (2:2724), Landing Page Pricing 섹션

### 7.1 변경 사항

| 항목 | Before | After |
|------|--------|-------|
| 유료 플랜 이름 | Starter | **Pro** |
| Free 박스 배경 | 흰색 | 흰색 (동일) |
| Pro 박스 배경 | 다크 (#171717) | **흰색** (Free와 동일 디자인) |
| Free 박스 CTA | "Get Started" 버튼 | **CTA 없음** (현재 사용 중이므로) |
| Pro 박스 CTA | 흰색 버튼 | **검은색 (#171717) 버튼**, "Upgrade to Pro" |

### 7.2 디자인 원칙

- 두 박스 **동일한 디자인** (흰색 배경, 테두리, 검은 텍스트)
- 차별화는 CTA 유무로만 표현
- 무료 사용자에게 "현재 사용 중" 상태를 명시적으로 보여줌

---

## 8. Login / Sign Up 화면

**결정일**: 2026-02-22
**Figma 노드**: Login (3:939), Sign Up (3:967)

### 8.1 공통 디자인 토큰

| 속성 | 값 |
|------|-----|
| 배경 | #F5F5F5 (light gray) |
| 카드 배경 | #FFFFFF |
| 카드 Corner radius | 12px |
| 카드 Shadow | 0 2px 8px rgba(0,0,0,0.06) |
| 카드 너비 | 400px |
| 카드 Padding | 32px |
| Input 높이 | 44px |
| Input Corner radius | 8px |
| Input Border | #E5E5E5 |
| Input Focus Border | #171717 |
| Primary Button | #171717 bg, white text, 8px radius, full width |
| Google Button | white bg, #E5E5E5 border, 8px radius, full width |

### 8.2 Login 화면

- 로고 + "Welcome back" + "Sign in to your account"
- Email + Password 입력
- "Forgot password?" 링크 (우측 정렬)
- "Sign In" 버튼 (primary)
- "or" 구분선
- "Continue with Google" 버튼
- "Continue with GitHub" 버튼
- "Don't have an account? Sign up" 링크

### 8.3 Sign Up 화면

- 로고 + "Create your account"
- Name + Email + Password 입력
- "Must be at least 8 characters" 힌트 텍스트
- "Create Account" 버튼 (primary)
- "or" 구분선
- "Continue with Google" 버튼
- "Continue with GitHub" 버튼
- "Already have an account? Sign in" 링크
- "By creating an account, you agree to our Terms and Privacy Policy" 하단 텍스트

---

## 9. Profile Settings — Appearance (다크/라이트 모드)

**결정일**: 2026-02-22
**Figma 노드**: Profile Settings (2:2644) 내 Appearance 카드 (3:830)

### 9.1 구조

Profile Settings 페이지 하단에 "Appearance" 카드 추가.

### 9.2 디자인

- 카드 제목: "Appearance" (16px Semi Bold)
- 두 개의 선택 옵션: **Light** / **Dark**
- 각 옵션은 **미니 레이아웃 프리뷰**로 시각화:
  - Light: 밝은 사이드바 + 밝은 콘텐츠 영역
  - Dark: 어두운 사이드바 + 어두운 콘텐츠 영역
- 선택된 옵션: **2px 검은 테두리** (bold border)
- 비선택 옵션: **1px #E5E5E5 테두리** (thin border)
- 옵션 라벨: "Light" / "Dark" (13px Medium, 중앙 정렬)

### 9.3 미니 프리뷰 카드

| 속성 | 값 |
|------|-----|
| 프리뷰 카드 크기 | 160 x 100px |
| Corner radius | 8px |
| Light 사이드바 색상 | #F5F5F5 |
| Light 콘텐츠 색상 | #FFFFFF |
| Dark 사이드바 색상 | #262626 |
| Dark 콘텐츠 색상 | #171717 |

### 9.4 Profile 페이지 전체 구조

Profile Settings 페이지에는 4개 섹션이 있음 (위→아래 순서):

1. **Account Info** — 읽기 모드(Name/Email 텍스트 표시 + Edit 버튼) → 편집 모드(Name 입력 + Email 읽기 전용 + Cancel/Save 버튼)
2. **Appearance** — Light/Dark 테마 토글 (미니 프리뷰)
3. **Change Password** — 로그인 방식에 따라 분기:
   - **Email/Password**: 읽기 모드(•••••••••••• + Edit 버튼) → 편집 모드(Current/New/Confirm 입력 + Cancel/Update Password)
   - **GitHub OAuth**: "You signed in with GitHub." + "Manage in GitHub Settings" 외부 링크
   - **Google OAuth**: "You signed in with Google." + "Manage in Google Settings" 외부 링크
4. **Danger Zone** (빨간 테두리) — Delete Account + 설명 + "Delete Account" 버튼 (빨간색)

### 9.5 Plan & Billing 페이지 상세

Plan & Billing 페이지에는 3개 섹션이 있음:

1. **Current Plan** (Free Plan 뱃지) — 3 projects, All core features, Community support — CTA 없음
2. **Pro Plan** ($19/month) — For growing teams, Unlimited projects, All features, Priority support — "Start Free Trial" CTA + "14-day free trial"
3. **Billing Info** — "No billing information on file." + "Add Payment Method" 버튼

---

## 10. Overview — Security Grade Guide

**결정일**: 2026-02-22
**Figma 노드**: Overview (1:692) 내 Security Grade Guide 카드 (3:898)

### 10.1 구조

Overview 페이지 Stats 카드 아래에 "Security Grade Guide" 카드 배치.

### 10.2 등급별 정보

| 등급 | 라벨 | Badge 색상 | 설명 |
|------|------|-----------|------|
| A | Excellent | #21B873 (green) | Your app passed all security checks. Safe to deploy. |
| B | Good | #21B873 (green) | Minor improvements recommended but generally secure. |
| C | Fair | #D9A621 (amber) | Several issues found. Review before deploying. |
| D | Poor | #F97316 (orange) | Significant security risks detected. Fix before going live. |
| F | Critical | #EB3838 (red) | Critical vulnerabilities found. Immediate action required. |

### 10.3 디자인

- 카드 타이틀: "Security Grade Guide"
- 각 등급은 한 행: [Badge] [Label] [Description]
- Badge: 24x24px 원, 등급 문자 (12px Bold, white)
- Label: 14px Semi Bold
- Description: 13px Regular, #737373
- 행 사이 Separator (#E5E5E5)

---

## 11. Setup SDK 다이얼로그

**결정일**: 2026-02-22
**Figma 노드**: Setup SDK (Dialog) (3:1006)
**트리거**: Overview (No SDK) 페이지 → Runtime Events 섹션 → "Set Up SDK" 버튼 클릭

### 11.1 구조

```
┌─────────────────────────────────────────────┐
│  Set Up SDK                            [✕] │
│  Install the Guardrail SDK to detect        │
│  password attacks, suspicious logins...     │
│─────────────────────────────────────────────│
│  ① Install the SDK                          │
│  ┌─────────────────────────────────────┐    │
│  │ npm install @guardrail/sdk          │    │
│  └─────────────────────────────────────┘    │
│                                             │
│  ② Add the middleware to your app           │
│  ┌─────────────────────────────────────┐    │
│  │ import { guardrail } from           │    │
│  │   '@guardrail/sdk'                  │    │
│  │ app.use(guardrail({                 │    │
│  │   projectKey: process.env           │    │
│  │     .GUARDRAIL_PROJECT_KEY          │    │
│  │ }))                                 │    │
│  └─────────────────────────────────────┘    │
│                                             │
│  ③ Or ask Claude Code                       │
│  If you're using Claude Code, say:          │
│  ┌─ › Install Guardrail SDK ───────────┐   │
│  │   in my project                      │   │
│  └──────────────────────────────────────┘   │
│─────────────────────────────────────────────│
│           [Close]    [Copy Project Key]     │
└─────────────────────────────────────────────┘
```

### 11.2 디자인 토큰

| 속성 | 값 |
|------|-----|
| 모달 너비 | 520px |
| Corner radius | 16px |
| Padding | 28px |
| Section spacing | 20px |
| Step 번호 | 24x24 dark circle, white text |
| 코드 블록 | #F5F5F5 bg, 8px radius, 14px padding |
| AI Prompt 카드 | #F5F2FF bg, #E0D9FF border, purple text |
| Footer | Close (outline) + Copy Project Key (primary dark) |

---

## 13. New Project Dialog — GitHub OAuth 연동

**결정일**: 2026-02-23
**구현 파일**: `src/components/dialogs/new-project-dialog.tsx`
**상세 스펙**: `guardrail-feature-spec.md` (루트)

### 13.1 변경 내용

기존 단순 URL 입력 → GitHub OAuth 연동을 통한 private repo 지원.

### 13.2 UI States (4가지)

| State | UI | 전환 |
|-------|-----|------|
| **Not Connected** (기본) | "Connect GitHub" 버튼 + "Or enter URL manually" 링크 | 버튼 클릭 → Connecting |
| **Connecting** | 스피너 + "Connecting to GitHub..." | OAuth 완료 → Connected |
| **Connected** | "@parkdev" 표시 + Repo 검색 드롭다운 + Disconnect 링크 | Repo 선택 시 표시 |
| **Manual** (폴백) | 기존 URL 입력 필드 + "Connect GitHub for private repos" 링크 | Public repo 수동 입력용 |

### 13.3 Repo 검색 드롭다운

- 검색 아이콘 + "Search repositories..." placeholder
- 결과 목록: 🔒 (private) / 🌐 (public) 아이콘 + repo full_name + language
- 선택 시: 선택된 repo 표시 + X 버튼으로 해제 가능

---

## 14. Profile Settings — Edit/Cancel 패턴

**결정일**: 2026-02-23
**구현 파일**: `src/components/settings/account-info.tsx`, `src/components/settings/change-password.tsx`

### 14.1 Account Info

| 모드 | UI |
|------|-----|
| **읽기 (기본)** | Name/Email 텍스트 표시 + 우측 상단 Edit 버튼 |
| **편집** | Name 입력 + Email 읽기 전용 + Cancel/Save 버튼 (Edit 버튼 숨김) |

### 14.2 Change Password — Auth Provider별 분기

| Provider | UI |
|----------|-----|
| **Email** (기본) | 읽기: •••••• + Edit → 편집: 3개 입력 + Cancel/Update Password |
| **GitHub** | GitHub 아이콘 + "You signed in with GitHub..." + "Manage in GitHub Settings" 외부 링크 |
| **Google** | Google 아이콘 + "You signed in with Google..." + "Manage in Google Settings" 외부 링크 |

### 14.3 Component Props

```tsx
// ChangePassword
interface ChangePasswordProps {
  authProvider?: "email" | "github" | "google";  // default: "email"
}
```

---

## 15. Figma 프레임 목록 (전체)

| 프레임 이름 | Figma Node | 설명 |
|------------|-----------|------|
| Landing Page | 1:168 | 랜딩 페이지 (Hero + Features + Pricing + Footer) |
| Dashboard - Overview | 1:692 | 프로젝트 현황판 + Security Grade Guide |
| Dashboard - Scan Result | 1:782 | 스캔 결과 상세 |
| Dashboard - Project Settings | 1:914 | 프로젝트 설정 |
| Scans History | 1:974 | 스캔 히스토리 목록 |
| Dashboard - Empty - No Projects | 1:1175 | Empty State: 프로젝트 없음 |
| Dashboard - Empty - No Scans | 1:1194 | Empty State: 스캔 없음 |
| Dashboard - Empty - No Monitoring | 1:1216 | Empty State: 모니터링 없음 |
| Account Menu (Popup) | 2:1331 | 계정 메뉴 팝업 |
| Account Menu (Component) | 2:1382 | 계정 메뉴 컴포넌트 |
| New Project (Dialog) | 2:2411 | 새 프로젝트 생성 다이얼로그 |
| Dashboard - Overview (No SDK) | 2:2515 | Overview (SDK 미연결 상태) |
| Profile Settings | 2:2644 | 프로필 설정 + Appearance + Change Password + Delete Account |
| Plan & Billing | 2:2724 | 플랜 & 결제 (Free/Pro) + Billing Info |
| Login | 3:939 | 로그인 화면 (Email + Google + GitHub) |
| Sign Up | 3:967 | 회원가입 화면 (Email + Google + GitHub) |
| Setup SDK (Dialog) | 3:1006 | SDK 설치 가이드 다이얼로그 |
| How to Fix 모달들 | (섹션 4 참조) | 이슈별 해결 가이드 모달 7종 |
| Runtime 모달들 | (섹션 4 참조) | 런타임 이벤트 모달 4종 |

---

## 16. 모니터링 URL 수집 플로우

**결정일**: 2026-02-23

### 16.1 개요

코드 스캔(GitHub URL)과 별도로, 배포된 앱의 **Uptime / SSL 인증서 / 보안 헤더**를 실시간 모니터링하려면 사용자의 **배포 URL**(site_url)이 필요하다. URL은 강제하지 않고, 자연스러운 시점에 유도한다.

### 16.2 데이터 구조

`projects` 테이블에 필드 추가:

| 필드 | 타입 | 설명 |
|------|------|------|
| `site_url` | text (nullable) | 배포된 앱 URL (e.g. https://myapp.vercel.app) |

- `site_url`이 `null` → 대시보드 Stats 카드에 `"--"` 표시
- `site_url`이 있음 → Vercel Cron이 자동으로 모니터링 시작

### 16.3 URL 수집 3단계 전략

| 단계 | 트리거 | 대시보드 UI | Claude Code |
|:----:|--------|-----------|-------------|
| **1차** | SDK 첫 연결 감지 | 토스트/배너로 URL 입력 유도 | 메시지로 URL 입력 제안 |
| **2차** | 배포 명령어 감지 | — | `vercel deploy` 등 감지 시 자동 제안 |
| **보조** | 사용자 직접 | `"--"` 카드 클릭 / Settings | `guardrail monitor <url>` 명령어 |

핵심 원칙:
- **강제하지 않는다** — URL 없어도 코드 스캔은 100% 사용 가능
- **동기가 높을 때 묻는다** — SDK 연결 직후 = "앱이 돌아가고 있다"는 신호
- **나중에도 쉽게 추가** — 여러 진입점 제공

### 16.4 대시보드 — 1차 수집 (SDK 연결 직후)

SDK가 GuardRail에 첫 ping을 보낸 시점에 Overview 페이지 상단에 배너 표시:

```
┌─────────────────────────────────────────────────────────────┐
│  📡 SDK 연결됨!  배포된 URL을 입력하면 Uptime/SSL을          │
│  24시간 감시합니다.                                          │
│                                                             │
│  🔗 [https://                              ]  [모니터링 시작] │
│                                                             │
│  [ 로컬에서만 개발 중 — 나중에 설정 ]                          │
└─────────────────────────────────────────────────────────────┘
```

디자인 토큰:

| 속성 | 값 |
|------|-----|
| 배경 | #F0FDF4 (light green) |
| 테두리 | 1px #BBF7D0 |
| Corner radius | 12px |
| Padding | 16px 20px |
| Input 높이 | 40px |
| "모니터링 시작" 버튼 | Primary (#171717) |
| "나중에" 링크 | text link, #737373 |

### 16.5 대시보드 — 보조 수집 ("--" 카드 클릭)

Stats 카드에서 Uptime `"--"` 또는 SSL `"--"` 클릭 시 인라인 확장:

```
┌──────────────────────────────────┐
│  Uptime                          │
│  Is your app running?            │
│                                  │
│  --                              │
│  ─ Not monitored                 │
│──────────────────────────────────│
│  모니터링을 시작하려면             │
│  배포된 앱 URL을 입력하세요        │
│                                  │
│  🔗 [https://            ]       │
│  [모니터링 시작]                   │
└──────────────────────────────────┘
```

### 16.6 대시보드 — Settings 페이지

Project Settings 페이지에 "Monitoring URL" 필드 추가:

```
┌─────────────────────────────────────────────┐
│  Monitoring                                 │
│                                             │
│  Site URL                                   │
│  [https://myapp.vercel.app          ] [Save]│
│                                             │
│  이 URL로 Uptime, SSL 인증서,               │
│  보안 헤더를 자동 체크합니다.                  │
└─────────────────────────────────────────────┘
```

### 16.7 Claude Code — URL 수집 흐름

**1차: SDK 연결 직후**

```
Claude Code:
  SDK가 연결되었습니다.
  배포된 URL이 있으면 외부 모니터링도 시작할 수 있습니다.

  1. URL 입력
  2. 아직 로컬 개발 중
  3. 나중에
```

**2차: 배포 명령어 감지 시**

```
[사용자가 "vercel deploy" 실행]

Claude Code:
  배포가 완료된 것 같습니다.
  GuardRail 모니터링에 URL을 등록할까요?

  → https://myapp.vercel.app

  [ 등록 ]  [ 나중에 ]
```

**수동: 명령어**

```
사용자: guardrail monitor https://myapp.vercel.app
Claude Code: 모니터링이 시작되었습니다.
```

---

## 17. 모니터링 체크 항목 (Uptime / SSL / 보안 헤더)

**결정일**: 2026-02-23

### 17.1 Vercel Cron 구조

```
/api/cron/monitor  ← Vercel Cron이 주기적 호출
    ├── 1) Uptime 체크: fetch(site_url) → status code + latency
    ├── 2) SSL 체크: TLS handshake → 인증서 만료일 파싱
    └── 3) 보안 헤더 체크: response headers 분석
```

Cron 주기: **Pro 플랜 5분 간격** / Free 플랜은 모니터링 미제공

### 17.2 Uptime 체크

| 항목 | 설명 |
|------|------|
| 방법 | `fetch(site_url)` → HTTP status code |
| 저장 값 | status_code, response_time_ms, is_up (boolean) |
| 판정 | 200-399 → UP, 400+ 또는 timeout → DOWN |
| 대시보드 표시 | 최근 24시간 uptime % (e.g. "99.7%") |

Stats 카드 상태:

| 조건 | 값 | 색상 | Status 텍스트 |
|------|-----|------|-------------|
| site_url 없음 | `"--"` | gray | "Not monitored" |
| 100% | `"100%"` | green | "All systems operational" |
| 95-99.9% | `"99.7%"` | green | "Operational" |
| 90-94.9% | `"92.3%"` | amber | "Degraded" |
| < 90% | `"85.1%"` | red | "Experiencing downtime" |
| 현재 다운 | `"DOWN"` | red | "Site is down" |

### 17.3 SSL 인증서 체크

| 항목 | 설명 |
|------|------|
| 방법 | TLS 연결 → 인증서 `valid_to` 파싱 |
| 저장 값 | expires_at, days_remaining, issuer |
| 대시보드 표시 | 남은 일수 (e.g. "42 days") |

Stats 카드 상태:

| 조건 | 값 | 색상 | Status 텍스트 |
|------|-----|------|-------------|
| site_url 없음 | `"--"` | gray | "Not monitored" |
| 30일+ 남음 | `"42 days"` | green | "Valid" |
| 14-30일 남음 | `"21 days"` | amber | "Expiring soon" |
| 7일 미만 | `"5 days"` | red | "Expires this week" |
| 만료됨 | `"Expired"` | red | "Certificate issue" |
| 인증서 없음 (HTTP) | `"None"` | red | "No SSL certificate" |

### 17.4 보안 헤더 체크

| 항목 | 설명 |
|------|------|
| 방법 | `fetch(site_url)` → response headers 분석 |
| 체크 대상 | 6가지 보안 헤더 |

체크 대상 헤더:

| # | 헤더 | 설명 |
|---|------|------|
| 1 | `Strict-Transport-Security` | HTTPS 강제 |
| 2 | `X-Frame-Options` | 프레임 삽입 차단 |
| 3 | `Content-Security-Policy` | 코드 실행 제한 |
| 4 | `X-Content-Type-Options` | MIME 타입 고정 |
| 5 | `Referrer-Policy` | 출처 정보 제한 |
| 6 | `Permissions-Policy` | 권한 차단 (카메라/마이크 등) |

저장 값: 각 헤더별 present (boolean) + value (string)

보안 헤더 결과는 **Open Issues 카운트에 반영** — 누락된 헤더 1개 = 이슈 1개

### 17.5 모니터링 결과 DB 테이블

`monitoring_results` 테이블:

| 필드 | 타입 | 설명 |
|------|------|------|
| id | uuid | PK |
| project_id | uuid | FK → projects |
| checked_at | timestamptz | 체크 시각 |
| uptime_status | boolean | UP/DOWN |
| uptime_latency_ms | integer | 응답 시간 |
| ssl_expires_at | timestamptz | SSL 만료일 |
| ssl_days_remaining | integer | SSL 잔여일 |
| headers_present | jsonb | { hsts: true, xfo: false, ... } |

### 17.6 알림 트리거

| 이벤트 | 알림 방법 | 조건 |
|--------|---------|------|
| 사이트 다운 | 이메일 즉시 | 2회 연속 실패 (오탐 방지) |
| SSL 30일 남음 | 이메일 1회 | days_remaining ≤ 30 |
| SSL 7일 남음 | 이메일 매일 | days_remaining ≤ 7 |
| SSL 만료 | 이메일 즉시 | days_remaining ≤ 0 |
| 보안 헤더 누락 발견 | 대시보드 표시 | 스캔 결과에 포함 (별도 알림 없음) |

---

## 18. Dashboard Stats 카드 — 실제 데이터 연동

**결정일**: 2026-02-23
**구현 파일**: `src/components/dashboard/stats-card.tsx`

### 18.1 `"--"` → 실제 데이터 전환 조건

| 카드 | `"--"` 조건 | 실제 데이터 조건 |
|------|------------|----------------|
| Security Grade | 스캔 기록 없음 | 최소 1회 스캔 완료 |
| Uptime | `site_url` 없음 또는 모니터링 데이터 없음 | `site_url` 있음 + 모니터링 데이터 1건+ |
| SSL Certificate | `site_url` 없음 또는 모니터링 데이터 없음 | `site_url` 있음 + SSL 체크 데이터 1건+ |
| Open Issues | 스캔 기록 없음 | 최소 1회 스캔 완료 |

### 18.2 Stats 데이터 소스

```
DashboardStats {
  grade        ← scans 테이블 (최신 스캔)
  gradeLabel   ← grade에서 계산
  uptime       ← monitoring_results (최근 24시간 평균)
  uptimeStatus ← uptime 값에서 계산
  sslDays      ← monitoring_results (최신 레코드)
  sslStatus    ← sslDays 값에서 계산
  openIssues   ← issues 테이블 + 보안 헤더 누락 수
  issuesStatus ← openIssues 값에서 계산
}
```

### 18.3 Stats 카드 색상 로직 (구현됨)

**Uptime 카드:**
- `"--"` → gray dot + "Not monitored"
- `uptimeStatus`에 "down" 포함 → red dot + red text
- 그 외 → green dot + green text

**SSL 카드:**
- `"--"` → gray dot + "Not monitored"
- `sslStatus`에 "issue" 포함 → red dot + red text
- `sslStatus`에 "soon" 포함 → amber dot + amber text
- 그 외 → green dot + green text

### 18.4 `"--"` 카드 클릭 인터랙션

`"--"` 상태의 Uptime/SSL 카드는 **클릭 가능** (cursor: pointer):
- 클릭 시 → 16.5에 정의된 URL 입력 인라인 확장 표시
- 또는 모니터링 설정 페이지로 이동

---

## 19. Overview (No SDK) — 2섹션 Setup 구조 (Quick Monitor + Full Protection)

**결정일**: 2026-02-23
**위치**: Overview (No SDK) 페이지 (2:2515) — 기존 Runtime Events "Set Up SDK" 버튼 영역 대체

### 19.1 설계 의도

SDK 설치와 URL 모니터링은 **독립적인 기능**이므로 각각 별도 섹션으로 분리:

| 기능 | URL만 필요 | SDK 필요 |
|------|:---------:|:-------:|
| Uptime 모니터링 | O | X |
| SSL 인증서 체크 | O | X |
| 보안 헤더 6종 체크 | O | X |
| 런타임 이벤트 (브루트포스, 의심 로그인 등) | X | O |

사용자가 **SDK 설치 없이도 즉시 가치를 느낄 수 있도록** URL 입력만으로 모니터링을 시작할 수 있게 하고,
더 깊은 보호가 필요한 사용자는 SDK를 설치하도록 유도.

### 19.2 전체 레이아웃

```
┌─────────────────────────────────────────────────────────────────────┐
│  Overview (No SDK) 페이지                                           │
│  ┌───────────────────────────────────────────────────────────────┐  │
│  │  Stats Cards: Grade(--) | Uptime(--) | SSL(--) | Issues(--)  │  │
│  └───────────────────────────────────────────────────────────────┘  │
│                                                                     │
│  ┌──────────────────────── Section A ────────────────────────────┐  │
│  │  🔗 Quick Monitor                                             │  │
│  │  Monitor uptime, SSL, and security headers —                  │  │
│  │  no code changes needed.                                      │  │
│  │                                                               │  │
│  │  [https://your-app.vercel.app          ]  [Start Monitoring]  │  │
│  │                                                               │  │
│  │  ✓ Uptime tracking (24/7)                                     │  │
│  │  ✓ SSL certificate expiry alerts                              │  │
│  │  ✓ Security headers audit (6 checks)                          │  │
│  └───────────────────────────────────────────────────────────────┘  │
│                                                                     │
│  ┌──────────────────────── Section B ────────────────────────────┐  │
│  │  🛡️ Full Protection with SDK                                  │  │
│  │  Detect attacks in real-time — brute force, suspicious        │  │
│  │  logins, and unauthorized access attempts.                    │  │
│  │                                                               │  │
│  │  ✓ Brute force attack detection                               │  │
│  │  ✓ Suspicious login alerts                                    │  │
│  │  ✓ Unusual traffic spike monitoring                           │  │
│  │  ✓ New location login tracking                                │  │
│  │                                                               │  │
│  │                                        [Set Up SDK →]         │  │
│  └───────────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────┘
```

### 19.3 Section A — Quick Monitor

사용자가 배포 URL만 입력하면 **즉시** Uptime/SSL/보안 헤더 모니터링을 시작.

#### 카피 (Value Proposition)

| 요소 | 텍스트 |
|------|--------|
| 아이콘 | 🔗 (또는 Globe 아이콘) |
| 타이틀 | "Quick Monitor" (16px Semi Bold) |
| 서브타이틀 | "Monitor uptime, SSL, and security headers — no code changes needed." (14px Regular, #737373) |
| 체크리스트 | ✓ Uptime tracking (24/7) / ✓ SSL certificate expiry alerts / ✓ Security headers audit (6 checks) |

#### 입력 필드

| 속성 | 값 |
|------|-----|
| Input placeholder | "https://your-app.vercel.app" |
| Input 높이 | 44px |
| Input Corner radius | 8px |
| Input Border | #E5E5E5 (기본) / #171717 (focus) |
| 버튼 | "Start Monitoring" — Primary (#171717 bg, white text) |
| 레이아웃 | Input + 버튼 가로 배치 (input flex-1, 버튼 auto) |

#### 검증

| 케이스 | 동작 |
|--------|------|
| 유효한 HTTPS URL 입력 | `site_url` 저장 → 모니터링 시작 → Stats 카드 업데이트 |
| HTTP URL 입력 | 자동으로 https://로 변환 후 저장 |
| 빈 값 또는 유효하지 않은 형식 | Input 하단에 "Please enter a valid URL (e.g. https://myapp.com)" 에러 메시지 (12px, #EB3838) |

#### 성공 상태

URL 입력 완료 시 Section A가 성공 상태로 전환:

```
┌───────────────────────────────────────────────────────────────┐
│  ✅ Quick Monitor                                             │
│  Monitoring active for https://myapp.vercel.app               │
│                                                               │
│  ✓ Uptime tracking (24/7)                                     │
│  ✓ SSL certificate expiry alerts                              │
│  ✓ Security headers audit (6 checks)                          │
│                                                               │
│  [Change URL]                                                 │
└───────────────────────────────────────────────────────────────┘
```

### 19.4 Section B — Full Protection with SDK

SDK를 설치하면 **런타임 이벤트 탐지**까지 가능.

#### 카피 (Value Proposition)

| 요소 | 텍스트 |
|------|--------|
| 아이콘 | 🛡️ (또는 Shield 아이콘) |
| 타이틀 | "Full Protection with SDK" (16px Semi Bold) |
| 서브타이틀 | "Detect attacks in real-time — brute force, suspicious logins, and unauthorized access attempts." (14px Regular, #737373) |
| 체크리스트 | ✓ Brute force attack detection / ✓ Suspicious login alerts / ✓ Unusual traffic spike monitoring / ✓ New location login tracking |

#### CTA 버튼

| 속성 | 값 |
|------|-----|
| 텍스트 | "Set Up SDK" (→ 화살표 포함) |
| 스타일 | Outline 버튼 (#171717 border, #171717 text) |
| 동작 | 클릭 시 기존 Setup SDK 다이얼로그(섹션 11) 열기 |
| 위치 | 섹션 우측 하단 정렬 |

SDK 설치 완료 후 Section B도 성공 상태로 전환:

```
┌───────────────────────────────────────────────────────────────┐
│  ✅ Full Protection with SDK                                  │
│  SDK connected — runtime protection active.                   │
│                                                               │
│  ✓ Brute force attack detection                               │
│  ✓ Suspicious login alerts                                    │
│  ✓ Unusual traffic spike monitoring                           │
│  ✓ New location login tracking                                │
└───────────────────────────────────────────────────────────────┘
```

### 19.5 디자인 토큰

| 속성 | 값 |
|------|-----|
| 섹션 카드 Background | #FFFFFF |
| 섹션 카드 Border | 1px #E5E5E5 |
| 섹션 카드 Corner radius | 12px |
| 섹션 카드 Padding | 24px |
| 섹션 간 간격 | 16px |
| 체크 아이콘 색상 | #21B873 (green) |
| 체크리스트 텍스트 | 13px Regular, #525252 |
| 체크리스트 행 간격 | 8px |
| 성공 상태 타이틀 아이콘 | ✅ (green checkmark) |

### 19.6 섹션 16과의 관계

| 시나리오 | Quick Monitor | 섹션 16 배너 |
|----------|:------------:|:----------:|
| Quick Monitor에서 URL 입력 완료 | ✅ 활성 | 표시 안 함 (이미 수집) |
| Quick Monitor 건너뛰고 SDK만 설치 | 미입력 상태 유지 | SDK 연결 후 배너 표시 (16.4) |
| 둘 다 건너뜀 | 미입력 | SDK 연결 후 배너 표시 |

### 19.7 반응 순서 (사용자 여정)

1. 사용자가 Overview (No SDK) 진입
2. **Quick Monitor** 섹션이 먼저 눈에 띔 (코드 변경 불필요 → 진입장벽 낮음)
3. URL 입력 → Stats 카드가 `"--"`에서 실제 데이터로 전환 → **즉각적 보상**
4. 아래쪽 **Full Protection** 섹션에서 "더 많은 보호가 가능하구나" 인지
5. SDK 설치 → 런타임 이벤트까지 탐지 시작

---

## Changelog

| 날짜 | 변경 내용 |
|------|----------|
| 2026-02-22 | "How to Fix?" 모달 UX 방식 결정 (모달 다이얼로그) |
| 2026-02-22 | Stripe key / Rate limit 이슈 모달 디자인 추가 |
| 2026-02-22 | "Ask AI to fix this" 섹션 추가 (모든 모달 공통) |
| 2026-02-22 | Passed Check 5개 항목 각각 How to Fix 모달 디자인 추가 |
| 2026-02-22 | Runtime Events 모달 4종 디자인 추가 (Block IP, Traffic Spike, Was This You? x2) |
| 2026-02-22 | Runtime Events 모달 스펙 문서화 (섹션 5) |
| 2026-02-22 | 사이드바 4탭 구조로 변경 (Overview, Scan, Scan History, Settings) |
| 2026-02-22 | Pricing: Starter → Pro 리네이밍 + 디자인 통일 (흰색 배경) |
| 2026-02-22 | Free 플랜 CTA 제거 |
| 2026-02-22 | Login / Sign Up 화면 추가 (이메일+비밀번호 + Google + GitHub OAuth) |
| 2026-02-22 | Profile Settings > Appearance (다크/라이트 모드 토글) 추가 |
| 2026-02-22 | Overview > Security Grade Guide (A-F 등급 설명) 추가 |
| 2026-02-22 | Setup SDK 다이얼로그 디자인 추가 (3단계 가이드 + Ask AI) |
| 2026-02-22 | Login/Sign Up에 GitHub OAuth 버튼 추가 |
| 2026-02-22 | Landing Page Pricing: Starter → Pro 수정, Free CTA 제거 |
| 2026-02-22 | Empty States 3종 사이드바에 Scan 탭 추가 (4탭 통일) |
| 2026-02-22 | Profile Settings: Change Password + Danger Zone (Delete Account) 스펙 반영 |
| 2026-02-22 | Plan & Billing: Start Free Trial CTA + Billing Info 섹션 + Priority support 스펙 반영 |
| 2026-02-22 | Scan Result: 하단 Security Grade 가이드 스펙 반영 |
| 2026-02-22 | 파일명 변경: guardrail-additional-spec → guardrail-ux-spec |
| 2026-02-23 | New Project Dialog: GitHub OAuth 연동 UI 추가 (4가지 상태: not_connected/connecting/connected/manual + repo 검색 드롭다운) |
| 2026-02-23 | Profile > Account Info: 읽기/편집 모드 분리 (Edit → Save/Cancel 토글) |
| 2026-02-23 | Profile > Change Password: OAuth 사용자 분기 처리 (GitHub/Google → 외부 링크 안내, Email → Edit/Save/Cancel) |
| 2026-02-23 | How to Fix 모달: Footer 버튼 간소화 — Rescan/Mark as Fixed 제거, Close만 유지 |
| 2026-02-23 | 모니터링 URL 수집 플로우 추가 (섹션 16) — SDK 연결 직후 1차 수집, "--" 카드 클릭 보조 수집, Settings 상시 수집, Claude Code 연동 |
| 2026-02-23 | 모니터링 체크 항목 스펙 추가 (섹션 17) — Uptime/SSL/보안헤더 6종 체크 + Vercel Cron 구조 + monitoring_results 테이블 + 알림 트리거 |
| 2026-02-23 | Dashboard Stats 카드 실제 데이터 연동 스펙 추가 (섹션 18) — "--" → 실제 값 전환 조건, 색상 로직, 데이터 소스, 클릭 인터랙션 |
| 2026-02-23 | Overview (No SDK) 2섹션 Setup 구조 추가 (섹션 19) — Quick Monitor (URL만으로 Uptime/SSL/헤더 즉시 모니터링) + Full Protection (SDK 설치로 런타임 이벤트 탐지), 각 섹션별 value prop 카피 + 성공 상태 디자인 |
