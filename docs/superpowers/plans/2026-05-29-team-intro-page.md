# 대웅찬넷 팀 소개 페이지 Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 단일 정적 `index.html` 하나로 (주)대웅찬넷 팀 정체성·팀원·강점·아이디어 ①②를 소개하는 반응형 한 페이지를 만든다.

**Architecture:** 빌드 도구·프레임워크·외부 의존성 없이 `index.html` 1개에 HTML + 내장 `<style>`로 작성한다. 상단 sticky 네비 + 5개 `<section>` 앵커 스크롤. 미니멀 라이트 톤(흰 바탕/검정 포인트). 섹션을 하나씩 `<main>`에 덧붙이며 점진적으로 완성한다.

**검증 방식(중요):** 정적 브로슈어 페이지라 단위 테스트 러너가 없다(무빌드 제약). 따라서 각 태스크는 "코드 작성 → 브라우저에서 눈으로 확인 + 콘텐츠 존재 grep 확인 → 커밋" 사이클로 진행한다. 자바스크립트 테스트 프레임워크를 새로 들이지 않는다(YAGNI, 스펙의 무빌드 결정 준수).

**Tech Stack:** HTML5, CSS3 (내장 `<style>`, CSS 변수·Grid·Flexbox·미디어쿼리), 시스템 폰트 스택. JS 없음(스크롤은 CSS `scroll-behavior`).

**Spec:** `docs/superpowers/specs/2026-05-29-team-page-design.md`

---

## File Structure

```
ASMteamPage/
  index.html      # 전체 페이지 (HTML + 내장 <style>) — 이번 작업의 산출물
  .gitignore      # .superpowers/, .omc/ 제외
  team.md         # 원본 정보 (기존)
  docs/superpowers/specs/2026-05-29-team-page-design.md  # 설계 (기존)
```

- 단일 `index.html` 유지. CSS는 `<style>` 안에 둔다.
- 섹션 추가 규칙: 각 섹션 HTML은 **`</main>` 바로 앞에** 순서대로(Hero→팀원→강점→아이디어) 덧붙인다. CSS 블록은 **`</style>` 바로 앞에** 덧붙인다.

---

## Task 1: 프로젝트 스캐폴드 (git + index.html 뼈대 + 네비)

**Files:**
- Create: `ASMteamPage/.gitignore`
- Create: `ASMteamPage/index.html`

- [ ] **Step 1: git 저장소 초기화**

Run:
```bash
cd /Users/7bellaa/ASMteamPage && git init
```
Expected: `Initialized empty Git repository ...`

- [ ] **Step 2: `.gitignore` 작성**

Create `ASMteamPage/.gitignore`:
```
.superpowers/
.omc/
.DS_Store
```

- [ ] **Step 3: `index.html` 뼈대 작성 (base CSS 토큰 + sticky 네비 + 빈 main/footer)**

Create `ASMteamPage/index.html`:
```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>대웅찬넷 · 팀 소개</title>
  <style>
    :root{
      --bg:#ffffff; --ink:#111111; --muted:#555555; --muted2:#888888;
      --line:#e5e7eb; --soft:#f7f7fb; --tag-bg:#eef0ff; --tag-ink:#3344aa;
      --maxw:960px; --radius:14px;
    }
    *{box-sizing:border-box;}
    html{scroll-behavior:smooth;}
    body{
      margin:0; background:var(--bg); color:var(--ink);
      font-family:-apple-system,BlinkMacSystemFont,"Segoe UI",Roboto,"Apple SD Gothic Neo","Noto Sans KR",sans-serif;
      line-height:1.6; -webkit-font-smoothing:antialiased;
    }
    a{color:inherit; text-decoration:none;}
    .container{max-width:var(--maxw); margin:0 auto; padding:0 20px;}
    section{padding:72px 0; scroll-margin-top:64px;}
    .section-label{font-size:12px; font-weight:700; letter-spacing:1.5px; color:var(--muted2); text-transform:uppercase;}
    .section-title{font-size:28px; font-weight:800; margin:6px 0 0;}
    .section-lead{font-size:16px; color:var(--muted); margin:14px 0 0;}
    /* nav */
    .nav{position:sticky; top:0; z-index:10; background:rgba(255,255,255,.9); backdrop-filter:saturate(180%) blur(10px); border-bottom:1px solid var(--line);}
    .nav .container{display:flex; align-items:center; justify-content:space-between; height:60px;}
    .nav .brand{font-weight:800;}
    .nav .links{display:flex; gap:22px; font-size:14px; color:var(--muted);}
    .nav .links a:hover{color:var(--ink);}
    @media(max-width:720px){
      .nav .links{gap:14px; font-size:13px;}
      section{padding:52px 0;}
      .section-title{font-size:23px;}
    }
    /* ===== component styles (태스크별로 이 위에 추가) ===== */
  </style>
</head>
<body>
  <header class="nav">
    <div class="container">
      <a class="brand" href="#home">대웅찬넷</a>
      <nav class="links">
        <a href="#team">팀원</a>
        <a href="#strengths">강점</a>
        <a href="#ideas">아이디어</a>
      </nav>
    </div>
  </header>

  <main>
    <!-- 섹션은 이 main 안, </main> 바로 앞에 순서대로 추가 -->
  </main>

  <footer id="footer" class="footer">
    <!-- Task 6에서 내용 채움 -->
  </footer>
</body>
</html>
```

