# 진학 플랫폼 허브 구현 계획

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 정현석 선생님의 진학/학습 웹앱들을 하나로 묶는 정적 허브 페이지(`index.html`)를 만들어 https://specialzoker.github.io/sujifather/ 에 배포한다.

**Architecture:** 빌드 도구·프레임워크 없는 순수 HTML/CSS 단일 페이지. 앱 목록은 페이지 상단 `<script>`의 배열 데이터(`APPS`)로 관리하고, JS가 분류별로 그룹지어 DOM을 렌더한다. 새 앱은 배열에 객체 하나만 추가하면 반영된다. 딥그린 아카데믹 톤, 반응형.

**Tech Stack:** HTML5, CSS3(변수/그리드/flex), 바닐라 JS. 외부 의존성 없음. 배포는 GitHub Pages(프로젝트 사이트, 하위 경로).

**작업 폴더:** `C:\Users\user\Documents\claude\sujifather` (이미 `git init` 및 설계 문서 커밋 완료)

**환경 주의:** 이 PC는 node/npm이 PATH에 없음. 이 프로젝트는 node 불필요(정적 파일)라 브라우저로 직접 열어 확인한다. 확인은 Browser pane의 `preview_start {url}` 또는 파일을 직접 열어서 한다.

---

## File Structure

- `index.html` — 허브 페이지 전체. `<head>` 메타/스타일, `<body>`에 헤더·앱 목록 컨테이너·푸터, 하단 `<script>`에 `APPS` 데이터 + 렌더 함수. (단일 파일 유지: 링크 페이지 규모라 분리 불필요, YAGNI)
- `README.md` — 저장소 소개 + 새 앱 추가법(APPS 배열에 항목 추가) 안내.
- `docs/superpowers/specs/2026-07-15-jinhak-platform-design.md` — 설계 문서(이미 존재).

`index.html` 내부 책임 분리:
- **데이터**: `APPS` 배열(각 항목 `{name, desc, url, category, icon}`), `CATEGORY_ORDER` 배열(분류 표시 순서).
- **렌더**: `renderApps()` — 분류 순서대로 섹션 생성, 각 분류 안에 해당 앱 타일을 `<a target="_blank" rel="noopener">`로 출력.
- **스타일**: `<style>` 안 CSS 변수로 딥그린 팔레트 정의.

---

## Task 1: 저장소 뼈대 + 최소 index.html (제목만)

**Files:**
- Create: `C:\Users\user\Documents\claude\sujifather\index.html`

- [ ] **Step 1: 최소 index.html 작성**

`C:\Users\user\Documents\claude\sujifather\index.html`:

```html
<!doctype html>
<html lang="ko">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>진학 플랫폼 · 광명북고 정현석</title>
</head>
<body>
  <header class="site-header">
    <h1>진학 플랫폼</h1>
    <p class="subtitle">광명북고 진로진학부장 정현석</p>
  </header>
  <main id="app-list"></main>
  <footer class="site-footer">
    경기진협 자료개발국 간사 · 광명북고 진로진학부장 정현석
  </footer>
</body>
</html>
```

- [ ] **Step 2: 브라우저로 확인**

Browser pane: `preview_start`에 `{url: "file:///C:/Users/user/Documents/claude/sujifather/index.html"}` 전달(또는 파일 직접 열기).
Expected: "진학 플랫폼" 제목, 부제, 빈 본문, 푸터 텍스트가 보인다(스타일 없음).

- [ ] **Step 3: 커밋**

```bash
cd "C:/Users/user/Documents/claude/sujifather"
git add index.html
git -c user.name="specialzoker" -c user.email="specialzoker@gmail.com" commit -m "feat: 허브 페이지 뼈대(제목·부제·푸터)"
```

---

## Task 2: APPS 데이터 + 분류별 렌더링 (스타일 전)

**Files:**
- Modify: `C:\Users\user\Documents\claude\sujifather\index.html` (`</body>` 앞에 `<script>` 추가)

- [ ] **Step 1: APPS 데이터와 렌더 함수 추가**

`index.html`의 `</body>` 바로 앞에 삽입:

