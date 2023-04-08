---
title: Writable stores
---

모든 애플리케이션 상태가 애플리케이션의 구성 요소 계층 구조에 속하는 것은 아닙니다. 경우에 따라 관련 없는 여러 구성 요소 또는 일반 JavaScript 모듈에서 액세스해야 하는 값이 있을 수 있습니다.

Svelte에서는 *stores*로 이 작업을 수행합니다. 스토어는 단순히 스토어 값이 변경될 때마다 관심 있는 당사자에게 알림을 제공하는 '구독' 메서드가 있는 객체입니다. `App.svelte`에서 `count`는 저장소이고 `count.subscribe` 콜백에서 `countValue`를 설정합니다.

`count`의 정의를 보려면 `stores.js` 탭을 클릭하십시오. 이것은 *쓰기 가능한* 저장소입니다. 즉, `subscribe` 외에도 `set` 및 `update` 메소드가 있습니다.

이제 `Incrementer.svelte` 탭으로 이동하여 `+` 버튼을 연결할 수 있습니다.

```js
function increment() {
	count.update(n => n + 1);
}
```

이제 `+` 버튼을 클릭하면 카운트가 업데이트됩니다. `Decrementer.svelte`에 대해 역으로 수행합니다.

마지막으로 `Resetter.svelte`에서 `reset`을 구현합니다.

```js
function reset() {
	count.set(0);
}
```