- [ ] **Step 4: 브라우저로 열어 확인**

Run:
```bash
open /Users/7bellaa/ASMteamPage/index.html
```
Expected (눈으로 확인):
- 상단에 흰색 sticky 바, 왼쪽 "대웅찬넷", 오른쪽 "팀원 · 강점 · 아이디어" 링크
- 본문은 아직 비어 있음, 콘솔 에러 없음

- [ ] **Step 5: 커밋**

```bash
cd /Users/7bellaa/ASMteamPage && git add .gitignore index.html team.md docs/superpowers/specs/2026-05-29-team-page-design.md && git commit -m "feat: scaffold team intro page (nav + base styles)"
```

---

## Task 2: Hero 섹션 (팀 정체성)

**Files:**
- Modify: `ASMteamPage/index.html`

- [ ] **Step 1: Hero CSS 추가**

`/* ===== component styles ... ===== */` 주석 줄 바로 아래(즉 `</style>` 앞)에 추가:
```css
    .hero{padding-top:90px; padding-bottom:80px;}
    .hero-eyebrow{font-size:13px; font-weight:700; letter-spacing:1px; color:var(--tag-ink);}
    .hero-title{font-size:46px; font-weight:900; line-height:1.15; margin:14px 0 0; letter-spacing:-.5px;}
    .hero-slogan{font-size:17px; color:var(--muted); margin:18px 0 0; max-width:620px;}
    .hero-chips{display:flex; gap:10px; flex-wrap:wrap; margin-top:26px;}
    .chip{font-size:13px; font-weight:600; background:var(--soft); border:1px solid var(--line); padding:7px 14px; border-radius:999px;}
    @media(max-width:720px){ .hero-title{font-size:32px;} .hero-slogan{font-size:15px;} }
```

- [ ] **Step 2: Hero HTML 추가**

`<main>` 안, `</main>` 바로 앞에 추가:
```html
    <section id="home" class="hero">
      <div class="container">
        <div class="hero-eyebrow">(주)대웅찬넷 · DAEUNGCHANNET INC.</div>
        <h1 class="hero-title">IT 서비스를 만드는<br>부산대학교 학생 창업팀</h1>
        <p class="hero-slogan">함께 오랜 시간을 보낸 친구들이 모여, 빠르게 움직이고 끝까지 완주합니다.</p>
        <div class="hero-chips">
          <span class="chip">IT · Software</span>
          <span class="chip">Student Startup</span>
          <span class="chip">Problem Solving</span>
        </div>
      </div>
    </section>
```

- [ ] **Step 3: 콘텐츠 존재 확인 (grep)**

Run:
```bash
grep -c "부산대학교 학생 창업팀" /Users/7bellaa/ASMteamPage/index.html
```
Expected: `1` 이상

- [ ] **Step 4: 브라우저 새로고침 후 확인**

Expected (눈으로): 큰 제목 "IT 서비스를 만드는 / 부산대학교 학생 창업팀", 그 아래 슬로건, 칩 3개(IT · Software / Student Startup / Problem Solving)

- [ ] **Step 5: 커밋**

```bash
cd /Users/7bellaa/ASMteamPage && git add index.html && git commit -m "feat: add hero section (team identity + slogan)"
```

---

## Task 3: 팀원 섹션 (카드 3개)

**Files:**
- Modify: `ASMteamPage/index.html`

