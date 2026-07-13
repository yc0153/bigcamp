# GitHub Pages · Supabase · Vercel · Codex 사용 총정리

이 문서는 Day 1 아이디어 보드를 만들고 공개 배포한 실제 과정을 다시 따라 할 수 있도록 정리한 가이드다.

## 1. 세 도구의 역할

| 도구 | 역할 | 이번 프로젝트에서 한 일 |
| --- | --- | --- |
| GitHub | 코드 저장소·버전 관리 | `yc0153/bigcamp`에 `main` 브랜치 저장 |
| GitHub Pages | 정적 웹사이트 공개 | `index.html`을 무료 URL로 공개 |
| Supabase | 데이터베이스·인증 | `ideas` 테이블과 익명 로그인을 구성 |
| Vercel | GitHub 연동 배포 | `main`에 push할 때 자동 재배포 |
| Codex | 코드 작성·터미널·브라우저 작업 보조 | 코드 수정, 설정 확인, 배포 검증 |

구조는 다음과 같다.

```text
사용자 브라우저
   ├─ GitHub Pages 또는 Vercel에서 index.html 다운로드
   └─ Supabase API에 익명 로그인 후 ideas 데이터 조회·등록

GitHub 저장소
   ├─ GitHub Pages 배포 원본
   └─ Vercel 자동 배포 원본
```

GitHub Pages와 Vercel은 웹사이트를 보여주는 역할이고, Supabase는 아이디어를 저장하는 역할이다. GitHub는 주로 코드를 보관하는 장소다.

## 2. 이번 프로젝트에서 사용한 값

다음 값은 이 프로젝트를 다시 찾을 때 사용한다.

| 항목 | 값 |
| --- | --- |
| 로컬 프로젝트 | `C:\Users\praisy\Desktop\빅데이터캠프\camp_AI` |
| GitHub 저장소 | `https://github.com/yc0153/bigcamp` |
| 기본 브랜치 | `main` |
| GitHub Pages | `https://yc0153.github.io/bigcamp/` |
| Vercel | `https://bigcamp.vercel.app/` |
| Supabase 프로젝트 | `yc0153's Project` |
| Supabase project ref | `fcyrxbdbalbsqsfbohfm` |
| Supabase URL | `https://fcyrxbdbalbsqsfbohfm.supabase.co` |
| Supabase 테이블 | `public.ideas` |

## 3. 시작 전에 준비할 것

### 계정

- GitHub 계정
- Supabase 계정
- Vercel 계정
- GitHub와 Vercel을 연결할 때 사용할 GitHub 로그인 상태

### 프로젝트

- 배포할 프로젝트 폴더
- 실행할 HTML 파일. 이번 프로젝트는 루트의 `index.html`이다.
- Git 저장소와 `main` 브랜치
- 커밋할 수 있는 변경사항

### 미리 정할 정보

- GitHub 저장소 이름
- 기본 브랜치 이름
- Vercel 프로젝트 이름
- Supabase 테이블 이름과 저장할 컬럼
- 공개 URL을 어떤 것으로 사용할지

### 절대 제공하지 않을 정보

Codex나 프론트엔드 코드에 다음 정보를 넣으면 안 된다.

- GitHub·Supabase·Vercel 비밀번호
- OTP, 인증 코드, 개인 액세스 토큰
- Supabase `secret key` 또는 `service_role key`
- OpenAI API 비밀 키

Supabase의 `publishable key`는 RLS와 접근 정책을 켠 상태에서 브라우저에 사용할 수 있다. 그래도 실제 키는 이 문서나 공개 README에 기록하지 않는다.

## 4. Codex에게 처음 요청할 프롬프트

프로젝트 폴더를 Codex에서 연 뒤 다음처럼 요청한다.

```text
현재 작업 폴더의 Day 1 강의자료와 코드 파일을 모두 확인해줘.
강의에서 요구하는 산출물과 현재 코드 상태를 비교하고,
먼저 구현해야 할 최소 MVP와 필요한 파일을 정리해줘.
파일을 수정하기 전에는 현재 Git 상태와 기존 변경사항을 보존하고,
내가 제공해야 하는 로그인·프로젝트·API 정보가 생기면 그 시점에 알려줘.
```

