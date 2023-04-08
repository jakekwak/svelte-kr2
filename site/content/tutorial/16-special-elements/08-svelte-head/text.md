---
title: <svelte:head>
---

`<svelte:head>` 요소를 사용하면 문서의 `<head>` 내부에 요소를 삽입할 수 있습니다.

```html
<svelte:head>
	<link rel="stylesheet" href="/tutorial/dark-theme.css">
</svelte:head>
```

> SSR(서버측 렌더링) 모드에서 `<svelte:head>`의 내용은 나머지 HTML과 별도로 반환됩니다.