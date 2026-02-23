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
| `/login` | `login/page.tsx` | Login |
| `/signup` | `signup/page.tsx` | Sign Up |
| `/dashboard` | `(dashboard)/dashboard/page.tsx` | Overview |
| `/project/[id]/scan/[scanId]` | 해당 `page.tsx` | Scan Result |
| `/project/[id]/scans` | 해당 `page.tsx` | Scans History |
| `/project/[id]/settings` | 해당 `page.tsx` | Project Settings |
| `/settings/profile` | 해당 `page.tsx` | Profile Settings |
| `/settings/billing` | 해당 `page.tsx` | Plan & Billing |
| `(dashboard)` | `layout.tsx` | 대시보드 공통 레이아웃 |

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
| `guardrail-implementation-plan.md` | 2026-02-23 | 구현 현황 + 차이점 섹션 추가 |
| `figma-review-28frames.md` | 2026-02-23 | 섹션 6 (구현 대비 차이점) 추가 |
| `guardrail-feature-spec.md` (root) | 2026-02-23 | GitHub OAuth + Profile OAuth 정책 포함 |
