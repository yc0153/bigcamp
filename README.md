# 캠프 아이디어 보드 - Day 1 리허설

## 산출물

- `team_prd.md`: 문제·타깃·핵심 기능·성공 기준·Wow 포인트
- `speckit_inputs.md`: PRD를 Spec Kit/Codex 입력으로 연결하는 초안
- `AGENTS.md`: 팀 개발 규칙과 검증 방법
- `index.html`: 아이디어 카드 등록, Supabase 공유 저장(실패 시 `localStorage` fallback), 오늘의 피치 뽑기 MVP
- [`deployment_guide.md`](deployment_guide.md): GitHub Pages·Supabase·Vercel 준비물, 설정 순서, Codex 복사 프롬프트

## 로컬 실행

```powershell
python -m http.server 4173
```

브라우저에서 `http://localhost:4173`을 열고 다음을 확인한다.

1. Supabase 연결 상태가 표시되는지 확인
2. 아이디어 카드 등록
3. 새로고침 후 카드 유지
4. `오늘의 피치 뽑기` 버튼 동작

## Supabase 연결

- 프로젝트: `yc0153's Project`
- 테이블: `public.ideas`
- 인증: 익명 로그인
- 저장 방식: Supabase에 공유 저장하고, 연결에 실패하면 해당 브라우저의 `localStorage`에 임시 저장한다.
- 보안: `ideas` 테이블에 RLS를 켜고 익명 사용자의 조회·등록 정책만 허용한다. Supabase secret/service_role key는 사용하지 않는다.

## Day 1 배포 체크리스트

### GitHub

1. GitHub에 로그인한다.
2. 우측 상단 `+` → `New repository`를 클릭한다.
3. Repository name에 `camp-ai-rehearsal`을 입력한다.
4. 공개 레포가 필요하면 `Public`을 선택한다.
5. `Initialize this repository` 옵션은 선택하지 않고 `Create repository`를 클릭한다.
6. 생성된 레포의 `Code` → `HTTPS` 주소를 복사한다.
7. 로컬에서 원격 저장소를 연결하고 `main`을 push한다.

```powershell
git remote add origin https://github.com/<아이디>/camp-ai-rehearsal.git
git push -u origin main
```

### Vercel

1. Vercel에 로그인한다.
2. Dashboard에서 `Add New...` → `Project`를 클릭한다.
3. GitHub 저장소 목록에서 `camp-ai-rehearsal`의 `Import`를 클릭한다.
4. Framework Preset은 `Other` 또는 자동 감지 값을 유지한다.
5. Build Command는 비워 두고, Output Directory는 프로젝트 루트로 둔다.
6. `Deploy`를 클릭한다.
7. 발급된 URL을 휴대폰과 노트북에서 열어 등록·새로고침·랜덤 피치를 확인한다.

Day 2에는 새 프로젝트나 새 URL을 만들지 않고, 같은 레포의 `main`에 push해 같은 Vercel URL을 갱신한다.