- [ ] **Step 1: 팀원 카드 CSS 추가** (`</style>` 앞)

```css
    .common-info{font-size:13px; color:var(--muted2); margin:6px 0 0;}
    .member-grid{display:grid; grid-template-columns:repeat(3,1fr); gap:18px; margin-top:28px;}
    .member-card{background:#fff; border:1px solid var(--line); border-radius:var(--radius); overflow:hidden;}
    .member-accent{height:6px; background:var(--ink);}
    .member-body{padding:20px;}
    .member-name{font-size:19px; font-weight:800; margin:0; display:inline-block;}
    .role-badge{display:inline-block; margin-left:8px; font-size:11px; font-weight:700; padding:3px 10px; border-radius:999px; background:var(--soft); color:var(--muted); vertical-align:middle;}
    .role-badge.lead{background:var(--ink); color:#fff;}
    .member-bio{font-size:14px; color:var(--muted); margin:12px 0 0;}
    .member-mil{font-size:12px; color:var(--muted2); margin:8px 0 0;}
    .tags{display:flex; gap:6px; flex-wrap:wrap; margin-top:14px;}
    .tag{font-size:12px; font-weight:600; background:var(--tag-bg); color:var(--tag-ink); padding:4px 10px; border-radius:7px;}
    @media(max-width:720px){ .member-grid{grid-template-columns:1fr;} }
```

- [ ] **Step 2: 팀원 섹션 HTML 추가** (`</main>` 앞, Hero 다음)

```html
    <section id="team">
      <div class="container">
        <div class="section-label">Members</div>
        <h2 class="section-title">팀원 소개</h2>
        <p class="section-lead">4년지기 친구 3명, 필요한 건 뭐든 배웁니다.</p>
        <p class="common-info">부산대학교 3학년 · 2003년생 · 군필</p>
        <div class="member-grid">
          <article class="member-card">
            <div class="member-accent"></div>
            <div class="member-body">
              <h3 class="member-name">정선웅</h3><span class="role-badge lead">팀장</span>
              <p class="member-bio">알고리즘과 수학을 깊게 파왔습니다. 개발은 이번이 첫 도전.</p>
              <p class="member-mil">공군 5비 항공전자장비정비병</p>
              <div class="tags"><span class="tag">C++</span><span class="tag">Python</span></div>
            </div>
          </article>
          <article class="member-card">
            <div class="member-accent"></div>
            <div class="member-body">
              <h3 class="member-name">한대희</h3><span class="role-badge">팀원</span>
              <p class="member-bio">React 프론트엔드 개발 경험(해커톤·인턴). 부산대 PLATO 캘린더 크롬 확장 운영 중.</p>
              <p class="member-mil">공군 5비 공병</p>
              <div class="tags"><span class="tag">JavaScript</span><span class="tag">C++</span></div>
            </div>
          </article>
          <article class="member-card">
            <div class="member-accent"></div>
            <div class="member-body">
              <h3 class="member-name">정의찬</h3><span class="role-badge">팀원</span>
              <p class="member-bio">피지컬 AI와 데이터 엔지니어링에 관심이 많습니다. 개발은 새로운 도전.</p>
              <p class="member-mil">육군 국직(유사공군)</p>
              <div class="tags"><span class="tag">Python</span></div>
            </div>
          </article>
        </div>
      </div>
    </section>
```

- [ ] **Step 3: 콘텐츠 확인 (grep)**

Run:
```bash
grep -o -E "정선웅|한대희|정의찬" /Users/7bellaa/ASMteamPage/index.html | sort -u | wc -l
```
Expected: `3`

- [ ] **Step 4: 브라우저 확인**

Expected (눈으로): 카드 3개 가로 배치(정선웅·한대희·정의찬 순), 각 카드 상단에 검정 액센트 바, 이름 옆 역할 배지(정선웅=검정 "팀장"), 소개·군 정보·기술 태그. 아바타/이미지 없음. 네비 "팀원" 클릭 시 이 섹션으로 스크롤.

- [ ] **Step 5: 커밋**

```bash
cd /Users/7bellaa/ASMteamPage && git add index.html && git commit -m "feat: add team members section (3 cards)"
```

---

## Task 4: 팀 강점 섹션 (6개 그리드)

**Files:**
- Modify: `ASMteamPage/index.html`

- [ ] **Step 1: 강점 CSS 추가** (`</style>` 앞)

