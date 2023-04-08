---
title: <svelte:window>
---

모든 DOM 요소에 이벤트 리스너를 추가할 수 있는 것처럼 `<svelte:window>`를 사용하여 `window` 객체에 이벤트 리스너를 추가할 수 있습니다.

11행에서 `keydown` 리스너를 추가합니다.

```html
<svelte:window on:keydown={handleKeydown}/>
```

> DOM 요소와 마찬가지로 `preventDefault`와 같은 [이벤트 수정자](/tutorial/event-modifiers)를 추가할 수 있습니다.