Codex가 코드를 만들거나 수정할 때 사용할 공통 조건은 다음과 같다.

```text
사용자에게 보이는 문구는 한국어로 작성해줘.
기존 파일과 기존 사용자 변경사항을 덮어쓰지 말고,
작은 단위로 수정한 뒤 diff 확인, 로컬 실행, 브라우저 검증까지 해줘.
비밀 키나 service_role 키는 프론트엔드와 Git 커밋에 넣지 마.
```

## 5. GitHub 저장소 준비 및 GitHub Pages 배포

### 5-1. 저장소가 없는 경우

1. [GitHub](https://github.com/)에 로그인한다.
2. 우측 상단 `+`를 누른다.
3. `New repository`를 선택한다.
4. 저장소 이름을 정한다.
5. 공개 저장소가 필요하면 `Public`을 선택한다.
6. `Add a README file`이나 초기화 옵션은 선택하지 않는다. 이미 로컬 프로젝트가 있기 때문이다.
7. `Create repository`를 누른다.

GitHub 로그인과 저장소 생성은 사용자가 직접 처리한다. 완료 후 Codex에 다음 프롬프트를 넣는다.

```text
GitHub 저장소를 만들었어.
원격 저장소 주소는 https://github.com/<GitHub아이디>/<저장소이름>.git 이고,
현재 프로젝트를 main 브랜치로 연결할 수 있는지 먼저 확인해줘.
기존 커밋과 변경사항을 보존하면서 필요한 git 명령을 실행하고,
push 전에 실행할 명령과 결과를 알려줘.
```

### 5-2. 저장소가 이미 있는 경우

이번 프로젝트처럼 이미 저장소가 있으면 새 저장소를 만들 필요가 없다.

```text
현재 프로젝트의 Git 원격 저장소가 https://github.com/yc0153/bigcamp.git인지 확인해줘.
main 브랜치와 원격 상태를 확인하고, 변경사항을 push하기 전에
git status, git diff --check, 최신 커밋을 점검해줘.
```

### 5-3. GitHub Pages 켜기

1. GitHub 저장소 화면에서 `Settings`를 누른다.
2. 왼쪽 메뉴에서 `Pages`를 누른다.
3. `Build and deployment`의 `Source`를 `Deploy from a branch`로 선택한다.
4. Branch에서 `main`을 선택한다.
5. 폴더는 `/(root)`를 선택한다.
6. `Save`를 누른다.
7. 몇 분 기다린 뒤 Pages URL을 연다.

설정 화면을 연 뒤 Codex에 넣을 프롬프트:

```text
GitHub 저장소 Settings > Pages 화면을 열었어.
이 프로젝트는 루트 index.html 하나로 동작하는 정적 사이트야.
현재 화면에서 main 브랜치와 /(root)를 선택해야 하는지 확인하고,
내가 눌러야 할 버튼을 순서대로 알려줘.
저장 후 공개 URL이 생기면 실제 페이지를 열어 index.html이 보이는지 검증해줘.
```

저장 후의 확인 프롬프트:

```text
GitHub Pages 설정을 저장했어.
https://<GitHub아이디>.github.io/<저장소이름>/ 을 열어서
페이지 로딩, 아이디어 등록, 새로고침 후 유지, 랜덤 피치 버튼을 확인해줘.
문제가 있으면 Pages 설정·배포 상태·브라우저 오류를 나눠서 진단해줘.
```

### 5-4. GitHub Pages의 한계

- 정적 HTML, CSS, JavaScript 배포에 적합하다.
- 브라우저에서 실행되는 JavaScript는 사용할 수 있다.
- 서버 비밀 키를 안전하게 보관하는 서버 기능은 제공하지 않는다.
- OpenAI API 비밀 키를 `index.html`에 넣으면 안 된다.
- 새 커밋이 Pages에 반영되기까지 시간이 걸릴 수 있다.

## 6. Supabase 프로젝트·테이블·인증 설정

### 6-1. 로그인 및 프로젝트 선택

1. [Supabase](https://supabase.com/)에 접속한다.
2. `Continue with GitHub`로 로그인한다.
3. 조직을 선택한다.
4. 기존 프로젝트가 있으면 먼저 재사용할지 확인한다.
5. 새 프로젝트를 만들 경우 프로젝트 이름, 조직, 데이터베이스 비밀번호, 리전을 설정한다.

데이터베이스 비밀번호는 사용자가 직접 정하고 보관한다. Codex에게 전달하지 않는다.

이번 프로젝트에서는 새 프로젝트를 만들지 않고 `yc0153's Project`를 재사용했다.

Codex에 넣을 프롬프트:

```text
Supabase에 로그인했어.
기존 프로젝트를 먼저 확인하고, 새 프로젝트를 만들지 재사용할지 판단해줘.
프로젝트 이름, project ref, 프로젝트 URL, 상태(Healthy 여부)를 확인하되
데이터베이스 비밀번호나 secret key는 읽거나 출력하지 마.
```

### 6-2. SQL Editor에서 테이블과 RLS 만들기

Supabase Dashboard에서 `SQL Editor`를 열고 새 쿼리를 만든다. 아래 쿼리는 이번 Day 1 아이디어 보드에 사용한 구조다.

```sql
create table if not exists public.ideas (
  id uuid primary key default gen_random_uuid(),
  title text not null,
  problem text not null,
  wow text not null,
  created_at timestamptz not null default now()
);

alter table public.ideas enable row level security;

grant usage on schema public to anon, authenticated;
grant select on table public.ideas to anon, authenticated;
grant insert on table public.ideas to authenticated;

create policy "ideas_select_authenticated"
on public.ideas
for select
to authenticated
using (true);

create policy "ideas_insert_authenticated"
on public.ideas
for insert
to authenticated
with check (true);
```

실행 순서:

1. 위 SQL 전체를 붙여 넣는다.
2. `Run`을 누른다.
3. `Success. No rows returned`가 표시되는지 확인한다.
4. Table Editor에서 `public.ideas`가 생겼는지 확인한다.

주의:

- 이미 같은 정책을 만든 뒤 같은 SQL을 다시 실행하면 중복 정책 오류가 날 수 있다.
- 처음부터 `drop policy`를 넣지 않는다. 기존 데이터를 지울 수 있는 작업은 먼저 확인해야 한다.
- 오류가 나면 정책 목록을 확인한 뒤 없는 부분만 추가한다.

SQL Editor를 열었을 때 Codex에 넣을 프롬프트:

```text
Supabase SQL Editor 화면을 열었어.
public.ideas 테이블을 만들고 RLS를 켜야 해.
기존 테이블이나 정책이 있는지 먼저 확인하고,
기존 데이터를 삭제하는 drop 문 없이 안전한 SQL을 제안해줘.
실행 결과가 성공인지 확인한 뒤 Table Editor에서도 확인해줘.
```

### 6-3. Anonymous Sign-ins 켜기

이번 앱은 회원가입 화면 없이 익명 사용자를 authenticated 역할로 만들기 위해 익명 로그인을 사용했다.

1. Supabase Dashboard에서 `Authentication` 또는 `Auth`를 연다.
2. `Providers`를 선택한다.
3. `Anonymous Sign-ins`를 찾는다.
4. 스위치를 켠다.
5. `Save changes`를 누른다.

Supabase가 CAPTCHA를 권장할 수 있다. 학습용 Day 1에서는 우선 익명 로그인을 켜고, 실제 서비스에서는 CAPTCHA·남용 방지·데이터 삭제 정책을 추가한다.

```text
Supabase Auth Providers 화면을 열었어.
Anonymous Sign-ins가 켜져 있는지 확인하고,
꺼져 있으면 내가 직접 스위치를 켤 수 있도록 정확한 위치와 Save changes 순서를 알려줘.
회원가입·비밀번호·secret key 설정은 건드리지 마.
```

### 6-4. API URL과 publishable key 확인

Supabase Dashboard에서 `Project Settings > API Keys`로 이동한다.

확인할 값:

- Project URL: `https://<project-ref>.supabase.co`
- Publishable key: 보통 `sb_publishable_`로 시작하는 공개용 키

프론트엔드에는 Project URL과 publishable key만 사용한다. `secret key`, `service_role key`는 절대 사용하지 않는다.

```text
Supabase API Keys 화면을 열었어.
Project URL과 publishable key가 무엇인지 확인해줘.
secret key나 service_role key는 읽거나 출력하지 말고,
프론트엔드에 넣어도 되는 값과 넣으면 안 되는 값을 구분해줘.
```

### 6-5. Codex에 Supabase 연결을 요청하는 프롬프트

Project URL과 publishable key를 확인한 뒤, 실제 키를 공개 문서에 넣지 않고 현재 Codex 작업에 필요한 범위에서만 전달한다.

```text
Supabase 설정을 완료했어.
Project URL은 https://<project-ref>.supabase.co 이고,
테이블은 public.ideas야.

현재 index.html의 localStorage 저장을 Supabase 우선 저장으로 바꿔줘.
1. 익명 로그인(signInAnonymously)
2. ideas 조회와 created_at 내림차순 정렬
3. 새 아이디어 insert
4. Supabase 연결 실패 시 localStorage fallback
5. 화면에 연결 상태 표시
6. 기존 localStorage 사용자 카드가 있으면 클라우드 데이터가 비어 있을 때 한 번만 이전

publishable key만 사용하고 secret/service_role key는 절대 사용하지 마.
수정 후 diff 확인과 로컬 브라우저 검증까지 해줘.
```

### 6-6. Supabase 연결 검증

정상 연결이면 화면에 다음과 비슷한 문구가 표시된다.

```text
Supabase에 저장 중 · 모든 기기에서 공유됩니다.
```

검증 순서:

1. 앱을 연다.
2. Supabase 연결 상태가 성공인지 확인한다.
3. 아이디어를 하나 등록한다.
4. Supabase Table Editor에서 `public.ideas` 행이 생겼는지 확인한다.
5. 다른 브라우저나 다른 배포 URL을 열어 같은 카드가 보이는지 확인한다.
6. `오늘의 피치 뽑기`를 눌러 랜덤 선택을 확인한다.

```text
현재 앱과 Supabase 연결을 검증해줘.
연결 상태 문구, ideas 행 조회, 새 카드 등록, 새로고침 유지,
다른 공개 URL에서 같은 데이터가 보이는지 차례대로 확인해줘.
오류가 나면 Auth, RLS 정책, 테이블 이름, Project URL, publishable key를
각각 따로 점검하고 secret key를 요구하지 마.
```

## 7. Vercel 가입·GitHub 연동·배포

### 7-1. 처음 가입하기

1. [Vercel](https://vercel.com/)에 접속한다.
2. `Sign Up`을 누른다.
3. `Continue with GitHub`를 선택한다.
4. GitHub 권한 승인 화면이 나오면 저장소 접근 범위를 확인한다.
5. Vercel 팀을 만든다. 이번에는 Hobby 플랜 팀을 사용했다.

사용자가 로그인·권한 승인을 직접 완료한 뒤 Codex에 다음과 같이 말한다.

```text
Vercel 가입과 GitHub 로그인을 완료했어.
현재 Vercel 화면에서 GitHub 저장소를 연결하는 다음 단계와
내가 눌러야 할 버튼을 순서대로 알려줘.
비밀번호나 API 토큰은 입력하지 말고, 화면 상태만 확인해줘.
```

### 7-2. GitHub App 설치

1. Vercel에서 `Add New...` 또는 `New Project`를 선택한다.
2. GitHub 저장소를 가져오는 화면에서 `Install` 또는 GitHub App 연결 버튼을 누른다.
3. GitHub 권한 화면에서 사용할 저장소를 선택한다.
4. 저장소 범위를 가능하면 필요한 저장소로 제한한다.
5. `Install`을 완료하고 Vercel로 돌아온다.

```text
Vercel에서 GitHub App Install 화면이 보여.
yc0153/bigcamp 저장소를 Vercel이 가져올 수 있도록
필요한 저장소만 선택하는 방법과 Install 후 다음 화면을 알려줘.
```

### 7-3. 프로젝트 가져오기

이번 프로젝트에서 사용한 설정은 다음과 같다.

| Vercel 항목 | 설정 |
| --- | --- |
| Repository | `yc0153/bigcamp` |
| Branch | `main` |
| Project Name | `bigcamp` |
| Framework Preset | `Other` |
| Root Directory | `./` 또는 저장소 루트 |
| Build Command | 비워 둠 |
| Output Directory | 비워 둠 또는 기본값 |
| Install Command | 정적 HTML이므로 별도 설치 없음 |
| Environment Variables | 현재는 없음 |

클릭 순서:

1. 저장소 옆 `Import`를 누른다.
2. `Project Name`을 `bigcamp`로 확인한다.
3. Framework Preset을 `Other`로 둔다.
4. Root Directory가 저장소 루트인지 확인한다.
5. Build Command와 Output Directory를 비워 둔다.
6. 현재 앱은 Supabase publishable key를 정적 `index.html`에서 사용하므로 Vercel 환경변수는 비워 둔다.
7. `Deploy`를 누른다.

```text
Vercel Import 화면에서 yc0153/bigcamp를 선택했어.
이 프로젝트는 루트 index.html 하나로 동작하는 정적 사이트야.
Project Name은 bigcamp, Framework Preset은 Other,
Root Directory는 저장소 루트, Build Command와 Output Directory는 비워 두고
Environment Variables도 현재는 비워 두는 것이 맞는지 확인해줘.
설정이 맞으면 Deploy 직전 최종 점검만 해줘.
```

### 7-4. 배포 완료 확인

1. 배포 화면에서 `Ready` 또는 성공 상태를 확인한다.
2. `Visit` 또는 생성된 도메인을 누른다.
3. 앱의 Supabase 연결 상태가 성공인지 확인한다.
4. 카드 등록·새로고침·랜덤 피치를 확인한다.

```text
Vercel 배포가 완료됐어.
https://bigcamp.vercel.app/을 열어서
페이지 로딩, Supabase 공유 상태, 카드 조회, 카드 등록,
새로고침 유지, 랜덤 피치 버튼을 검증해줘.
최신 Git 커밋이 반영된 것도 확인해줘.
```

### 7-5. 이후 업데이트 방법

Vercel은 GitHub 저장소와 연결되어 있으므로 다음 흐름으로 업데이트한다.

```text
코드 수정 → git diff 확인 → git commit → git push origin main
→ Vercel 자동 배포 → 공개 URL 확인
```

업데이트 요청 프롬프트:

```text
변경사항을 검토하고 git diff --check를 실행해줘.
문제가 없으면 적절한 feat/fix/docs 커밋을 만들고 main에 push해줘.
push 후 Vercel 공개 URL에서 최신 변경사항과 기존 기능을 직접 확인해줘.
```

### 7-6. Vercel 환경변수가 필요한 경우

현재 Day 1 정적 앱에는 Vercel 환경변수가 필요하지 않다.

다음 단계에서 OpenAI API 서버 기능을 추가할 때는 달라진다.

- OpenAI API 키는 Vercel `Settings > Environment Variables`에 저장한다.
- 변수 이름은 예를 들어 `OPENAI_API_KEY`로 둔다.
- `Production`, `Preview`, `Development` 적용 범위를 확인한다.
- 서버 함수에서만 읽는다.
- `index.html`이나 브라우저 코드에서 읽지 않는다.
- Supabase `service_role` 키도 브라우저가 아닌 서버에서만 사용해야 한다.

## 8. 전체 과정을 한 번에 요청하는 Codex 프롬프트

새 프로젝트에서 비슷한 작업을 다시 할 때 사용할 수 있는 통합 프롬프트다. 로그인과 권한 승인 화면은 사용자가 직접 처리한다.

```text
현재 프로젝트를 Day 1 정적 웹앱으로 배포해줘.

목표:
- GitHub 저장소 main 브랜치에 코드 보관
- GitHub Pages로 정적 사이트 공개
- Supabase public.ideas 테이블에 아이디어 공유 저장
- Vercel에서 GitHub main 브랜치 자동 배포

작업 순서:
1. 현재 파일·Git 상태·실행 방법을 먼저 점검한다.
2. 기존 변경사항을 보존한다.
3. Supabase는 publishable key와 RLS만 사용하고 secret/service_role key는 사용하지 않는다.
4. 익명 로그인, ideas 조회·등록, localStorage fallback을 구현한다.
5. GitHub Pages와 Vercel 설정에서 내가 눌러야 할 위치를 단계별로 안내한다.
6. 로그인·권한 승인·비밀번호 입력은 내가 직접 처리한다.
7. 커밋 전 diff --check와 로컬 브라우저 검증을 한다.
8. push 후 GitHub Pages와 Vercel 공개 URL을 각각 열어 최신 버전을 검증한다.
9. 마지막에 변경 파일, 커밋 hash, 공개 URL, 남은 작업을 정리한다.
```

## 9. 문제 발생 시 Codex 프롬프트

### GitHub Pages가 이전 화면일 때

```text
GitHub Pages 공개 URL이 이전 화면을 보여줘.
현재 저장소 main의 최신 커밋, Settings > Pages의 Source/Branch/Folder,
Pages 배포 상태와 캐시 가능성을 순서대로 확인해줘.
삭제나 강제 reset은 하지 말고, 필요한 조치만 알려줘.
```

### Vercel이 이전 커밋일 때

```text
Vercel 공개 URL이 최신 변경사항을 보여주지 않아.
GitHub main 최신 커밋과 Vercel Deployments의 배포 커밋을 비교하고,
자동 배포 실패인지 캐시인지 확인해줘.
필요하면 Vercel에서 Redeploy할 위치를 안내하고 공개 URL을 다시 검증해줘.
```

### Supabase 연결 실패일 때

```text
앱에 Supabase 연결 실패 또는 localStorage fallback 문구가 보여.
secret key를 요구하지 말고 다음을 순서대로 점검해줘.
1. Supabase 프로젝트 URL
2. publishable key 형식
3. Anonymous Sign-ins 활성화
4. public.ideas 테이블 존재 여부
5. RLS 활성화 여부
6. authenticated 대상 select/insert 정책
7. 브라우저 콘솔과 네트워크 오류
원인과 수정 방법을 구분해서 알려줘.
```

### SQL 정책 중복 오류일 때

```text
Supabase SQL 실행에서 policy already exists 오류가 났어.
drop 문으로 바로 삭제하지 말고 pg_policies에서 현재 정책을 조회한 뒤,
이미 올바른 정책이면 재실행하지 않아도 되는지 확인해줘.
부족한 정책만 추가하는 안전한 SQL을 제안해줘.
```

## 10. 최종 점검표

### 코드·Git

- [ ] `index.html`이 저장소 루트에 있다.
- [ ] `git diff --check`가 통과한다.
- [ ] `git status`에 의도하지 않은 변경이 없다.
- [ ] 최신 커밋이 `main`에 push되어 있다.
- [ ] 프론트엔드에 secret/service_role/OpenAI 비밀 키가 없다.

### GitHub Pages

- [ ] Pages Source가 `Deploy from a branch`다.
- [ ] Branch가 `main`이다.
- [ ] Folder가 `/(root)`다.
- [ ] 공개 URL에서 화면이 열린다.

### Supabase

- [ ] `public.ideas` 테이블이 있다.
- [ ] RLS가 켜져 있다.
- [ ] Anonymous Sign-ins가 켜져 있다.
- [ ] authenticated 사용자의 select/insert 정책이 있다.
- [ ] 카드 등록 후 Table Editor에 행이 생긴다.

### Vercel

- [ ] GitHub 저장소가 연결되어 있다.
- [ ] 프로젝트가 `main` 브랜치를 배포한다.
- [ ] Framework Preset이 정적 앱에 맞게 `Other`다.
- [ ] 배포 상태가 `Ready`다.
- [ ] push 후 자동 재배포된다.

### 사용자 기능

- [ ] Supabase 성공 상태가 표시된다.
- [ ] 아이디어 등록이 된다.
- [ ] 새로고침 후 카드가 유지된다.
- [ ] 다른 브라우저·다른 공개 URL에서도 같은 카드가 보인다.
- [ ] 오늘의 피치 뽑기가 작동한다.

## 11. 이번 작업의 완료 결과

- GitHub 저장소: `https://github.com/yc0153/bigcamp`
- GitHub Pages: `https://yc0153.github.io/bigcamp/`
- Vercel: `https://bigcamp.vercel.app/`
- Supabase 테이블: `public.ideas`
- 최신 연결 커밋: `ac43061 feat: connect ideas to supabase`