```css
    .strength-grid{display:grid; grid-template-columns:repeat(3,1fr); gap:16px; margin-top:28px;}
    .strength-card{background:#fff; border:1px solid var(--line); border-radius:12px; padding:20px;}
    .strength-num{font-size:13px; font-weight:800; color:var(--tag-ink);}
    .strength-card h3{font-size:16px; font-weight:800; margin:6px 0 0;}
    .strength-card p{font-size:13px; color:var(--muted); margin:8px 0 0;}
    @media(max-width:760px){ .strength-grid{grid-template-columns:repeat(2,1fr);} }
    @media(max-width:480px){ .strength-grid{grid-template-columns:1fr;} }
```

- [ ] **Step 2: 강점 섹션 HTML 추가** (`</main>` 앞, 팀원 다음)

```html
    <section id="strengths">
      <div class="container">
        <div class="section-label">Strengths</div>
        <h2 class="section-title">팀 강점</h2>
        <div class="strength-grid">
          <div class="strength-card"><span class="strength-num">01</span><h3>오픈마인드</h3><p>모든 분야의 가능성을 열어두고 탐색합니다.</p></div>
          <div class="strength-card"><span class="strength-num">02</span><h3>PS를 사랑함</h3><p>문제를 푸는 행위 자체를 즐깁니다.</p></div>
          <div class="strength-card"><span class="strength-num">03</span><h3>검증된 호흡</h3><p>수년간 알고 지낸 사이 → 적응기 제로, 바로 시작.</p></div>
          <div class="strength-card"><span class="strength-num">04</span><h3>빠른 역할 분담</h3><p>서로의 성격·실력·관심을 이미 파악.</p></div>
          <div class="strength-card"><span class="strength-num">05</span><h3>팀 폭파 가능성 0%</h3><p>관계가 단단해 갈등으로 무너지지 않습니다.</p></div>
          <div class="strength-card"><span class="strength-num">06</span><h3>자동 일정 조율</h3><p>같은 학교·과·학번 → 시간표가 자연스레 맞춰짐.</p></div>
        </div>
      </div>
    </section>
```

- [ ] **Step 3: 콘텐츠 확인 (grep)**

Run:
```bash
grep -c '<div class="strength-card">' /Users/7bellaa/ASMteamPage/index.html
```
Expected: `6` (HTML 카드만 카운트; CSS 규칙 `.strength-card{`는 제외됨)

- [ ] **Step 4: 브라우저 확인**

Expected (눈으로): 강점 6개가 데스크탑 3열×2행 그리드. 각 카드 번호(01~06, 파란색)·제목·설명. 창을 좁히면 2열→1열로 재배치. 네비 "강점" 클릭 시 스크롤.

- [ ] **Step 5: 커밋**

```bash
cd /Users/7bellaa/ASMteamPage && git add index.html && git commit -m "feat: add team strengths section (6-item grid)"
```

---

## Task 5: 아이디어 섹션 (① AI 캘린더 메인 + ② 화면 분할 보조)

**Files:**
- Modify: `ASMteamPage/index.html`

- [ ] **Step 1: 아이디어 CSS 추가** (`</style>` 앞)

```css
    .idea-main{margin-top:28px;}
    .idea-tag{font-size:14px; font-weight:800; color:var(--ink); margin-bottom:12px;}
    .idea-tag.muted{color:var(--muted2);}
    .problem-box{background:#fff; border:1px solid var(--line); border-radius:var(--radius); padding:22px;}
    .ps-label{font-size:11px; font-weight:700; letter-spacing:1.5px; color:var(--muted2);}
    .ps-label.sol{color:#7c9cff;}
    .ps-title{font-size:19px; font-weight:800; margin:6px 0 0;}
    .prob-list{list-style:none; padding:0; margin:14px 0 0; display:flex; flex-direction:column; gap:10px; font-size:14px; color:var(--muted);}
    .prob-list.small{font-size:13px; gap:7px; list-style:disc; padding-left:18px; color:var(--muted);}
    .bridge{background:var(--soft); border:1px dashed #d5d5e0; border-radius:12px; padding:16px 18px; margin:14px 0; font-size:14px; color:#444; line-height:1.6;}
    .solution-box{background:var(--ink); color:#fff; border-radius:var(--radius); padding:24px;}
    .sol-title{font-size:21px; font-weight:800; margin:6px 0 0;}
    .sol-desc{font-size:14px; color:#bbb; margin:10px 0 0;}
    .feat-tags{display:flex; gap:9px; flex-wrap:wrap; margin-top:18px;}
    .feat{font-size:13px; background:#1c1c28; border:1px solid #2c2c3c; padding:7px 12px; border-radius:9px;}
    .idea-sub{margin-top:30px; background:#fff; border:1px solid var(--line); border-radius:var(--radius); padding:22px;}
    .sub-title{font-size:17px; font-weight:800; margin:0;}
    .sub-desc{font-size:14px; color:var(--muted); margin:6px 0 0;}
    .sub-cols{display:flex; gap:16px; margin-top:16px; flex-wrap:wrap;}
    .sub-col{flex:1; min-width:220px;}
    .sub-col.soft{background:var(--soft); border-radius:10px; padding:14px;}
    .sub-sol{font-size:13px; color:#444; margin:8px 0 0; line-height:1.6;}
    @media(max-width:720px){ .sol-title{font-size:18px;} .ps-title{font-size:17px;} }
```

