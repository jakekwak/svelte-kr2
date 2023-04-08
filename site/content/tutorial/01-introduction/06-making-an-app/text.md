---
title: Making an app
---

이 자습서는 구성 요소 작성 프로세스에 익숙해지도록 설계되었습니다. 그러나 언젠가는 자신의 텍스트 편집기에서 편안하게 구성 요소 작성을 시작하고 싶을 것입니다.

먼저 Svelte를 빌드 도구와 통합해야 합니다. [vite-plugin-svelte](https://github.com/sveltejs/vite-plugin-svelte/)로 [Vite](https://vitejs.dev/)를 설정하는 [SvelteKit](https://kit.svelte.dev)을 사용하는 것이 좋습니다...

```bash
npm create svelte@latest myapp
```

There are also a number of [community-maintained integrations](https://sveltesociety.dev/tools).

Don't worry if you're relatively new to web development and haven't used these tools before. We've prepared a simple step-by-step guide, [Svelte for new developers](/blog/svelte-for-new-developers), which walks you through the process.

You'll also want to configure your text editor. There are [plugins](https://sveltesociety.dev/tools#editor-support) for many popular editors as well as an official [VS Code extension](https://marketplace.visualstudio.com/items?itemName=svelte.svelte-vscode).

[커뮤니티에서 유지되는 통합](https://sveltesociety.dev/tools)도 많이 있습니다.

웹 개발에 비교적 익숙하지 않고 이전에 이러한 도구를 사용해 본 적이 없더라도 걱정하지 마십시오. 프로세스를 안내하는 간단한 단계별 가이드인 [신규 개발자를 위한 Svelte](/blog/svelte-for-new-developers)를 준비했습니다.

또한 텍스트 편집기를 구성할 수도 있습니다. 많은 유명 편집자를 위한 [플러그인](https://sveltesociety.dev/tools#editor-support)과 공식 [VS Code 확장 프로그램](https://marketplace.visualstudio.com/items?itemName=svelte.svelte-vscode)이 있습니다.

<!-- 
NOTE: 편집자 설정 가이드를 위한 더 나은 장소가 있을 때까지 제거되었습니다. https://github.com/sveltejs/svelte/pull/7310#issuecomment-1049923609 참조하세요
편집기에 Svelte 플러그인이 없는 경우 [이 가이드](/blog/setting-up-your-editor)에 따라 구문 강조를 위해 `.svelte` 파일을 `.html`과 동일하게 처리하도록 텍스트 편집기를 구성할 수 있습니다. -->

그런 다음 프로젝트 설정이 완료되면 Svelte 구성 요소를 쉽게 사용할 수 있습니다. 컴파일러는 각 구성 요소를 일반 JavaScript 클래스로 변환합니다. 가져오기하고 `new`로 인스턴스화하기만 하면 됩니다:

```js
import App from './App.svelte';

const app = new App({
	target: document.body,
	props: {
		// we'll learn about props later
		answer: 42
	}
});
```

그런 다음 필요한 경우 [구성요소 API](/docs#run-time-client-side-component-api)를 사용하여 `app`과 상호작용할 수 있습니다.
