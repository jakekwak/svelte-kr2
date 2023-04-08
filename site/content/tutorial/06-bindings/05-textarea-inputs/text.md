---
title: Textarea inputs
---

`<textarea>` 요소는 Svelte의 텍스트 입력과 유사하게 동작합니다 — `bind:value`를 사용합니다:

```html
<textarea bind:value={value}></textarea>
```

이와 같이 이름이 일치하는 경우 단축 형식을 사용할 수도 있습니다.

```html
<textarea bind:value></textarea>
```

이것은 텍스트 영역뿐만 아니라 모든 바인딩에 적용됩니다.