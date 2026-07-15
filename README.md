# 진학 플랫폼 (sujifather)

광명북고 진로진학부장 정현석의 진학/학습 웹앱 허브.

- 배포: https://specialzoker.github.io/sujifather/
- 단일 정적 페이지(`index.html`). 빌드 없음.

## 접근 제한 (로그인 게이트)

허가된 이메일만 들어올 수 있습니다. 통합검색기와 **같은 Firebase 명단**(`search-alluniv` 프로젝트의 `allowed_emails`)을 공유하므로, 명단 추가·접속 신청 승인은 **통합검색기 관리자 화면**에서 하면 허브에도 그대로 적용됩니다. 같은 도메인이라 한쪽에 로그인하면 다른 쪽도 자동 통과합니다.

※ 이 게이트는 허브 "현관문"입니다. 나머지 앱(counselting·nonsul·wrong)은 각자의 주소로 직접 접근하면 열리므로, 필요하면 각 앱에도 같은 게이트를 넣어야 합니다.

## 새 앱 추가하기

`index.html` 하단 `<script>`의 `APPS` 배열에 항목 하나를 추가하면 됩니다:

    { name: "앱 이름", desc: "한 줄 설명",
      url: "https://specialzoker.github.io/<앱경로>/",
      category: "대학 찾기·판정", icon: "🔎" }

- `category`는 `CATEGORY_ORDER`에 있는 이름을 사용하세요. 새 분류가 필요하면 `CATEGORY_ORDER` 배열에도 이름을 추가합니다(표시 순서 = 배열 순서).
- `icon`은 이모지 하나.

저장 후 `index.html`을 브라우저로 열어 확인하고 커밋·푸시하면 GitHub Pages에 반영됩니다.