```html
  <script>
    // 새 앱 추가: 아래 배열에 객체 하나만 넣으면 됩니다.
    // category 값은 CATEGORY_ORDER에 있는 이름을 쓰세요(없으면 CATEGORY_ORDER에도 추가).
    const CATEGORY_ORDER = ["대학 찾기·판정", "학습 보조"];

    const APPS = [
      { name: "대입통합검색기", desc: "성적 입력 → 권역·계열·전형별 대학 판정",
        url: "https://specialzoker.github.io/daip-search/", category: "대학 찾기·판정", icon: "🔎" },
      { name: "2027 수시 상담용", desc: "수시 대학 판정 (수시 NAVI)",
        url: "https://specialzoker.github.io/counselting/", category: "대학 찾기·판정", icon: "🎯" },
      { name: "논술 대학 추천기", desc: "수능 성적 기반 논술전형 대학 추천",
        url: "https://specialzoker.github.io/nonsul/", category: "대학 찾기·판정", icon: "✍️" },
      { name: "오답 유사문제 추천", desc: "틀린 문제 입력 → 유사문제 추천",
        url: "https://specialzoker.github.io/wrong/", category: "학습 보조", icon: "📚" },
    ];

    function renderApps() {
      const root = document.getElementById("app-list");
      root.innerHTML = "";
      for (const category of CATEGORY_ORDER) {
        const apps = APPS.filter(a => a.category === category);
        if (apps.length === 0) continue;
        const section = document.createElement("section");
        section.className = "cat-section";
        const h = document.createElement("h2");
        h.className = "cat-title";
        h.textContent = category;
        section.appendChild(h);
        for (const app of apps) {
          const a = document.createElement("a");
          a.className = "app-tile";
          a.href = app.url;
          a.target = "_blank";
          a.rel = "noopener noreferrer";
          a.innerHTML =
            '<span class="app-icon" aria-hidden="true">' + app.icon + '</span>' +
            '<span class="app-text"><span class="app-name"></span>' +
            '<span class="app-desc"></span></span>';
          a.querySelector(".app-name").textContent = app.name;
          a.querySelector(".app-desc").textContent = app.desc;
          section.appendChild(a);
        }
        root.appendChild(section);
      }
    }
    renderApps();
  </script>
```

- [ ] **Step 2: 브라우저로 확인**

페이지 새로고침(navigate 또는 reload).
Expected: "대학 찾기·판정" 아래 3개(대입통합검색기/2027 수시 상담용/논술 대학 추천기), "학습 보조" 아래 1개(오답 유사문제 추천)가 아이콘·이름·설명과 함께 링크로 뜬다. 클릭 시 각 앱이 새 탭으로 열린다(read_page로 href·target 확인).

- [ ] **Step 3: 커밋**

```bash
cd "C:/Users/user/Documents/claude/sujifather"
git add index.html
git -c user.name="specialzoker" -c user.email="specialzoker@gmail.com" commit -m "feat: APPS 데이터 기반 분류별 앱 목록 렌더링"
```

---

## Task 3: 딥그린 아카데믹 스타일 + 반응형

**Files:**
- Modify: `C:\Users\user\Documents\claude\sujifather\index.html` (`<head>`에 `<style>` 추가)

- [ ] **Step 1: `<style>` 블록을 `</head>` 앞에 추가**

```html
  <style>
    :root{
      --green:#24503a; --green-mid:#2f6b45; --green-soft:#3d7a54;
      --cream:#f5f6ee; --card:#fffdf7; --line:#e2e5d3;
      --ink:#24503a; --muted:#7a8a72;
      --shadow:0 1px 2px rgba(36,80,58,.08);
      --radius:10px;
    }
    *{box-sizing:border-box;}
    body{
      margin:0; background:var(--cream); color:var(--ink);
      font-family:system-ui,-apple-system,"Segoe UI","Malgun Gothic",sans-serif;
      line-height:1.5;
    }
    .site-header{ text-align:center; padding:40px 20px 8px; }
    .site-header h1{ margin:0; font-size:1.9rem; font-weight:800; color:var(--green); letter-spacing:-.01em; }
    .site-header .subtitle{ margin:6px 0 0; color:var(--muted); font-size:.95rem; }
    main{ max-width:640px; margin:0 auto; padding:20px 16px 8px; }
    .cat-section{ margin-bottom:22px; }
    .cat-title{
      font-size:.8rem; font-weight:700; text-transform:uppercase; letter-spacing:.06em;
      color:var(--green-soft); margin:0 0 10px; padding-bottom:5px;
      border-bottom:1px solid var(--line);
    }
    .app-tile{
      display:flex; align-items:center; gap:14px;
      background:var(--card); border:1px solid var(--line); border-radius:var(--radius);
      padding:14px 16px; margin-bottom:10px; text-decoration:none; color:inherit;
      box-shadow:var(--shadow); transition:transform .12s ease, box-shadow .12s ease, border-color .12s ease;
    }
    .app-tile:hover{ transform:translateY(-2px); box-shadow:0 6px 16px rgba(36,80,58,.14); border-color:var(--green-soft); }
    .app-icon{
      flex:0 0 auto; width:44px; height:44px; border-radius:10px;
      background:var(--green-mid); color:#fff;
      display:flex; align-items:center; justify-content:center; font-size:1.3rem;
    }
    .app-text{ display:flex; flex-direction:column; min-width:0; }
    .app-name{ font-weight:700; font-size:1.02rem; color:var(--green); }
    .app-desc{ font-size:.86rem; color:var(--muted); }
    .site-footer{ text-align:center; color:var(--muted); font-size:.8rem; padding:24px 16px 40px; }
    @media (max-width:480px){
      .site-header{ padding-top:28px; }
      .site-header h1{ font-size:1.6rem; }
      .app-tile{ padding:12px 13px; gap:11px; }
      .app-icon{ width:38px; height:38px; font-size:1.15rem; }
    }
  </style>
```