- [ ] **Step 2: 아이디어 섹션 HTML 추가** (`</main>` 앞, 강점 다음 — main의 마지막 섹션)

```html
    <section id="ideas">
      <div class="container">
        <div class="section-label">Ideas</div>
        <h2 class="section-title">우리가 풀고 싶은 문제</h2>
        <p class="section-lead">지금 가장 무게를 두고 있는 두 가지 아이디어입니다.</p>

        <div class="idea-main">
          <div class="idea-tag">아이디어 ① · AI 내장 캘린더</div>
          <div class="problem-box">
            <div class="ps-label">THE PROBLEM</div>
            <h3 class="ps-title">기존 캘린더, 너무 불편하다</h3>
            <ul class="prob-list">
              <li>📱 폰·데탑·맥북·애플워치·아이패드 — 기기 간 동기화가 제각각</li>
              <li>📝 일정 하나 등록하는 데 날짜·시간·태그·위치·반복·알림… 입력이 너무 많음</li>
              <li>📅 기간에 걸친 일정이나 중간에 끊기는 일정은 여러 번 등록해야 함</li>
            </ul>
          </div>
          <div class="bridge">💡 구글 캘린더 + AI 에이전트 조합이면 가능은 하지만, <strong>일반 사용자에게는 그 세팅 자체가 진입장벽</strong>입니다. 그래서 AI를 처음부터 내장한 캘린더를 만듭니다.</div>
          <div class="solution-box">
            <div class="ps-label sol">OUR SOLUTION</div>
            <h3 class="sol-title">자연어 딸깍, 스크린샷 딸깍 — AI가 알아서 잡아주는 캘린더</h3>
            <p class="sol-desc">"내일 3시에 회의" 한 줄, 혹은 포스터 스크린샷 한 장이면 AI 에이전트가 일정을 알아서 등록·정리합니다.</p>
            <div class="feat-tags">
              <span class="feat">🗣️ 자연어 입력</span>
              <span class="feat">📸 스크린샷 입력</span>
              <span class="feat">✅ 캘린더 + 할 일 통합</span>
              <span class="feat">🔄 멀티 디바이스 동기화</span>
              <span class="feat">📆 멀티데이·비연속 일정 지원</span>
            </div>
          </div>
        </div>

        <div class="idea-sub">
          <div class="idea-tag muted">아이디어 ② · 화면 분할 프리셋</div>
          <h3 class="sub-title">단축키 한 번에 '작업 모드'가 펼쳐지는 윈도우 매니저</h3>
          <p class="sub-desc">매번 창 위치를 잡는 그 30초를, 단축키 한 번으로.</p>
          <div class="sub-cols">
            <div class="sub-col">
              <div class="ps-label">PROBLEM</div>
              <ul class="prob-list small">
                <li>작업 시작마다 같은 창 배치를 반복</li>
                <li>패턴인데도 매번 수작업</li>
                <li>기존 매니저는 '배치'만, '앱 실행+배치'는 약함</li>
              </ul>
            </div>
            <div class="sub-col soft">
              <div class="ps-label sol">SOLUTION</div>
              <p class="sub-sol">프리셋 + 단축키 = 한 방에 세팅<br>· <strong>CP 프리셋</strong>: Codeforces + VS Code<br>· <strong>메이플 프리셋</strong>: 메인 게임 + 서브 모니터 유튜브</p>
            </div>
          </div>
        </div>
      </div>
    </section>
```

