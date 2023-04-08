---
title: Else blocks
---

두 조건(`if user.logged In` 및 `if !user.logged In`)은 상호 배타적이므로 `else` 블록을 사용하여 이 구성 요소를 약간 단순화할 수 있습니다.

```html
{#if user.loggedIn}
	<button on:click={toggle}>
		Log out
	</button>
{:else}
	<button on:click={toggle}>
		Log in
	</button>
{/if}
```

> `#` 문자는 항상 *블록 열기* 태그를 나타냅니다. `/` 문자는 항상 *블록 닫기* 태그를 나타냅니다. `{:else}`와 같은 `:` 문자는 *블록 연속* 태그를 나타냅니다. 걱정하지 마십시오. 이미 Svelte가 HTML에 추가하는 거의 모든 구문을 배웠습니다.