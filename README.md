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