- [ ] **Step 2: 브라우저로 확인 (데스크톱)**

새로고침. Expected: 크림 배경, 초록 제목, 크림빛 카드에 초록 아이콘 블록, hover 시 살짝 떠오름. 분류 소제목이 초록 밑줄과 함께 보인다.

- [ ] **Step 3: 브라우저로 확인 (모바일)**

`resize_window`로 폭 375px(mobile). Expected: 카드가 가로로 안 넘치고, 아이콘·글자가 작아지며 한 화면에 자연스럽게 들어온다. 가로 스크롤 없음.

- [ ] **Step 4: 스크린샷으로 증거 남기고 커밋**

```bash
cd "C:/Users/user/Documents/claude/sujifather"
git add index.html
git -c user.name="specialzoker" -c user.email="specialzoker@gmail.com" commit -m "style: 딥그린 아카데믹 테마 + 반응형"
```

---

## Task 4: README + 새 앱 추가 안내

**Files:**
- Create: `C:\Users\user\Documents\claude\sujifather\README.md`

- [ ] **Step 1: README 작성**

```markdown
# 진학 플랫폼 (sujifather)

광명북고 진로진학부장 정현석의 진학/학습 웹앱 허브.

- 배포: https://specialzoker.github.io/sujifather/
- 단일 정적 페이지(`index.html`). 빌드 없음.

## 새 앱 추가하기

`index.html` 하단 `<script>`의 `APPS` 배열에 항목 하나를 추가하면 됩니다:

    { name: "앱 이름", desc: "한 줄 설명",
      url: "https://specialzoker.github.io/<앱경로>/",
      category: "대학 찾기·판정", icon: "🔎" }

- `category`는 `CATEGORY_ORDER`에 있는 이름을 사용하세요. 새 분류가 필요하면 `CATEGORY_ORDER` 배열에도 이름을 추가합니다(표시 순서 = 배열 순서).
- `icon`은 이모지 하나.

저장 후 `index.html`을 브라우저로 열어 확인하고 커밋·푸시하면 GitHub Pages에 반영됩니다.
```

- [ ] **Step 2: 커밋**

```bash
cd "C:/Users/user/Documents/claude/sujifather"
git add README.md
git -c user.name="specialzoker" -c user.email="specialzoker@gmail.com" commit -m "docs: README + 새 앱 추가 안내"
```

---

## Task 5: GitHub 저장소 생성 + 배포 (사용자 확인 필요)

> 이 태스크는 외부에 공개 게시하는 단계다. 실행 전 사용자에게 명시적으로 확인받고 진행한다.

**Files:** 없음(원격 작업)

- [ ] **Step 1: 원격 저장소 생성 + 푸시 (사용자 승인 후)**

`gh`가 인증돼 있으면:

```bash
cd "C:/Users/user/Documents/claude/sujifather"
gh repo create sujifather --public --source=. --remote=origin --push
```

또는 수동: GitHub에서 `sujifather` public 저장소 생성 후

```bash
git branch -M main
git remote add origin https://github.com/specialzoker/sujifather.git
git push -u origin main
```

- [ ] **Step 2: GitHub Pages 켜기 (사용자 조작)**

사용자에게 안내: 저장소 **Settings → Pages → Build and deployment → Source: "Deploy from a branch" → Branch: `main` / `/ (root)` → Save.**
몇 분 뒤 https://specialzoker.github.io/sujifather/ 접속.

- [ ] **Step 3: 배포 확인**

Browser pane으로 https://specialzoker.github.io/sujifather/ 열기.
Expected: 허브가 뜨고 네 앱 링크가 새 탭으로 정상 이동.

---

## Self-Review 결과

- **스펙 커버리지:** 호스팅/주소(Task 1·5), 헤더(Task 1), 분류별 세로 리스트(Task 2), 딥그린 톤·반응형(Task 3), 확장성=APPS 데이터+README(Task 2·4), 새 탭 링크(Task 2), 푸터(Task 1) — 모두 태스크로 매핑됨.
- **플레이스홀더:** 모든 코드 스텝에 실제 코드 포함. TBD 없음.
- **타입/이름 일관성:** `APPS` 항목 필드(name/desc/url/category/icon), `CATEGORY_ORDER`, `renderApps()`, `#app-list`, 클래스명(`app-tile`/`app-icon`/`app-name`/`app-desc`/`cat-title`/`cat-section`)이 Task 2·3·4에서 일치.
