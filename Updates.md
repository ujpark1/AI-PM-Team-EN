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
