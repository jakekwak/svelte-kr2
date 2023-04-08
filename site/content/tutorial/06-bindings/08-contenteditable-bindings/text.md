---
title: Contenteditable bindings
---

`contenteditable="true"` 속성이 있는 요소는 `textContent` 및 `innerHTML` 바인딩을 지원합니다.

```html
<div
	contenteditable="true"
	bind:innerHTML={html}
></div>
```