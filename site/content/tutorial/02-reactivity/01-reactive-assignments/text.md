---
title: Assignments
---

Svelte의 중심에는 예를 들어 이벤트에 대한 응답으로 DOM을 애플리케이션 상태와 동기화 상태로 유지하기 위한 강력한 *반응성* 시스템이 있습니다.

이를 시연하려면 먼저 이벤트 핸들러를 연결해야 합니다. 9행을 다음과 같이 바꿉니다.

```html
<button on:click={incrementCount}>
```

`incrementCount` 함수 내에서 `count` 값을 변경하기만 하면 됩니다.

```js
function incrementCount() {
	count += 1;
}
```

Svelte는 DOM을 업데이트해야 함을 알려주는 일부 코드를 사용하여 이 할당을 '계측(instruments)'합니다.
