---
title: Getting started
---

---

대화형 온라인 환경에서 Svelte를 사용해 보려면 [the REPL](https://svelte.dev/repl) 또는 [StackBlitz](https://node.new/svelte)를 사용해 보세요.

로컬에서 프로젝트를 만들려면 Svelte 팀의 공식 애플리케이션 프레임워크인 [SvelteKit](https://kit.svelte.dev/)를 사용하는 것이 좋습니다.
```
npm create svelte@latest myapp
cd myapp
npm install
npm run dev
```

SvelteKit은 [Svelte 컴파일러](https://www.npmjs.com/package/svelte) 호출을 처리하여 `.svelte` 파일을 DOM 및 `.css` 파일을 생성하는 `.js` 파일로 변환합니다. 그것. 또한 개발 서버, 라우팅 및 배포와 같은 웹 애플리케이션을 구축하는 데 필요한 다른 모든 부분을 제공합니다. [SvelteKit](https://kit.svelte.dev/)는 [Vite](https://vitejs.dev/)를 활용하여 코드를 빌드하고 서버 측 렌더링(SSR)을 처리합니다. Svelte 컴파일을 처리하기 위한 [모든 주요 웹 번들러용 플러그인](https://sveltesociety.dev/tools#bundling)이 있습니다. 그러면 HTML에 삽입할 수 있는 `.js` 및 `.css`가 출력되지만 대부분의 다른 사람들은 SSR을 처리하지 않습니다.

완전한 앱 프레임워크가 필요하지 않고 간단한 프런트엔드 전용 사이트/앱을 구축하려는 경우 `npm init vite`를 실행하고 `svelte`를 선택하여 Vite와 함께 Svelte(키트 없음)를 사용할 수도 있습니다. 옵션. 이를 통해 `npm run build`는 `dist` 디렉토리 내에 HTML, JS 및 CSS 파일을 생성합니다.

Svelte 팀은 [VS Code 확장](https://marketplace.visualstudio.com/items?itemName=svelte.svelte-vscode)을 유지하며 다양한 다른 [편집기](https://sveltesociety.dev/tools#editor-support) 및 도구와의 통합도 있습니다.

문제가 있는 경우 [Discord](https://svelte.dev/chat) 또는 [StackOverflow](https://stackoverflow.com/questions/tagged/svelte)에서 도움을 받으세요.