# Updates — 주요 업데이트 기록

PM Director가 관리하는 주요 업데이트 이력입니다.
모든 중요한 의사결정, 구현 변경, 스펙 수정 사항을 기록합니다.

---

## 2026-02-23

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
