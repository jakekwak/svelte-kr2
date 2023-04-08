---
title: Custom stores
---

개체가 `subscribe` 메서드를 올바르게 구현하는 한 저장소입니다. 그 이상은 무엇이든 됩니다. 따라서 도메인별 논리를 사용하여 사용자 지정 저장소를 만드는 것은 매우 쉽습니다.

예를 들어, 이전 예제의 `count` 저장소에는 `increment`, `decrement` 및 `reset` 메소드가 포함될 수 있으며 `set` 및 `update` 노출을 피할 수 있습니다.

```js
function createCount() {
	const { subscribe, set, update } = writable(0);

	return {
		subscribe,
		increment: () => update(n => n + 1),
		decrement: () => update(n => n - 1),
		reset: () => set(0)
	};
}
```

