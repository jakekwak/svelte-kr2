---
title: DOM event forwarding
---

이벤트 전달은 DOM 이벤트에도 적용됩니다.

`<CustomButton>`에 대한 클릭에 대한 알림을 받으려면 `CustomButton.svelte`의 `<button>` 요소에 대한 `click` 이벤트를 전달하기만 하면 됩니다.

```html
<button on:click>
	Click me
</button>
```