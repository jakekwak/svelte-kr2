---
title: Select bindings
---

`<select>` 요소와 함께 `bind:value`를 사용할 수도 있습니다. 라인 20을 업데이트합니다:

```html
<select bind:value={selected} on:change="{() => answer = ''}">
```

`<option>` 값은 문자열이 아닌 객체입니다. Svelte는 신경 쓰지 않습니다.

> `selected`의 초기 값을 설정하지 않았기 때문에 바인딩은 자동으로 기본값(목록의 첫 번째 값)으로 설정합니다. 하지만 조심하세요. 바인딩이 초기화될 때까지 `selected`는 정의되지 않은 상태로 유지되므로 맹목적으로 참조할 수 없습니다. 템플릿의 `selected.id`. 사용 사례에서 허용하는 경우 이 문제를 우회하도록 초기 값을 설정할 수도 있습니다.
