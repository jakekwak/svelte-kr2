---
title: tick
---

`tick` 함수는 구성 요소가 처음 초기화될 때뿐만 아니라 언제든지 호출할 수 있다는 점에서 다른 수명 주기 함수와 다릅니다. 보류 중인 상태 변경이 DOM에 적용되는 즉시(또는 보류 중인 상태 변경이 없는 경우 즉시) 해결되는 약속을 반환합니다.

Svelte에서 구성 요소 상태를 업데이트하면 DOM이 즉시 업데이트되지 않습니다. 대신 다른 구성 요소를 포함하여 적용해야 하는 다른 변경 사항이 있는지 확인하기 위해 다음 *미세 작업*까지 기다립니다. 그렇게 하면 불필요한 작업을 피하고 브라우저가 작업을 보다 효과적으로 일괄 처리할 수 있습니다.

이 예제에서 해당 동작을 볼 수 있습니다. 텍스트 범위를 선택하고 탭 키를 누르십시오. `<textarea>` 값이 변경되기 때문에 현재 선택 항목이 지워지고 커서가 성가시게 끝까지 이동합니다. `tick`을 가져와서 이 문제를 해결할 수 있습니다...

```js
import { tick } from 'svelte';
```

...그리고 `handleKeydown`의 끝에서 `this.selectionStart` 및 `this.selectionEnd`를 설정하기 직전에 실행합니다.

```js
await tick();
this.selectionStart = selectionStart;
this.selectionEnd = selectionEnd;
```