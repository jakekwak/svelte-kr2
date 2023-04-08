---
title: <svelte:body>
---

`<svelte:window>`와 마찬가지로 `<svelte:body>` 요소를 사용하면 `document.body`에서 실행되는 이벤트를 수신할 수 있습니다. 이는 `window`에서 실행되지 않는 `mouseenter` 및 `mouseleave` 이벤트에 유용합니다.

`mouseenter` 및 `mouseleave` 처리기를 `<svelte:body>` 태그에 추가합니다.

```html
<svelte:body
	on:mouseenter={handleMouseenter}
	on:mouseleave={handleMouseleave}
/>
```