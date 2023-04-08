---
title: If blocks
---

HTML에는 조건문 및 루프와 같은 *논리*를 표현하는 방법이 없습니다. Svelte가 그렇습니다.

일부 마크업을 조건부로 렌더링하기 위해 `if` 블록으로 래핑합니다.

```html
{#if user.loggedIn}
	<button on:click={toggle}>
		Log out
	</button>
{/if}

{#if !user.loggedIn}
	<button on:click={toggle}>
		Log in
	</button>
{/if}
```

사용해 보십시오 - 구성 요소를 업데이트하고 버튼을 클릭하십시오.