- [ ] **Step 3: 콘텐츠 확인 (grep)**

Run:
```bash
grep -o -E "아이디어 ① · AI 내장 캘린더|아이디어 ② · 화면 분할 프리셋|진입장벽" /Users/7bellaa/ASMteamPage/index.html | wc -l
```
Expected: `3`

- [ ] **Step 4: 브라우저 확인**

Expected (눈으로): 아이디어 ① — 흰 문제 박스(불편 3가지) → 점선 다리 문구(진입장벽) → 검정 해결 박스(특징 태그 5개). 그 아래 아이디어 ② 보조 카드(문제 목록 + 옅은 회색 해결 박스, CP/메이플 프리셋 예시). 네비 "아이디어" 클릭 시 스크롤.

- [ ] **Step 5: 커밋**

```bash
cd /Users/7bellaa/ASMteamPage && git add index.html && git commit -m "feat: add ideas section (AI calendar + window preset)"
```

---

## Task 6: 푸터 + 전체 반응형/앵커 최종 검증

**Files:**
- Modify: `ASMteamPage/index.html`

- [ ] **Step 1: 푸터 CSS 추가** (`</style>` 앞)

```css
    .footer{border-top:1px solid var(--line); padding:36px 0; margin-top:20px;}
    .footer-brand{font-weight:800;}
    .footer-meta{font-size:13px; color:var(--muted2); margin-top:4px;}
```

- [ ] **Step 2: 푸터 HTML 채우기**

`index.html`의 빈 `<footer id="footer" class="footer">` 블록을 아래로 교체:
```html
  <footer id="footer" class="footer">
    <div class="container">
      <div class="footer-brand">(주)대웅찬넷</div>
      <div class="footer-meta">부산대학교 학생 창업팀 · 2026</div>
    </div>
  </footer>
```

- [ ] **Step 3: 로컬 서버로 띄워 최종 확인**

Run:
```bash
cd /Users/7bellaa/ASMteamPage && python3 -m http.server 8000
```
브라우저에서 `http://localhost:8000` 열고 확인 (확인 후 Ctrl+C로 서버 종료):
- 5개 영역이 순서대로 보인다: Hero → 팀원 → 강점 → 아이디어 → 푸터
- 네비 링크(팀원/강점/아이디어) 클릭 시 해당 섹션으로 부드럽게 스크롤되고, sticky 네비에 제목이 가리지 않는다(`scroll-margin-top` 적용 확인)
- 창 너비를 ~375px로 줄였을 때: 팀원 카드 1열, 강점 그리드 1열, 아이디어 ② 2단이 1단으로, 네비가 깨지지 않음
- 콘솔 에러 없음

- [ ] **Step 4: 콘텐츠 누락 일괄 확인 (grep)**

Run (괄호가 정규식으로 해석되지 않도록 고정 문자열 `grep -F`로 각각 확인):
```bash
cd /Users/7bellaa/ASMteamPage && for s in "(주)대웅찬넷" "팀원 소개" "팀 강점" "우리가 풀고 싶은 문제"; do grep -qF "$s" index.html && echo "OK: $s" || echo "MISSING: $s"; done
```
Expected: 4줄 모두 `OK:` (MISSING 없음)

- [ ] **Step 5: 커밋**

```bash
cd /Users/7bellaa/ASMteamPage && git add index.html && git commit -m "feat: add footer and finalize responsive layout"
```

---

## 완료 기준 (스펙 수용 기준 대응)

- [ ] `index.html`을 열면 5개 섹션(Hero/팀원/강점/아이디어/푸터)이 보인다 → Task 1~6
- [ ] Hero에 팀명·"부산대 학생 창업팀"·슬로건 표시 → Task 2
- [ ] 네비 팀원/강점/아이디어 앵커 스크롤 동작 → Task 1(네비)+3/4/5(앵커 id)+6(검증)
- [ ] 팀원 3명(PPT 순서) 정확 표시, 아바타 없음 → Task 3
- [ ] 팀 강점 6개 그리드 → Task 4
- [ ] 아이디어 ①(문제3→다리→해결) 메인 + ②(화면 분할) 보조 → Task 5
- [ ] 데스크탑/모바일 레이아웃 안 깨짐 → Task 6
- [ ] 오프라인(외부 네트워크 없이) 렌더링 → 외부 의존성 0 (전 태스크)
