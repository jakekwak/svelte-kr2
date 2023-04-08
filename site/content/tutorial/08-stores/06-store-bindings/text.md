---
title: Store bindings
---

스토어가 쓰기 가능한 경우(즉, `set` 메서드가 있는 경우) 로컬 구성 요소 상태에 바인딩할 수 있는 것처럼 해당 값에 바인딩할 수 있습니다.

이 예에서는 쓰기 가능한 스토어 `name`과 파생된 스토어 `greeting`이 있습니다. `<input>` 요소를 업데이트합니다.

```html
<input bind:value={$name}>
```

입력 값을 변경하면 이제 `name` 및 모든 종속 항목이 업데이트됩니다.

구성 요소 내부에 값을 저장하도록 직접 할당할 수도 있습니다. `<button>` 요소를 추가합니다.

```html
<button on:click="{() => $name += '!'}">
	Add exclamation mark!
</button>
```

`$name += '!'` 할당은 `name.set($name + '!')`과 동일합니다.