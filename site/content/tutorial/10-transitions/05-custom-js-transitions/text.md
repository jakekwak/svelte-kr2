---
title: Custom JS transitions
---

일반적으로 가능한 한 전환에 CSS를 사용해야 하지만 타자기 효과와 같이 JavaScript 없이는 달성할 수 없는 몇 가지 효과가 있습니다.

```js
function typewriter(node, { speed = 1 }) {
	const valid = (
		node.childNodes.length === 1 &&
		node.childNodes[0].nodeType === Node.TEXT_NODE
	);

	if (!valid) {
		throw new Error(`This transition only works on elements with a single text node child`);
	}

	const text = node.textContent;
	const duration = text.length / (speed * 0.01);

	return {
		duration,
		tick: t => {
			const i = Math.trunc(text.length * t); // ~~(text.length * t)
			node.textContent = text.slice(0, i);
		}
	};
}
```
