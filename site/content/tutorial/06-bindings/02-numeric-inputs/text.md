---
title: Numeric inputs
---

DOM에서는 모든 것이 문자열입니다. 이는 `type="number"` 및 `type="range"`와 같은 숫자 입력을 처리할 때 도움이 되지 않습니다. 이는 `input.value`를 사용하기 전에 강제 변환해야 한다는 것을 의미하기 때문입니다.

`bind:value`를 사용하면 Svelte가 처리합니다.

```html
<input type=number bind:value={a} min=0 max=10>
<input type=range bind:value={a} min=0 max=10>
```