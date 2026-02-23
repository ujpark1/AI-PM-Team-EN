# Learning — 시행착오 기록

프로젝트 진행 중 발생한 실수와 교훈을 기록합니다.

---

## #1. Figma 프레임 전체 목록 미확인 (2026-02-23)

**실수:** Figma 파일에 28개 프레임이 있는데, 14개 페이지 프레임만 스크린샷 리뷰하고 "전부 확인했다"고 보고함. 나머지 14개(How to Fix 모달 7종, Runtime 모달 4종, Account Menu 2개, Setup SDK 다이얼로그 1개)를 빠뜨림.

**원인:** `figma_execute`로 전체 프레임 목록을 먼저 조회하지 않고, 스펙 문서에 언급된 페이지 프레임만 확인함.

**교훈:**
- 구현 시작 전 반드시 `figma.currentPage.children`으로 전체 프레임 수와 목록을 먼저 파악할 것
- 빠뜨린 것이 없는지 숫자로 교차 검증할 것 (스펙 문서 프레임 수 vs Figma 실제 프레임 수)
- "전부 확인했다"고 보고하기 전에 실제로 전부인지 재확인할 것

---

## #2. 앞선 14개 프레임 리뷰 결과 저장 위치 불명확 (2026-02-23)

**실수:** 첫 14개 페이지 프레임의 Figma Console 스크린샷 리뷰를 에이전트에게 위임했는데, 결과가 에이전트 내부 컨텍스트에만 존재하고 별도 파일로 저장하지 않음. 에이전트가 자체적으로 임시 파일(`tender-coalescing-treehouse-agent-*.md`)에 기록했지만, 이는 `.claude/plans/` 내부의 에이전트 전용 파일이라 관리가 불투명함.

**교훈:**
- 에이전트에게 리서치를 위임할 때, 결과물의 저장 위치를 명확히 지정할 것
- 중요한 리뷰 결과는 프로젝트 내 관리 가능한 위치에 저장할 것

---

## #3. DB 스키마 컬럼명과 코드 불일치 (2026-02-23)

**실수:** DB 마이그레이션에서 `profiles` 테이블의 컬럼을 `name`으로 정의했는데, 프론트엔드 코드(`user-provider.tsx`, `account-info.tsx`, `sidebar-user-footer.tsx`)에서는 `full_name`으로 참조. Supabase Auth의 `user_metadata.full_name`과 profiles 테이블의 `name` 컬럼이 혼동됨.

**원인:** Supabase Auth의 `raw_user_meta_data` → `full_name` 키와 profiles 테이블의 `name` 컬럼명이 달랐는데, 코드 작성 시 Auth 메타데이터 키와 DB 컬럼명을 구분하지 않고 동일하게 `full_name`으로 사용함.

**교훈:**
- DB 스키마 작성 후 코드에서 참조할 때, 마이그레이션 SQL의 정확한 컬럼명을 확인할 것
- Supabase Auth 메타데이터 필드명(`user_metadata.full_name`)과 자체 테이블 컬럼명(`profiles.name`)은 별개 — 혼동 주의
- 새 Provider/API 작성 시 Supabase select/update 쿼리의 컬럼명을 스키마와 교차 검증할 것
