---
question: Is there a router?
---

공식 라우팅 라이브러리는 [SvelteKit](https://kit.svelte.dev/)입니다. SvelteKit은 파일 시스템 라우터, SSR(서버 측 렌더링) 및 HMR(핫 모듈 재로딩)을 사용하기 쉬운 하나의 패키지로 제공합니다. React의 Next.js와 유사점을 공유합니다.

그러나 원하는 라우터 라이브러리를 사용할 수 있습니다. 많은 사람들이 [page.js](https://github.com/visionmedia/page.js)를 사용합니다. 매우 유사한 [navaid](https://github.com/lukeed/navaid)도 있습니다. 그리고 [universal-router](https://github.com/kriasoft/universal-router)는 하위 경로와 동형이지만 내장 기록 지원은 없습니다.

선언적 HTML 접근 방식을 선호하는 경우 동형 [svelte-routing](https://github.com/EmilTholin/svelte-routing) 라이브러리와 몇 가지 추가 기능이 포함된 [svelte-navigator](https://github.com/mefechoel/svelte-navigator)라는 포크가 있습니다.

클라이언트 측에서 해시 기반 라우팅이 필요한 경우 [svelte-spa-router](https://github.com/ItalyPaleAle/svelte-spa-router) 또는 [abstract-state-router](https://github.com/TehShrike/abstract-state-router/)를 확인하세요.

[Routify](https://routify.dev)는 SvelteKit의 라우터와 유사한 또 다른 파일 시스템 기반 라우터입니다. 버전 3은 Svelte의 기본 SSR을 지원합니다.
