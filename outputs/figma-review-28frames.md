# GuardRail Figma 디자인 리뷰 — 전체 28개 프레임 통합 정리

**Figma File:** https://www.figma.com/design/7tCelpMbaoPpReJkwBhnBg/Guardrail
**리뷰 수행일:** 2026-02-22
**리뷰 도구:** Figma Console MCP (figma_navigate + figma_take_screenshot) + claude.ai Figma MCP (get_design_context)

---

## 목차

1. [글로벌 디자인 시스템](#1-글로벌-디자인-시스템)
2. [페이지 프레임 (13개)](#2-페이지-프레임-13개)
3. [다이얼로그/모달 프레임 (15개)](#3-다이얼로그모달-프레임-15개)
4. [비교 분석 테이블](#4-비교-분석-테이블)
5. [누락/참고 사항](#5-누락참고-사항)

---

## 1. 글로벌 디자인 시스템

### 1.1 타이포그래피

- **Font Family:** Inter (모든 weight)
- **Weights:** Regular (400), Medium (500), Semi Bold (600), Bold (700)

| 용도 | 크기 | Weight | Line Height |
|------|------|--------|-------------|
| Hero headline | 56px | Bold | 64px |
| Section title (Landing) | 40px | Bold | - |
| Page title (Dashboard) | 26-28px | Bold/Semi Bold | - |
| Dialog title | 20-22px | Semi Bold/Bold | - |
| Empty state heading | 22px | Semi Bold | - |
| Card section title | 18px | Semi Bold | - |
| Section header (medium) | 16px | Semi Bold | - |
| Body large | 18px | Regular | 28px |
| Body / Input / Button | 14px | Regular/Medium | - |
| Small body / Table data | 13px | Regular/Medium | - |
| Helper / Label / Code | 12px | Regular/Medium | - |
| Tertiary / Meta | 11px | Regular | - |

### 1.2 컬러 팔레트

#### Neutrals
| 토큰 | HEX | 용도 |
|------|-----|------|
| Neutral 900 | `#171717` | Primary text, Primary 버튼 bg, 유저 아바타 bg, 프로그레스 fill |
| Neutral 700 | `#404040` | 코드 텍스트 |
| Neutral 600 | `#4d4d4d` | Secondary 버튼 텍스트, 코드 텍스트 |
| Neutral 500 | `#737373` | Secondary/muted 텍스트, 비활성 네비 |
| Neutral 400 | `#8c8c8c` | 헬퍼 텍스트 |
| Neutral 350 | `#a6a6a6` | Placeholder 텍스트 |
| Neutral 300 | `#b3b3b3` | Input placeholder (다이얼로그) |
| Neutral 250 | `#c7c7c7` | 비활성/disabled 텍스트 |
| Neutral 225 | `#d1d1d1` | 테마 프리뷰 스켈레톤 |
| Neutral 200 | `#d9d9d9` | 아바타 bg, 프로그레스 bar bg, 다이얼로그 input border |
| Neutral 150 | `#e5e5e5` | Card border, 구분선, 테이블 divider |
| Neutral 125 | `#e6e6e6` | Landing 페이지 border |
| Neutral 100 | `#ebebeb` | 프로그레스 bar bg (variant) |
| Neutral 50 | `#f5f5f5` | 페이지 bg, 활성 네비 bg, 코드 블록 bg, Source 배지 bg |
| Neutral 25 | `#f9f9f9` | 드롭다운 bg, Plan info card bg |
| Near-white | `#fafafa` | Input bg (설정), 테이블 헤더 bg |
| White | `#ffffff` | Card bg, Sidebar bg |

#### Green (Success / Grade A-B)
| 토큰 | HEX | 용도 |
|------|-----|------|
| Green 700 | `#1a8033` | Landing 배지 텍스트 |
| Green 600 | `#218c21` | Grade A/B 텍스트 |
| Green 500 | `#22c55e` | Online, "All clear", "Valid" 상태 |
| Green 400 | `#4db266` | Billing 체크마크 |
| Green BG | `#d9f7e0` | Grade A/B 배지 bg |
| Green Light BG | `#f0f7f0` | Landing 배지 bg |

#### Amber/Yellow (Warning / Grade C)
| 토큰 | HEX | 용도 |
|------|-----|------|
| Amber 600 | `#cc8000` | Grade C 텍스트 |
| Amber 500 | `#f59e0a` | "1 warning", "3 issues" 텍스트 |
| Amber BG | `#fff2d9` | Grade C 배지 bg |
| Amber Border | `#f2ebcc` | Warning 이슈 카드 border |

#### Orange (Grade D)
| 토큰 | HEX | 용도 |
|------|-----|------|
| Orange 600 | `#cc5900` (또는 `#cc4d1a`) | Grade D 텍스트 |
| Orange BG | `#ffecd9` (또는 `#ffebe0`) | Grade D 배지 bg |

#### Red (Critical / Danger / Grade F)
| 토큰 | HEX | 용도 |
|------|-----|------|
| Red 700 | `#d13333` | Grade F 텍스트 |
| Red 600 | `#e53333` | Danger zone border/텍스트, Delete 버튼 bg |
| Red 500 | `#ed4242` | Block IP 버튼 border/텍스트 |
| Red BG | `#ffe5e5` | Grade F 배지 bg |
| Red Light BG | `#f2d9d9` | Critical 이슈 카드 border, Danger zone card border |

#### Purple (AI/Claude Code)
| 토큰 | HEX | 용도 |
|------|-----|------|
| Purple 700 | `#7045fa` | 프롬프트 chevron ">" |
| Purple 600 | `#4d33b2` | 프롬프트 텍스트 |
| Purple BG | `#f5f2ff` | 프롬프트 카드 bg |
| Purple Border | `#e0d9ff` | 프롬프트 카드 border |

#### 기타
| 토큰 | HEX | 용도 |
|------|-----|------|
| Google Red | `#d94336` | Google "G" 아이콘 |

### 1.3 Spacing

| 요소 | 값 |
|------|-----|
| Landing 페이지 px | 120px |
| Landing 페이지 py | 80-100px |
| Dashboard content px | 48px |
| Dashboard content py | 40px |
| Card padding | 20-32px |
| Dialog padding | 28-32px |
| Sidebar px | 16px |
| Sidebar pt | 24px |
| Sidebar pb | 20px |
| Section gap (카드 간) | 24px |
| Card 내부 gap | 10-16px |
| Input px | 12-14px |
| Input py | 10-12px |
| Button px | 16-24px |
| Button py | 10-12px |
| Nav item px | 12px |
| Nav item py | 9px |
| Nav icon-to-text gap | 10px |
| Label-to-input gap | 6px |
| Form field gap | 20px |

### 1.4 Border Radius

| 요소 | 값 |
|------|-----|
| Cards (Dashboard) | 12px |
| Cards (Landing/Auth) | 16px |
| Dialogs/Modals | 16px |
| Buttons | 8px |
| Landing CTA | 10px |
| Inputs | 8px |
| Nav items (active) | 8px |
| Source badges | 12px (pill) |
| Grade badges | 7px |
| Avatar (circular) | 16px (32px element) |
| Project initial badge | 6px |
| Progress bar | 3px |

### 1.5 Shadows

| 요소 | 값 |
|------|-----|
| Auth card | `0px 4px 24px rgba(0,0,0,0.08)` |
| Dialog | `0px 8px 32px rgba(0,0,0,0.15)` |
| Setup SDK Dialog | `0px 8px 32px rgba(0,0,0,0.12)` |
| Overlay backdrop | `rgba(0,0,0,0.4)` |

### 1.6 Icons (Lucide, 24x24)

- shield (로고)
- bar-chart-2 (Overview)
- search (Scan / Run Scan / No scans)
- list (Scan History)
- settings (Settings)
- target (No projects)
- activity (Runtime Events / Scan nav)
- zap (MCP Integration)
- check-circle (Passed / All clear)
- x (Close dialog)

### 1.7 컴포넌트 공통 패턴

#### Buttons
| Variant | Background | Text | Border | Radius |
|---------|-----------|------|--------|--------|
| Primary | `#171717` | white | none | 8px |
| Secondary/Outlined | `#ffffff` | `#4d4d4d` or `#171717` | `#d9d9d9` or `#e5e5e5` | 8px |
| Danger | `#e53333` | white | none | 8px |
| Danger Outlined | transparent | `#e53333` | `#e53333` | 8px |
| Ghost/Subtle | `#f5f5f5` | `#171717` | none | 6px |

#### Inputs
| Property | Value |
|----------|-------|
| Background | `#ffffff` (dialog) or `#fafafa` (settings) |
| Border | `#d9d9d9` (dialog) or `#e5e5e5` (settings) |
| Radius | 8px |
| Padding | px-12~14, py-10~12 |
| Font | Inter Regular 14px `#171717` |
| Placeholder | Inter Regular 14px `#a6a6a6`~`#b3b3b3` |

#### Cards
| Property | Value |
|----------|-------|
| Background | `#ffffff` |
| Border | 1px solid `#e5e5e5` |
| Radius | 12px |
| Padding | 20-24px |
| Section title | 18px Semi Bold |

#### Grade Badge System
| Grade | BG | Text | Label |
|-------|-----|------|-------|
| A | `#d9f7e0` | `#218c21` | All clear |
| B | `#d9f7e0` | `#218c21` | Good |
| C | `#fff2d9` | `#cc8000` | Needs attention |
| D | `#ffecd9` | `#cc5900` | At risk |
| F | `#ffe5e5` | `#d13333` | Critical |

#### Status Indicators
| Status | Color |
|--------|-------|
| All clear / Online / Valid / No issues | `#22c55e` (green) |
| Warning / Issues (moderate) | `#f59e0a` (amber) |
| Critical issues / Danger | `#e53333` (red) |

---

## 2. 페이지 프레임 (13개)

---

### 2.1 Landing Page
- **Node ID:** `1:168`
- **프레임 크기:** 1356.625 x 2763px (긴 스크롤 페이지)

#### Layout
전체 화면 마케팅 랜딩 페이지. 5개 섹션 수직 스택.

**Navbar (100px height):**
- 왼쪽: Shield 아이콘 + "Guardrail" wordmark (bold black)
- 가운데: Features, Pricing, Docs 링크 (regular weight gray/black)
- 오른쪽: "Sign In" 버튼 (dark fill, white text, pill shape)
- 하단 녹색 accent line

**Hero Section:**
- `#f5f5f5` 배경, py-100px, px-120px
- 녹색 pill 배지: "Security for Vibe Coders" (`#f0f7f0` bg, `#1a8033` text)
- 대형 headline: **"Ship your app without security fear."** (56px Bold)
- 부제목: "Paste your GitHub URL. Get your security grade in 30 seconds. Plain language. No jargon. No setup." (gray, ~16px)
- URL 입력: white bg, `#e6e6e6` border, rounded-10px, placeholder "https://github.com/your-repo"
- "Scan Now ->" 버튼: `#171717` bg, white text, rounded-10px
- 하단 소텍스트: "Free for up to 3 projects. No credit card required"

**"Why Guardrail?" Section:**
- White bg, py-80px, px-120px
- 제목: "Why Guardrail?" (40px Bold, centered)
- 부제목: "Everything you need to ship securely, explained in plain language."
- 2x2 feature 카드 그리드 (각 360px, white bg, `#e6e6e6` border, 12px radius, 28px padding):
  1. **Pre-launch Scan** (search icon) — API keys, passwords, payment security 체크
  2. **Real-time Guard** (shield icon) — 24/7 모니터링, uptime, SSL, brute force
  3. **MCP Integration** (zap icon) — Claude Code 연동, 에디터에서 스캔
  4. **Dashboard** (bar-chart icon) — A-F 등급, 스캔 이력

**"How it works" Section:**
- `#f5f5f5` bg
- 3단계 수평 레이아웃 (1, 2, 3 원형 번호):
  1. **Paste your URL** — "GitHub URL or connect via MCP"
  2. **Get your grade** — "Security grade in 30 seconds"
  3. **Fix with guides** — "Plain-language fix instructions"

**Pricing Section:**
- White bg, py-80px, px-120px
- 제목: "Simple pricing. Start free." (40px Bold)
- 부제목: "Every feature included in both plans. Only difference? How many projects you need."
- 2개 카드 (각 340px):
  - **Free ($0/month):** 1px border `#e6e6e6`, "For your first vibe-coded app", 7개 체크마크 항목
  - **Pro ($19/month):** 2px border `#171717` (강조), "For Growing Projects" 녹색 배지, 6개 항목, "Upgrade to Pro" 검은 버튼
- 하단: "Save 2 months with annual billing!" 녹색 링크

**Footer:**
- 어두운 charcoal bg
- Guardrail 로고 + "Security for Vibe Coders" 태그라인
- 4컬럼: Product (Features, Pricing, Docs), Legal (Privacy, Terms), Connect (X/Twitter, GitHub)
- Copyright: "2026 Guardrail. All rights reserved."

---

### 2.2 Dashboard - Overview
- **Node ID:** `1:692`
- **프레임 크기:** 1356.625 x 1293px

#### Layout
표준 대시보드: 왼쪽 사이드바 + 메인 콘텐츠.

**사이드바 (260px, white bg):**
- 로고: Shield + "Guardrail" (Bold 18px)
- 프로젝트 드롭다운: `#f9f9f9` bg, `#e5e5e5` border, 8px radius
  - Grade A 배지 (녹색) + "My SaaS App" (Semi Bold 13px) + "my-saas.vercel.app" (Regular 11px) + chevron
- 네비게이션 (4개):
  - **Overview** (bar-chart-2) — 활성: `#f5f5f5` bg, Semi Bold, `#171717`
  - **Scan** (activity/search) — 비활성: Medium, `#737373`
  - **Scan History** (list) — 비활성
  - **Settings** (settings) — 비활성
- 하단:
  - Plan Info: `#f9f9f9` bg, "Free Plan" + "2 of 3 projects used" + Progress bar (6px height, `#171717` fill) + "Upgrade" 버튼
  - User Profile: "P" 아바타 (32px, `#171717` bg) + "Park" + "park@email.com"

**메인 콘텐츠 (`#f5f5f5` bg, px-48, py-40):**

*Header Row:*
- "Overview" (Bold 26px) + "My SaaS App · Last scan: 2 hours ago" (Regular 14px, gray)
- "Run Scan" 버튼 (dark bg, search icon)

*Stats Row (4개 카드, equal-width, gap-16):*
| 카드 | Label | Description | Value | Status |
|------|-------|-------------|-------|--------|
| Security Grade | Medium 13px gray | "Overall safety score A~F" 11px | Grade A 배지 (Bold 22px) | "All clear" `#218c21` + check-circle |
| Uptime | Medium 13px | "Is your app running?" 11px | "100%" Bold 24px | "Online" `#22c55e` |
| SSL Certificate | Medium 13px | "Your site's security lock" 11px | "287 days" Bold 24px | "Valid" `#22c55e` |
| Open Issues | Medium 13px | "Things that need fixing" 11px | "0" Bold 24px | "No issues found" `#22c55e` |

*Runtime Events 카드:*
- "Runtime Events" (Semi Bold 16px) + "View all ->" 링크
- "Real-time security alerts from your app"
- 4개 이벤트 행:
  1. Red dot — "Brute force detected" 10:23 AM — "Block IP" (red outlined 버튼)
  2. Orange dot — "Traffic spike detected" 09:45 AM — "View details" 버튼
  3. Orange dot — "Admin access at unusual hour" 03:12 AM — "Was this you?" 버튼
  4. Green dot — "Login from new location" Yesterday — "Was this you?" 버튼

*Recent Scans 카드:*
- "Recent Scans" (Semi Bold 16px) + "View all ->" 링크
- 3행: Grade 배지 | Date | Source tag (MCP/Web/Auto pill) | Checks | Status

---

### 2.3 Dashboard - Scan Result
- **Node ID:** `1:782`
- **프레임 크기:** 1357 x 2110px (긴 스크롤)

#### Layout
사이드바 + 콘텐츠. 프로젝트: "Side Project" Grade C.

**Header:**
- 브레드크럼: "Scans > Scan #12" (13px gray)
- "Scan Result" (Bold 26px)
- "Scanned today at 10:23 AM - via MCP" (14px gray)
- "Rescan" 버튼 (dark bg, refresh icon)

**Summary Card:**
- Grade C 배지 (64px height, 32px text, 14px radius)
- "A few things need fixing"
- "5 of 8 checks passed - 2 critical - 1 warning"

**Issues to Fix (2):**
- Issue Card 1 (border `#f2d9d9` red tint, 1.5px):
  - Red dot + "Stripe secret key is exposed in code"
  - Description + "How to fix?" 버튼 (`#f5f5f5` bg, 6px radius)
- Issue Card 2 (border `#f2ebcc` amber tint):
  - Amber dot + "No rate limit on login attempts"
  - Description + "How to fix?" 버튼

**Passed Checks (5):**
- Green check-circle 아이콘 + 항목명, `#e5e5e5` divider로 구분:
  1. Passwords are securely encrypted
  2. API keys are not exposed in code
  3. .env file is excluded from Git
  4. HTTPS is enabled
  5. Privacy policy page exists

**Security Grade Guide Card:**
- "Security Grade" + "Based on your latest scan results"
- A~F 5개 등급 행 (배지 36x36, 10px radius):
  - A (green): "All clear" — "No security issues found. All checks passed."
  - B (green): "Good" — "Minor warnings only. No critical vulnerabilities."
  - C (amber): "Needs attention" — "Some issues found. 1-2 warnings."
  - D (orange): "At risk" — "Critical issues detected. Immediate action recommended."
  - F (red): "Critical" — "Multiple critical vulnerabilities. Your app is exposed."

---

### 2.4 Dashboard - Project Settings
- **Node ID:** `1:914`
- **프레임 크기:** 1357 x 1593px

#### Layout
사이드바 + 콘텐츠. Settings 탭 활성.

**"Project Settings" (Bold 26px)**

**General Card** (white, `#e5e5e5` border, 12px radius, 24px padding):
- "General" (Semi Bold 18px)
- "Project Name" label (Medium 13px `#737373`) + Input (`#fafafa` bg, `#e5e5e5` border, 8px radius)
- "Project URL" label + Input

**MCP Connection Card:**
- "MCP Connection" (Semi Bold 18px)
- "Connect Guardrail to Claude Code for scanning from your editor" (12px `#737373`)
- "Project Key" label + 마스킹된 키 표시 (gr_sk_a1b2c3d4e5...xxxx) + "Copy" 버튼 (dark)
- "Regenerate Key" 링크 (Medium 13px `#e53333`) — "current key will stop working"
- "Claude Code Setup" label + 코드 블록 (dark bg `#262626`, 8px radius, px-18 py-16):
  ```json
  {
    "mcpServers": {
      "guardrail": {
        "command": "npx",
        "args": ["guardrail-mcp@latest"]
      }
    }
  }
  ```

**Danger Zone Card** (white bg, `#f2d9d9` border):
- "Danger Zone" (Semi Bold 18px `#e53333`)
- "These actions are permanent and cannot be undone." *(UX Writer 업데이트: 공감적 톤으로 전환)*
- "Delete Project" 버튼 (outlined, `#e53333` border/text)

---

### 2.5 Scans History
- **Node ID:** `1:974`
- **프레임 크기:** 1356 x 1463px

#### Layout
사이드바 + 콘텐츠. "Scan History" 탭 활성.

**"Scan History" (Bold 26px)**

**테이블 카드** (white, `#e5e5e5` border, 12px radius, px-24 pb-4):
- 헤더 (`#fafafa` bg, Semi Bold 12px `#737373`): Grade | Date (flex-1) | Source | Checks | Status
- 6개 행 (py-14, `#e5e5e5` divider):

| Grade | Date | Source | Checks | Status |
|-------|------|--------|--------|--------|
| A (green) | Today, 10:23 AM | MCP | 8/8 passed | All clear (`#22c55e`) |
| A (green) | Yesterday, 3:15 PM | Web | 8/8 passed | All clear |
| B (green) | Feb 20, 9:00 AM | Auto | 7/8 passed | 1 warning (`#f59e0a`) |
| C (amber) | Feb 19, 2:30 PM | MCP | 5/8 passed | 3 issues (`#f59e0a`) |
| C (amber) | Feb 18, 11:00 AM | Web | 5/8 passed | 3 issues |
| D (red/dark) | Feb 17, 4:00 PM | MCP | 3/8 passed | 5 issues (`#e53333`) |

- Grade 배지: 30px height, 7px radius
- Source 태그: `#f5f5f5` bg, 12px pill radius, px-10 py-4, Medium 11px `#737373`

---

### 2.6 Dashboard - Empty - No Projects
- **Node ID:** `1:1175`
- **프레임 크기:** 1356.625 x 900px

#### Layout
사이드바 + 콘텐츠. 사이드바 네비 항목 모두 **disabled** (`#c7c7c7`). 프로젝트 드롭다운 없음.

**Empty State (centered):**
- target 아이콘 (24x24)
- "No projects yet" (Semi Bold 22px, text-center) *(UX Writer 업데이트: "Create your first project!" → 상태 설명으로 전환)*
- "Add your app to start finding security issues." (Regular 15px `#737373`, text-center) *(UX Writer 업데이트: 기능→혜택 관점 전환)*
- "Free plan includes up to 3 projects."
- **"+ New Project"** 버튼 (Semi Bold 14px, `#171717` bg, white text, 8px radius, px-24 py-12)

---

### 2.7 Dashboard - Empty - No Scans
- **Node ID:** `1:1194`
- **프레임 크기:** 1356.625 x 900px

#### Layout
사이드바 + 콘텐츠. 프로젝트 존재하지만 네비 항목 disabled (`#c7c7c7`).

**Empty State (centered):**
- search 아이콘 (24x24)
- "No scans yet" (Semi Bold 22px, text-center)
- "Run your first security scan." (Regular 15px `#737373`, text-center)
- "Enter a GitHub URL or scan from Claude Code."
- URL Input Row:
  - Input: white bg, `#e5e5e5` border, 8px radius, 360px wide, h-44, placeholder "https://github.com/user/repo"
  - "Scan" 버튼 (`#171717` bg, Semi Bold 14px, 8px radius)
- "or scan directly from Claude Code" (Regular 13px `#737373`) *(UX Writer 업데이트: "via MCP" 전문 약어 제거)*

---

### 2.8 Dashboard - Empty - No Monitoring
- **Node ID:** `1:1216`
- **프레임 크기:** 1356.625 x 900px

#### Layout
사이드바 + 콘텐츠. Overview 구조와 유사하나 Runtime Events 영역이 empty state.

> **참고:** 이 프레임은 Figma Console 스크린샷 리뷰에서 직접 캡처되지 않았으나, Overview (No SDK) 프레임 (`2:2515`)과 유사한 패턴으로 Runtime Events 영역만 비어있는 상태입니다. 스펙 문서와 Figma 파일 구조 탐색으로 확인된 내용입니다.

---

### 2.9 Dashboard - Overview (No SDK)
- **Node ID:** `2:2515`
- **프레임 크기:** 1356.625 x 1293px

#### Layout
Overview와 동일하되, Runtime Events 섹션이 SDK 미설치 상태로 대체됨.

**SDK Setup Prompt Card (centered, p-28):**
- activity 아이콘 (24x24)
- "Runtime Events" (Semi Bold 16px)
- "Install the Guardrail SDK to detect password attacks, suspicious logins, and other threats while your app runs." (Regular 14px `#737373`, centered, 2줄)
- **"Setup SDK"** 버튼 (`#171717` bg, Medium 14px, 8px radius, px-20 py-10)
- Install snippet: `npm install @guardrail/sdk` (`#f5f5f5` bg, 8px radius, px-16 py-12, Regular 13px `#4d4d4d`)

나머지 (Stats Row, Recent Scans)는 Overview와 동일.

---

### 2.10 Profile Settings
- **Node ID:** `2:2644`
- **프레임 크기:** 1440 x 1272px

#### Layout
사이드바 + 콘텐츠. 4개 카드 수직 스택, 24px gap.

**"Profile" (Semi Bold 28px)**

**Account Info Card** (white, `#e5e5e5` border, 12px radius, 24px padding):
- "Account Info" (Semi Bold 18px)
- Name: 편집 가능, "Park" (dark text)
- Email: "(read only)" label, "park@email.com" (gray text, disabled)
- "Save" 버튼 (dark)

**Appearance Card:**
- "Appearance" (Semi Bold 18px)
- "Theme" label (Medium 14px)
- 2개 테마 프리뷰 (148x88px):
  - **Light**: 선택됨, 2px `#171717` border, 미니 대시보드 목업
  - **Dark**: 미선택, 1px `#e5e5e5` border, 어두운 프리뷰

**Change Password Card:**
- "Change Password" (Semi Bold 18px)
- Current Password, New Password, Confirm Password (3개 input)
- "Update Password" 버튼 (dark)

**Danger Zone Card** (`#e53333` red border):
- "Danger Zone" (Semi Bold 18px, `#e53333` text)
- "Delete Account" + "This action cannot be undone. All your data will be permanently deleted."
- "Delete" 버튼 (red bg `#e53333`, white text, 오른쪽 정렬)

---

### 2.11 Plan & Billing
- **Node ID:** `2:2724`
- **프레임 크기:** 1440 x 900px

#### Layout
사이드바 + 콘텐츠.

**"Plan & Billing" (Semi Bold 28px)**

**Plans Row (2개 카드, flex-1 equal width, 24px gap):**

*Current Plan Card (259px height):*
- "Current Plan" + "Free Plan" 배지 (pill, `#f5f5f5` bg, `#e5e5e5` border)
- 체크마크 리스트 (green `#4db266`):
  - 3 projects
  - All core features *(UX Writer 업데이트: "All features included" → Free/Pro 차별화)*
  - Community support

*Pro Plan Card:*
- "Pro Plan" + "For growing teams" + **"$19/month"** (Bold 24px, 오른쪽 정렬)
- 체크마크 리스트:
  - Unlimited projects
  - All features included
  - Priority support
- **"Start Free Trial"** 버튼 (full-width, dark bg)
- "14-day free trial" (12px gray, centered)

**Billing Info Card:**
- "Billing Info" (Semi Bold 18px)
- "No billing information on file." (14px gray)
- "Add Payment Method" 버튼 (outlined, white bg, `#e5e5e5` border)

---

### 2.12 Login
- **Node ID:** `3:939`
- **프레임 크기:** 1280 x 800px

#### Layout
`#f5f5f5` 전체 화면 배경 + 중앙 420px 카드.

**Card** (white bg, 16px radius, shadow `0px 4px 24px rgba(0,0,0,0.08)`, 40px padding):
- Guardrail 로고 (dark square 28px + "Guardrail" Bold 22px)
- **"Welcome back"** (Semi Bold 24px)
- "Sign in to your account" (14px gray) *(UX Writer 업데이트: "to continue" 필러 제거)*
- Email 입력 (label "Email" Medium 14px, placeholder "you@example.com")
- Password 입력 (label "Password", masked)
- "Forgot password?" 링크 (Medium 13px gray)
- **"Sign In"** 버튼 (full-width, dark bg, 8px radius, ~48px height)
- "or" 구분선
- **"Continue with Google"** (outlined, "G" red icon)
- **"Continue with GitHub"** (outlined, GitHub icon)
- "Don't have an account? **Sign up**" 링크

---

### 2.13 Sign Up
- **Node ID:** `3:967`
- **프레임 크기:** 1280 x 800px

#### Layout
Login과 동일한 패턴. 약간 더 높은 위치 (top=100px).

**Card** (420px, 16px radius, shadow):
- 로고 + "Guardrail"
- **"Create your account"** (Semi Bold 24px)
- "Start securing your app in minutes" (14px gray)
- Name 입력 (placeholder "Your name")
- Email 입력 (placeholder "you@example.com")
- Password 입력 (masked)
- "Must be at least 8 characters" (12px `#a6a6a6`)
- **"Create Account"** 버튼 (full-width, dark bg)
- "or" 구분선
- "Continue with Google" / "Continue with GitHub"
- "Already have an account? **Sign in**"
- Terms 텍스트 (11px `#a6a6a6`, Terms + Privacy 링크)

---

## 3. 다이얼로그/모달 프레임 (15개)

---

### 3.1 New Project Dialog
- **Node ID:** `2:2411`
- **프레임 크기:** 1440 x 900px (전체 화면 + overlay)

#### Layout
`rgba(0,0,0,0.4)` overlay + 중앙 480px 모달.

**Modal** (white bg, 16px radius, 32px padding, gap-24, shadow `0px 8px 32px rgba(0,0,0,0.15)`):
- Header: "New Project" (Semi Bold 22px) + X 닫기 아이콘
- "Create a new project to start scanning for security issues." (Regular 14px `#737373`)
- Form (gap-20):
  - **Project Name*** (필수, `*` `#e53333`): placeholder "My Awesome App"
  - **Project URL**: placeholder "https://my-app.vercel.app", helper "We'll monitor if your app stays online and secure" (12px `#8c8c8c`) *(UX Writer 업데이트: 괄호 전문 용어 제거)*
  - **GitHub Repository**: placeholder "https://github.com/user/repo", helper "We'll scan your code for security issues"
- Footer (gap-12, justify-end):
  - "Cancel" (outlined, `#d9d9d9` border, `#4d4d4d` text)
  - "Create Project" (dark bg, white text)

---

### 3.2 Setup SDK Dialog
- **Node ID:** `3:1042` (전체 다이얼로그), `3:1006` (컴포넌트)
- **프레임 크기:** 1357 x 1625 (overlay 포함) / 520 x 621 (컴포넌트)

#### Layout
gray overlay + 중앙 ~380px 모달 (white bg, 16px radius, 28px padding, gap-20).

**Content:**
1. Header: "Setup SDK" (Bold 20px) + X 닫기
2. Description: "Install the Guardrail SDK to detect password attacks, suspicious logins, and other threats while your app runs." (Regular 14px `#737373`)
3. Separator (1px `#e5e5e5`)
4. **Step 1** (24px `#171717` 원형 배지 "1"):
   - "Install the SDK" (Semi Bold 14px)
   - 코드 블록 (`#f5f5f5` bg, 8px radius): `npm install @guardrail/sdk`
5. **Step 2** (원형 배지 "2"):
   - "Add the middleware to your app" (Semi Bold 14px)
   - 코드 블록 (multi-line):
     ```
     import { guardrail } from '@guardrail/sdk'
     app.use(guardrail({
       projectKey: process.env.GUARDRAIL_PROJECT_KEY
     }))
     ```
6. **Step 3** (보라색/sparkle 아이콘):
   - "Or ask Claude Code" (Semi Bold 14px) *(UX Writer 업데이트: "just" 필러 제거)*
   - "If you're using Claude Code, say:" *(UX Writer 업데이트: "just" 필러 제거)*
   - **프롬프트 카드** (`#f5f2ff` bg, `#e0d9ff` border, 8px radius):
     - ">" (`#7045fa` Semi Bold 16px) + "Install Guardrail SDK in my project" (`#4d33b2` Medium 13px)
7. Separator
8. Footer: "Close" (outlined) + "Copy Project Key" (dark bg)

---

### 3.3 How to Fix — Stripe key exposed (Critical)
- **Node ID:** `3:213`
- **프레임 크기:** 1357 x 1625

#### Layout
gray overlay + 중앙 white 모달 (~380px).

**Content:**
1. **배지:** Red "Critical" (`~#E53935` bg, white text)
2. **제목:** "Stripe secret key is exposed in code"
3. **부제:** "Anyone can manipulate payments with this key. Move it to environment variables."
4. Separator
5. **"Why this matters"** — Stripe secret key (sk_live_...) 하드코딩 위험 설명
6. Separator
7. **"How to fix"** — 4 Steps:
   - Step 1: "Move the key to a .env.local file" + 코드 (`# .env.local` / `STRIPE_SECRET_KEY=sk_live_your_key_here`)
   - Step 2: "Update code to use environment variable" + Before/After 코드
   - Step 3: "Ensure .env.local is in .gitignore" + 코드 (`.gitignore`)
   - Step 4: "Rotate your Stripe key in the Stripe Dashboard" (텍스트만)
8. **"Ask AI to fix this"** (sparkle 아이콘):
   - Prompt 1: "> Move my Stripe secret key to .env.local and update all references"
   - Prompt 2: "> Find all hardcoded API keys in my project and move them to environment variables"
9. Separator
10. **Footer:** Close + Rescan + **"Mark as Fixed"** (dark bg)

---

### 3.4 How to Fix — No rate limit (Warning)
- **Node ID:** `3:277`
- **프레임 크기:** 1357 x 1625

#### Layout
동일 모달 패턴.

**Content:**
1. **배지:** Orange/Amber "Warning" (`~#F9A825` bg, dark text)
2. **제목:** "No rate limit on login attempts"
3. **부제:** "Someone can try passwords endlessly. You need a rate limiter."
4. **"Why this matters"** — brute-force 공격 위험 설명
5. **"How to fix"** — 3 Steps:
   - Step 1: "Install a rate limiting package" + `npm install @upstash/ratelimit @upstash/redis`
   - Step 2: "Add rate limiting to your login endpoint" + **가장 큰 코드 블록** (~15줄, Upstash ratelimit)
   - Step 3: "Add progressive delays after failed attempts" (텍스트만)
6. **"Ask AI to fix this":**
   - Prompt 1: "> Add rate limiting to my login API endpoint -- max 5 attempts per minute per IP"
   - Prompt 2: "> Set up Upstash Redis rate limiter with progressive delays for failed logins"
7. **Footer:** Close + Rescan + **"Mark as Fixed"**

---

### 3.5 How to Fix — Passwords encrypted (Passed)
- **Node ID:** `3:334`
- **프레임 크기:** 1357 x 1625

**Content:**
1. **배지:** Green "Passed" (`~#4CAF50` bg, white text)
2. **제목:** "Passwords are securely encrypted"
3. **부제:** "Your application correctly hashes passwords before storing them."
4. **"If this breaks, here's what to do"** — plaintext 저장 위험 설명
5. **"How to fix if this fails"** — 3 Steps:
   - Step 1: "Ensure you're using bcrypt or Argon2 for hashing" + bcrypt 코드
   - Step 2: "Never store passwords in plaintext or with weak hashing (MD5, SHA1)" + Before/After 코드
   - Step 3: "Set a minimum password length policy (8+ characters)" (텍스트만)
6. **AI Prompts:**
   - "> Check that all passwords in my database are hashed with bcrypt or Argon2"
   - "> Audit my authentication code for any plaintext password storage"
7. **Footer:** Close + Rescan (2버튼, "Mark as Fixed" 없음)

---

### 3.6 How to Fix — API keys not exposed (Passed)
- **Node ID:** `3:389`
- **프레임 크기:** 1357 x 1625

**Content:**
1. **배지:** Green "Passed"
2. **제목:** "API keys are not exposed in code"
3. **부제:** "No hardcoded API keys or secrets were found in your source code."
4. **"If this breaks, here's what to do"** — key 노출 위험 설명
5. **"How to fix if this fails"** — 3 Steps:
   - Step 1: "Move all secrets to environment variables" + `.env.local` 코드 (DATABASE_URL, API_KEY, SENDGRID_KEY)
   - Step 2: "Use a secrets scanner in your CI pipeline" + **GitHub Actions YAML** (trufflehog) — 유일한 YAML 코드
   - Step 3: "Rotate any keys that may have been exposed previously" (텍스트만)
6. **AI Prompts:**
   - "> Scan my entire codebase for hardcoded API keys, tokens, and secrets"
   - "> Set up a pre-commit hook to prevent committing secrets"
7. **Footer:** Close + Rescan

---

### 3.7 How to Fix — .env excluded from Git (Passed)
- **Node ID:** `3:444`
- **프레임 크기:** 1357 x 1625

**Content:**
1. **배지:** Green "Passed"
2. **제목:** ".env file is excluded from Git"
3. **부제:** "Your .env file is listed in .gitignore -- secrets aren't visible publicly."
4. **"If this breaks, here's what to do"** — Git 이력 노출 위험
5. **"How to fix if this fails"** — 3 Steps:
   - Step 1: "Add .env files to .gitignore" + `.gitignore` 코드
   - Step 2: "Remove .env from Git history if it was ever committed" + **git 명령어 + bfg** (유일)
   - Step 3: "Rotate all secrets that may have been exposed" (텍스트만)
6. **AI Prompts:**
   - "> Check if any .env files are tracked by Git and remove them from history"
   - "> Audit my .gitignore to make sure all secret files are excluded"
7. **Footer:** Close + Rescan

---

### 3.8 How to Fix — HTTPS enabled (Passed)
- **Node ID:** `3:499`
- **프레임 크기:** 1357 x 1625

**Content:**
1. **배지:** Green "Passed"
2. **제목:** "HTTPS is enabled"
3. **부제:** "Data is sent securely -- all traffic uses encrypted HTTPS connections."
4. **"If this breaks, here's what to do"** — plaintext 통신 위험
5. **"How to fix if this fails"** — 3 Steps:
   - Step 1: "Ensure your hosting provider has SSL/TLS enabled" + certbot 명령어
   - Step 2: "Redirect all HTTP traffic to HTTPS" + **Next.js config** (Strict-Transport-Security 헤더)
   - Step 3: "Check for mixed content (HTTP resources on HTTPS pages)" (텍스트만)
6. **AI Prompts:**
   - "> Enable HTTPS and add Strict-Transport-Security headers to my Next.js app"
   - "> Find and fix any mixed content issues (HTTP resources on HTTPS pages)"
7. **Footer:** Close + Rescan

---

### 3.9 How to Fix — Privacy policy exists (Passed)
- **Node ID:** `3:554`
- **프레임 크기:** 1357 x 1625

**Content:**
1. **배지:** Green "Passed"
2. **제목:** "Privacy policy page exists"
3. **부제:** "Your application has a privacy policy page accessible to users."
4. **"If this breaks, here's what to do"** — GDPR/CCPA/COPPA 위반, 앱스토어 요구사항
5. **"How to fix if this fails"** — 3 Steps:
   - Step 1: "Create a /privacy page in your application" + **React TSX 코드** (유일한 JSX 코드)
   - Step 2: "Include required sections: data collection, usage, sharing, user rights" (텍스트만)
   - Step 3: "Add a link to the privacy policy in your footer" (텍스트만)
6. **AI Prompts:**
   - "> Generate a GDPR-compliant privacy policy page for my Next.js app"
   - "> Create a /privacy route with all legally required sections"
7. **Footer:** Close + Rescan

> **특이점:** Steps 2, 3 모두 텍스트만 (코드 블록 1개뿐). 7종 중 가장 텍스트 중심.

---

### 3.10 Runtime — Block IP (Brute Force)
- **Node ID:** `3:624`
- **프레임 크기:** 1357 x 1293

#### Layout
gray overlay + compact white 모달.

**Content:**
1. Red dot + **"Brute Force"** (red text) + "10:23 AM" (gray) + X 닫기
2. **"Block IP: 203.0.113.42?"** (Bold ~18px)
3. "This IP attempted 15 failed logins in 3 minutes, targeting the account park@email.com. This pattern strongly suggests an automated brute-force attack."
4. Separator
5. **"Details"** (5행 key-value 테이블):
   - IP Address: **203.0.113.42**
   - Failed attempts: **15 in 3 minutes**
   - Target account: **park@email.com**
   - Location: **Unknown (VPN/Proxy)**
   - User agent: **python-requests/2.28.1**
6. Separator
7. **"Recommended actions"** (4개 bullet):
   - Block this IP from all login attempts
   - Enable CAPTCHA after 3 failed attempts
   - Notify the account owner about suspicious activity
   - Consider adding the IP range to your blocklist
8. Separator
9. **Footer:** "Dismiss" (ghost) + **"Block IP"** (**RED** 버튼 `~#E53935`)

---

### 3.11 Runtime — View Details (Traffic Spike)
- **Node ID:** `3:676`
- **프레임 크기:** 1357 x 1293

**Content:**
1. Orange dot + **"Traffic Spike"** (orange text) + "09:45 AM"
2. **"Unusual traffic spike detected"**
3. "Your app is receiving 7x more traffic than usual (680% above normal). This could be a DDoS attack, viral content, or bot crawl."
4. **"Details"** (5행):
   - Current traffic: **4,200 req/min (normal: 600)**
   - Duration: **Started 45 minutes ago**
   - Top path: **/api/login (62% of requests)**
   - Top referrer: **Direct / No referrer (89%)**
   - Error rate: **12% (normally 0.5%)**
5. **"Recommended actions"** (4개 bullet):
   - Monitor for the next 30 minutes
   - Enable rate limiting if error rate increases
   - Check if traffic is from legitimate users or bots
   - Scale up infrastructure if traffic is legitimate
6. **Footer:** "Dismiss" (ghost) + **"Enable Rate Limiting"** (dark/black 버튼)

---

### 3.12 Runtime — Was This You? (Admin Access)
- **Node ID:** `3:728`
- **프레임 크기:** 1357 x 1293

**Content:**
1. Orange dot + **"Suspicious"** (orange text) + "03:12 AM"
2. **"Admin access at unusual hour"**
3. "Someone logged into the admin panel at 3:12 AM. This is outside your usual activity hours (9 AM - 11 PM)."
4. **"Details"** (5행):
   - Account: **park@email.com (Admin)**
   - Time: **3:12 AM (unusual)**
   - IP Address: **192.168.1.x**
   - Location: **Seoul, South Korea**
   - Device: **Chrome 121 / macOS**
5. **"Recommended actions" 없음** — Details에서 바로 Footer로
6. **Footer:** **"No, secure my account"** (RED 버튼, 왼쪽) + "Yes, it was me" (ghost/outlined, 오른쪽)

> **UX 의도:** secure 액션(빨간 버튼)이 왼쪽에 더 눈에 띄게 배치.

---

### 3.13 Runtime — Was This You? (New Location)
- **Node ID:** `3:765`
- **프레임 크기:** 1357 x 1293

**Content:**
1. **Green** dot + **"New Location"** (green text) + "Yesterday"
2. **"Login from a new location"**
3. "We detected a login from Germany (Frankfurt) -- a location not previously associated with your account."
4. **"Details"** (5행):
   - Account: **park@email.com**
   - Location: **Frankfurt, Germany**
   - IP Address: **85.214.132.xxx**
   - Device: **Safari 17 / iOS 18**
   - First login from: **This country**
5. **"Recommended actions" 없음**
6. **Footer:** **"No, secure my account"** (RED) + "Yes, it was me" (ghost)

> Admin(3:728)과의 차이: Green dot (더 낮은 심각도), 다른 위치/디바이스, "(Admin)" 역할 표시 없음, "First login from" 항목.

---

### 3.14 Account Menu (Popup)
- **Node ID:** `2:1331`
- **프레임 크기:** 1440 x 900px (전체 레이아웃)

#### Layout
전체 앱 레이아웃에서 Account Menu가 열린 상태.

**사이드바 하단:**
- "P" 아바타 + "Park" + "park@email.com" 클릭 영역
- 위로 떠오르는 **Account Menu 팝업** (white card, shadow):
  - "Park" (bold) + "park@email.com" (gray)
  - Divider
  - "Profile" (person icon) — black text
  - "Plan & Billing" (card icon) — black text
  - **"Log out"** (exit icon) — **RED text** (`~#E53935`)

---

### 3.15 Account Menu (Component)
- **Node ID:** `2:1382`
- **프레임 크기:** 320 x 307px (독립 컴포넌트)

#### Layout
Account Menu 드롭다운만 추출한 디자인 컴포넌트.

**Card** (320px wide, white bg, 12px radius, drop shadow):
1. **유저 행:** "P" 아바타 (40px, dark rounded rectangle, white "P") + "Park" (bold 16px) + "park@email.com" (gray 14px)
2. Divider
3. **"Profile"** (person icon) + "Manage your account" (gray 12px 서브텍스트)
4. Divider
5. **"Plan & Billing"** (card icon) + "Free Plan · 2/3 projects" (gray 12px 서브텍스트)
6. Divider
7. **"Log out"** (exit icon) — **RED text** (`~#E53935`), 서브텍스트 없음

> **Component vs Popup 차이:**
> - Component(2:1382)가 더 상세: 각 메뉴에 서브텍스트 표시
> - 아바타 스타일: Component는 rounded rectangle, Popup에서는 circular

---

## 4. 비교 분석 테이블

### 4.1 How to Fix 모달 7종 비교

| 항목 | Stripe key (3:213) | Rate limit (3:277) | Passwords (3:334) | API keys (3:389) | .env/Git (3:444) | HTTPS (3:499) | Privacy (3:554) |
|------|-----|-----|-----|-----|-----|-----|-----|
| **배지** | Red "Critical" | Orange "Warning" | Green "Passed" | Green "Passed" | Green "Passed" | Green "Passed" | Green "Passed" |
| **설명 제목** | "Why this matters" | "Why this matters" | "If this breaks..." | "If this breaks..." | "If this breaks..." | "If this breaks..." | "If this breaks..." |
| **수정 제목** | "How to fix" | "How to fix" | "How to fix if this fails" | "How to fix if this fails" | "How to fix if this fails" | "How to fix if this fails" | "How to fix if this fails" |
| **Step 수** | 4 | 3 | 3 | 3 | 3 | 3 | 3 |
| **코드 블록 수** | 3 | 2 | 2 | 2 | 2 | 2 | 1 |
| **텍스트만 Step** | Step 4 | Step 3 | Step 3 | Step 3 | Step 3 | Step 3 | Steps 2, 3 |
| **Footer 버튼** | 3개 (Close, Rescan, Mark as Fixed) | 3개 | 2개 (Close, Rescan) | 2개 | 2개 | 2개 | 2개 |
| **AI 프롬프트** | 2개 | 2개 | 2개 | 2개 | 2개 | 2개 | 2개 |
| **코드 언어** | JS/env | JS (Upstash) | JS (bcrypt) | env/YAML | gitignore/git | shell/Next.js | TSX/React |

#### 핵심 패턴
- **Critical/Warning** (이슈): "Why this matters" + "How to fix" + 3버튼 Footer (Mark as Fixed 포함)
- **Passed** (통과): "If this breaks, here's what to do" + "How to fix if this fails" + 2버튼 Footer
- 모든 7종 동일 구조: Badge → Title → Subtitle → Separator → 설명 → Separator → Fix Steps → "Ask AI to fix this" (2개 프롬프트) → Separator → Footer

### 4.2 Runtime 모달 4종 비교

| 항목 | Brute Force (3:624) | Traffic Spike (3:676) | Admin (3:728) | New Location (3:765) |
|------|-----|-----|-----|-----|
| **Dot 색상** | Red | Orange | Orange | Green |
| **카테고리** | "Brute Force" | "Traffic Spike" | "Suspicious" | "New Location" |
| **타임스탬프** | 10:23 AM | 09:45 AM | 03:12 AM | Yesterday |
| **Details 행** | 5 | 5 | 5 | 5 |
| **Recommended actions** | 4 bullets | 4 bullets | 없음 | 없음 |
| **패턴** | Action | Action | Confirm | Confirm |
| **Primary 버튼** | "Block IP" (RED) | "Enable Rate Limiting" (dark) | "Yes, it was me" (ghost) | "Yes, it was me" (ghost) |
| **Secondary 버튼** | "Dismiss" (ghost) | "Dismiss" (ghost) | "No, secure my account" (RED) | "No, secure my account" (RED) |

#### 핵심 패턴
- **Action 패턴** (Brute Force, Traffic Spike): Details + Recommended actions + Dismiss/Action 버튼
- **Confirm 패턴** (Admin, New Location): Details만 + "Was this you?" 이진 선택 ("No, secure" RED 왼쪽 / "Yes" ghost 오른쪽)
- 모든 4종: Category dot/label/timestamp → Title → Description → Separator → Details (5행) → [Separator → Recommended actions] → Separator → Buttons

### 4.3 사이드바 공통 구조

```
Sidebar (260px, white bg, flex-col, h-full, pt-24 pb-20 px-16)
├── Logo Row: Shield icon + "Guardrail" Bold 18px (gap-8, pb-20)
├── Project Dropdown: #f9f9f9 bg, #e5e5e5 border, 8px radius, px-12 py-10
│   ├── Grade Badge (28px, 6px radius)
│   ├── Project Name (Semi Bold 13px)
│   ├── Project URL (Regular 11px)
│   └── Chevron
├── Spacer (16px)
├── Nav Items (each: flex, gap-10, px-12 py-9, rounded-8)
│   ├── Overview (bar-chart-2)
│   ├── Scan (search/activity)
│   ├── Scan History (list)
│   └── Settings (settings)
│   ── Active: #f5f5f5 bg, Semi Bold 14px #171717
│   ── Inactive: no bg, Medium 14px #737373
│   ── Disabled: no bg, Medium 14px #c7c7c7
├── [Flex spacer (flex-1)]
├── Plan Info Card: #f9f9f9 bg, #e5e5e5 border, 10px radius, p-12~14
│   ├── "Free Plan" Semi Bold 13px
│   ├── "2 of 3 projects used" Regular 11-12px #737373
│   ├── Progress bar (6px height, #d9d9d9/#ebebeb bg, #171717 fill, 3px radius)
│   └── "Upgrade" button (white bg, #d9d9d9/#e5e5e5 border, 6px radius, py-8)
└── User Profile: Avatar (32px circle, #171717 bg) + Name (Medium 13px) + Email (Regular 11px)
```

---

## 5. 누락/참고 사항

### 5.1 `1:1216` (Empty - No Monitoring) 프레임
- Figma Console 스크린샷 리뷰에서 이 프레임은 직접 캡처되지 않았음
- Overview (No SDK) 프레임 `2:2515`와 유사한 패턴으로, Dashboard에서 Runtime Events 영역이 비어있는(모니터링 없는) 상태를 보여주는 것으로 추정
- 구현 시 Figma Console에서 별도 확인 필요

### 5.2 Setup SDK Node ID 차이
- 첫 번째 리뷰에서 `3:1006`으로 캡처 (독립 컴포넌트 520x621)
- 두 번째 리뷰에서 `3:1042`로 캡처 (overlay 포함 1357x1625)
- 동일 다이얼로그의 두 가지 표현 (컴포넌트 vs 전체 화면 overlay)
- 계획에서는 `3:1042`를 기준으로 사용

### 5.3 Grade D 컬러 불일치
- 일부 소스: `#cc5900` text, `#ffecd9` bg
- 다른 소스: `#cc4d1a` text, `#ffebe0` bg
- 구현 시 Figma에서 정확한 값 재확인 필요 (미세한 차이이므로 `#cc5900`/`#ffecd9` 우선 사용)

### 5.4 리뷰 출처 파일
| 파일 | 내용 | 프레임 수 |
|------|------|-----------|
| `agent-a27caec.md` | Figma Console 스크린샷 — 12개 페이지 + 2개 다이얼로그 | 14 |
| `agent-a331d5e.md` | Figma Console 스크린샷 — 7개 How to Fix + 4개 Runtime + 2개 Account Menu + 1개 Setup SDK | 14 |
| `agent-a38d3e6.md` | claude.ai Figma get_design_context — 7개 화면 디자인 토큰 상세 | 7 |
| `agent-a89fe63.md` | claude.ai Figma get_design_context — 7개 화면 디자인 토큰 상세 | 7 |

---

## 6. 구현 대비 Figma 차이점 (2026-02-23)

프론트엔드 구현 과정에서 Figma 원안과 의도적으로 달라진 부분을 기록합니다.

### 6.1 New Project Dialog (`2:2411`)

**Figma 원안:** 단순 3필드 폼 (Project Name, Project URL, GitHub Repository URL)

**실제 구현:** GitHub OAuth 연동 4-state UI
- `not_connected`: "Connect GitHub" 버튼 표시 + 수동 URL 입력 링크
- `connecting`: 연결 중 스피너 표시
- `connected`: 연결 완료 → 레포 검색/선택 드롭다운 (private 포함 6개 mock repo)
- `manual`: 수동 URL 직접 입력 모드

**사유:** Private repo에 접근하려면 GitHub OAuth가 필요. Figma에는 public URL만 입력하는 단순 폼이었으나, PM 요구사항으로 OAuth 연동 추가.

### 6.2 Profile Settings (`2:2644`)

**Figma 원안:**
- Account Info: Name 입력 필드 항상 표시 + Save 버튼
- Change Password: 3개 입력 필드 항상 표시 + Update Password 버튼

**실제 구현:**
- Account Info: **읽기 모드**(이름 텍스트 + Edit 버튼) → **편집 모드**(입력 필드 + Cancel/Save 버튼)
- Change Password (Email 사용자): **읽기 모드**(•••• 마스킹 + Edit 버튼) → **편집 모드**(3개 입력 + Cancel/Update Password)
- Change Password (OAuth 사용자): Provider 아이콘 + "You signed in with [Provider]." + 외부 Settings 링크 (GitHub/Google)

**사유:** 의도하지 않은 수정 방지를 위한 Edit/Cancel 패턴 적용. OAuth 사용자는 비밀번호가 없으므로 Provider 정보 표시.

### 6.3 How to Fix 모달 Footer (`3:213` ~ `3:554`)

**Figma 원안:**
- Issue 모달: Close + Rescan + Mark as Fixed (3버튼)
- Passed 모달: Close + Rescan (2버튼)

**실제 구현:** 모든 모달에서 **Close 버튼만** 표시

**사유:** MVP 프론트엔드에서 Rescan/Mark as Fixed 기능 미구현. 동작하지 않는 버튼 노출은 사용자 혼란 유발.

### 6.4 다크 모드 지원

**Figma 원안:** Light 테마만 존재

**실제 구현:** `globals.css`에 Light/Dark 모드 CSS Custom Properties (`--gr-*`) 정의. Appearance 카드에서 테마 전환 가능.

**사유:** Profile Settings의 Appearance 카드에 Light/Dark 프리뷰가 있으므로 다크 모드 실제 구현.

### 6.5 랜딩페이지 UX Writing 카피 개선 (2026-02-23)

**Figma 원안 카피** → **실제 구현 카피** (UX Writing 원칙 기반 개선)

AI UX Writer가 `UX Writing Knowledge.md` 원칙(Conversational, User-focused, Clear, Front-loaded)에 따라 랜딩페이지 카피를 개선. 셀링 포인트가 되는 보안 용어(API keys, SSL, brute force 등)는 유지하되, 문장 구조와 동사를 개선하여 사용자 혜택 관점으로 재작성.

| 위치 | Figma 원안 | 구현 (개선) | 변경 사유 |
|------|-----------|------------|----------|
| Feature 카드 3 제목 | MCP Integration | MCP Integration (유지) | 셀링 포인트 용어로 유지 |
| Feature 카드 4 제목 | Dashboard | Security Dashboard | 구체적 맥락 추가 |
| Pre-launch Scan 설명 | Check for...before you go live | Catch...before your users find them | 동사 강화(Catch) + 사용자 혜택 관점 전환, 셀링 포인트 용어 유지 |
| Real-time Guard 설명 | Monitors your live app 24/7 — uptime, SSL... | Monitors uptime, SSL...around the clock | 모니터링 대상을 앞에 배치(Front-loaded), 셀링 포인트 용어 유지 |
| How it works Step 1 | "via MCP" | "from your editor" | 전문 약어 → 사용자 행동 맥락 |
| Free 플랜 | MCP (Claude Code) connection | Claude Code integration | 괄호 설명 필요한 용어 자체가 부적합 |
| Free 플랜 | SDK runtime guard | Live app protection | "SDK", "runtime" 개발자 전문 용어 제거 |
| Pro 플랜 | Same scans & MCP / Same dashboard | Unlimited scans & integrations / Full dashboard & history | "Same..." 반복 제거, 포함 가치 구체적 명시 |

**참조:** `AI-UX-Writer/UX Writing Updates.md` — 상세 Before/After + 변경 근거

### 6.6 전체 화면 UX Writing 카피 개선 — 2차 리뷰 (2026-02-23)

**대상:** 랜딩페이지 제외 전체 화면 (Login, Dashboard Empty States, SDK Setup, Scan Result, Project Settings, Profile Settings, Plan & Billing, New Project Dialog)

AI UX Writer가 `UX Writing Knowledge.md` 원칙(Concise, Clear, Conversational, Distinct, Inclusive Design)에 따라 전체 화면 카피 2차 리뷰 수행.

| 위치 | Figma 원안 | 구현 (개선) | 변경 사유 |
|------|-----------|------------|----------|
| Login 부제 | "Sign in to your account to continue" | "Sign in to your account" | "to continue" 필러 제거 (Concise) |
| Empty: No Projects 제목 | "Create your first project!" | "No projects yet" | 과도한 명령형 → 상태 설명 (Conversational) |
| Empty: No Projects 설명 | "Create a project and start security scanning." | "Add your app to start finding security issues." | 동사 반복 제거 + 혜택 관점 (User-focused) |
| Empty: No Scans 하단 | "or scan directly from Claude Code via MCP" | "or scan directly from Claude Code" | "via MCP" 전문 약어 제거 (Readable) |
| Empty: No Monitoring 설명 | "uptime (is it online?), SSL (security lock)" | "uptime, SSL, and security headers" | 괄호 설명 제거 + 중복 SDK 안내 정리 (Concise) |
| SDK Setup 버튼/제목 | "Setup SDK" | "Set Up SDK" | 동사형 올바른 영어 문법 (Clear) |
| SDK Setup Step 3 | "Or just ask Claude Code" / "just say:" | "Or ask Claude Code" / "say:" | 필러 "just" 제거 (Concise) |
| How to Fix AI 안내 | "Claude Code or your AI assistant" | "your AI coding tool" | 특정 도구 한정 제거 (Inclusive Design) |
| Project Danger Zone | "Irreversible actions — be careful!" | "These actions are permanent and cannot be undone." | 겁주는 톤 → 명확한 결과 설명 (Clear, Compassionate) |
| Profile Delete 버튼 | "Delete" | "Delete Account" | 삭제 대상 명시 (Distinct) |
| Free Plan 기능 | "All features included" | "All core features" | Free/Pro 차별화 (Purposeful) |
| New Project URL 도움말 | "(uptime & SSL)" | 괄호 전문 용어 제거 | 자연어로 충분 (Readable) |

**참조:** `AI-UX-Writer/UX Writing Updates.md` — 상세 Before/After + 변경 근거 (23:00 섹션)
