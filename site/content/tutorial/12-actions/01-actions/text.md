---
title: The use directive
---

작업은 기본적으로 요소 수준 수명 주기 함수입니다. 다음과 같은 경우에 유용합니다.

- 타사 라이브러리와의 인터페이스
- 지연 로딩 이미지
- 툴팁
- 커스텀 이벤트 핸들러 추가

이 앱에서는 사용자가 주황색 모달 외부를 클릭할 때 주황색 모달이 닫히도록 만들고 싶습니다. `outclick` 이벤트에 대한 이벤트 핸들러가 있지만 기본 DOM 이벤트는 아닙니다. 우리가 직접 디스패치해야 합니다. 먼저 `clickOutside` 함수를 가져옵니다...

```js
import { clickOutside } from "./click_outside.js";
```

...그런 다음 요소와 함께 사용하십시오.

```html
<div class="box" use:clickOutside on:outclick="{() => (showModal = false)}">
	Click outside me!
</div>
```

`click_outside.js` 파일을 엽니다. 전환 함수와 마찬가지로 액션 함수는 `node`(액션이 적용되는 요소)와 일부 선택적 매개 변수를 수신하고 액션 객체를 반환합니다. 해당 개체에는 요소가 마운트 해제될 때 호출되는 `destroy` 함수가 있을 수 있습니다.

사용자가 주황색 상자 외부를 클릭할 때 `outclick` 이벤트를 시작하려고 합니다. 한 가지 가능한 구현은 다음과 같습니다.

```js
export function clickOutside(node) {
	const handleClick = (event) => {
		if (!node.contains(event.target)) {
			node.dispatchEvent(new CustomEvent("outclick"));
		}
	};

	document.addEventListener("click", handleClick, true);

	return {
		destroy() {
			document.removeEventListener("click", handleClick, true);
		},
	};
}
```

`clickOutside` 기능을 업데이트하고 버튼을 클릭하여 모달을 표시한 다음 외부를 클릭하여 닫습니다.
