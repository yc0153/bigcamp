# Team Rules

## 개발 원칙

- 사용자에게 보이는 문구는 한국어로 작성한다.
- 첫 배포는 의존성 없는 정적 HTML로 끝낸다.
- 핵심 기능은 2개만 먼저 완성한다. 범위가 늘어나면 백로그로 옮긴다.
- `index.html`은 반드시 소문자 파일명으로 유지한다.
- 비밀 키·토큰·비밀번호를 프론트엔드 코드나 Git 커밋에 넣지 않는다.
- Supabase publishable key는 RLS와 정책을 함께 켠 경우에만 브라우저에 사용한다. service_role·secret key는 절대 공개하지 않는다.
- Day 2 서버 함수에서만 `process.env.OPENAI_API_KEY`를 읽는다.

## 협업 규칙

- 한 번에 한 가지 작은 변경만 한다.
- 커밋 메시지는 `feat:`, `fix:`, `docs:`, `chore:` 접두어를 사용한다.
- 커밋 전 `git diff`와 로컬 브라우저 확인을 한다.
- `main`에 push하면 배포가 갱신된다는 전제로, push 후 공개 URL을 직접 확인한다.

## 로컬 확인

```powershell
python -m http.server 4173
```

브라우저에서 `http://localhost:4173`을 열고 아이디어 등록, 새로고침 보존, 랜덤 피치를 확인한다.
