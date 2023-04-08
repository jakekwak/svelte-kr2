---
title: setContext and getContext
---

컨텍스트 API는 데이터와 함수를 소품으로 전달하거나 많은 이벤트를 전달하지 않고 구성 요소가 서로 '대화'할 수 있는 메커니즘을 제공합니다. 고급 기능이지만 유용한 기능입니다.

[Mapbox GL](https://docs.mapbox.com/mapbox-gl-js/overview/) 지도를 사용하는 이 예시 앱을 살펴보세요. `<MapMarker>` 구성 요소를 사용하여 마커를 표시하고 싶지만 기본 Mapbox 인스턴스에 대한 참조를 각 구성 요소의 소품으로 전달하고 싶지는 않습니다.

컨텍스트 API에는 `setContext`와 `getContext`라는 두 부분이 있습니다. 구성 요소가 `setContext(key, context)`를 호출하면 모든 *자식* 구성 요소가 `const context = getContext(key)`로 컨텍스트를 검색할 수 있습니다.

먼저 컨텍스트를 설정해 보겠습니다. `Map.svelte`에서 `svelte`에서 `setContext`를 가져오고 `mapbox.js`에서 `key`를 가져오고 `setContext`를 호출합니다.

```js
import { onDestroy, setContext } from 'svelte';
import { mapbox, key } from './mapbox.js';

setContext(key, {
	getMap: () => map
});
```

컨텍스트 개체는 원하는 모든 것이 될 수 있습니다. [수명 주기 함수](/tutorial/onmount)와 마찬가지로 `setContext` 및 `getContext`는 구성요소 초기화 중에 호출되어야 합니다. 나중에 호출하면(예: `onMount` 내에서) 오류가 발생합니다. 이 예에서 `map`은 구성 요소가 마운트될 때까지 생성되지 않으므로 컨텍스트 객체에는 `map` 자체가 아닌 `getMap` 함수가 포함됩니다.

방정식의 다른 측면에서 `MapMarker.svelte`에서 이제 Mapbox 인스턴스에 대한 참조를 얻을 수 있습니다.

```js
import { getContext } from 'svelte';
import { mapbox, key } from './mapbox.js';

const { getMap } = getContext(key);
const map = getMap();
```

이제 마커가 지도에 추가될 수 있습니다.

> `<MapMarker>`의 더 완성된 버전도 제거 및 소품 변경을 처리하지만 여기서는 컨텍스트만 보여줍니다.

## Context keys

In `mapbox.js` you'll see this line:

```js
const key = Symbol();
```

기술적으로는 모든 값을 키로 사용할 수 있습니다. 예를 들어 `setContext('mapbox', ...)`를 사용할 수 있습니다. 문자열 사용의 단점은 다른 구성 요소 라이브러리가 실수로 같은 것을 사용할 수 있다는 것입니다. 반면에 [기호](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol)를 사용한다는 것은 키가 어떤 상황에서도 충돌하지 않는다는 것을 의미합니다. 기호는 본질적으로 고유한 식별자이기 때문에 많은 구성 요소 레이어에서 작동하는 여러 다른 컨텍스트가 있는 경우에도 마찬가지입니다.

## Contexts vs. stores

컨텍스트와 스토어는 비슷해 보입니다. 저장소는 앱의 *모든* 부분에서 사용할 수 있는 반면 컨텍스트는 *구성 요소와 해당 하위 항목*에서만 사용할 수 있다는 점에서 다릅니다. 이것은 하나의 상태가 다른 것의 상태를 방해하지 않고 구성 요소의 여러 인스턴스를 사용하려는 경우에 유용할 수 있습니다.

실제로 두 가지를 함께 사용할 수도 있습니다. 컨텍스트는 반응적이지 않으므로 시간이 지남에 따라 변경되는 값은 저장소로 표시되어야 합니다.

```js
const { these, are, stores } = getContext(...);
```
