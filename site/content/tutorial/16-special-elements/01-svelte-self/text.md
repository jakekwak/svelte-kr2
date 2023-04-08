---
title: <svelte:self>
---

Svelte는 다양한 내장 요소를 제공합니다. 첫 번째 `<svelte:self>`는 컴포넌트가 재귀적으로 자신을 포함하도록 허용합니다.

폴더가 *기타* 폴더를 포함할 수 있는 이 폴더 트리 보기와 같은 항목에 유용합니다. `Folder.svelte`에서 우리는 이것을 할 수 있기를 원합니다...

```html
{#if file.files}
	<Folder {...file}/>
{:else}
	<File {...file}/>
{/if}
```

...하지만 그것은 불가능합니다. 왜냐하면 모듈은 자신을 가져올 수 없기 때문입니다. 대신 `<svelte:self>`를 사용합니다:

```html
{#if file.files}
	<svelte:self {...file}/>
{:else}
	<File {...file}/>
{/if}
```
