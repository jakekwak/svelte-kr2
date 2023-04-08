---
title: Template syntax
---


### Tags

---

`<div>`와 같은 소문자 태그는 일반 HTML 요소를 나타냅니다. `<Widget>` 또는 `<Namespace.Widget>`과 같은 대문자 태그는 *구성 요소*를 나타냅니다.

```sv
<script>
	import Widget from './Widget.svelte';
</script>

<div>
	<Widget/>
</div>
```


### Attributes and props

---

기본적으로 속성은 HTML과 동일하게 작동합니다.

```sv
<div class="foo">
	<button disabled>can't touch this</button>
</div>
```

---

HTML에서와 같이 값은 인용되지 않을 수 있습니다.

```sv
<input type=checkbox>
```

---

속성 값에는 JavaScript 표현식이 포함될 수 있습니다.

```sv
<a href="page/{p}">page {p}</a>
```

---

또는 JavaScript 표현식이 *일* 수 있습니다.


```sv
<button disabled={!clickable}>...</button>
```

---

부울 속성은 값이 [truthy](https://developer.mozilla.org/en-US/docs/Glossary/Truthy)인 경우 요소에 포함되고 [falsy](https://developer.mozilla.org/en-US/docs/Glossary/Falsy)인 경우 제외됩니다.

값이 [nullish](https://developer.mozilla.org/en-US/docs/Glossary/Nullish)(`null` 또는 `undefined`)가 아니면 다른 모든 속성이 포함됩니다.

```html
<input required={false} placeholder="This input field is not required">
<div title={null}>This div has no title attribute</div>
```

---

식에는 일반 HTML에서 구문 강조 표시가 실패하도록 하는 문자가 포함될 수 있으므로 값을 인용하는 것이 허용됩니다. 따옴표는 값이 구문 분석되는 방식에 영향을 주지 않습니다.

```sv
<button disabled="{number !== 42}">...</button>
```

---

속성 이름과 값이 일치하면(`name={name}`) `{name}`으로 대체할 수 있습니다.

```sv
<!-- These are equivalent -->
<button disabled={disabled}>...</button>
<button {disabled}>...</button>
```

---

관례적으로 구성 요소에 전달되는 값은 DOM의 기능인 *attributes*가 아니라 *properties* 또는 *props*라고 합니다.

요소와 마찬가지로 `name={name}`은 `{name}` 축약형으로 바꿀 수 있습니다.

```sv
<Widget foo={bar} answer={42} text="hello"/>
```

---

*확산 속성*을 사용하면 많은 속성 또는 속성을 요소 또는 구성 요소에 한 번에 전달할 수 있습니다.

요소 또는 구성 요소에는 일반 속성이 산재된 여러 스프레드 속성이 있을 수 있습니다.

```sv
<Widget {...things}/>
```

---

*`$$props`*는 `export`로 선언되지 않은 것을 포함하여 컴포넌트에 전달되는 모든 props를 참조합니다. Svelte가 최적화하기 어렵기 때문에 일반적으로 권장되지 않습니다. 그러나 드문 경우에 유용할 수 있습니다. 예를 들어 어떤 props가 구성 요소에 전달될 수 있는지 컴파일 시간에 알지 못하는 경우입니다.

```sv
<Widget {...$$props}/>
```

---

*`$$restProps`*는 `export`로 선언되지 *않은* props만 포함합니다. 알 수 없는 다른 특성을 구성 요소의 요소로 전달하는 데 사용할 수 있습니다. *`$$props`*와 동일한 최적화 문제를 공유하며 마찬가지로 권장되지 않습니다.

```html
<input {...$$restProps}>
```


> `input` 요소 또는 그 하위 `option` 요소의 `value` 속성은 `bind:group` 또는 `bind:checked`를 사용할 때 확산 속성으로 설정되지 않아야 합니다. Svelte는 이러한 경우 바인딩된 변수에 연결할 수 있도록 마크업에서 직접 요소의 '값'을 볼 수 있어야 합니다.

> 때로는 Svelte가 JavaScript에서 속성을 순차적으로 설정하므로 속성 순서가 중요합니다. 예를 들어 `<input type="range" min="0" max="1" value={0.5} step="0.1"/>`, Svelte는 값을 `1`(에서 반올림 0.5 단계는 기본적으로 1), 단계를 '0.1'로 설정합니다. 이 문제를 해결하려면 `<input type="range" min="0" max="1" step="0.1" value={0.5}/>`로 변경합니다.

> 또 다른 예는 `<img src="..." loading="lazy" />`입니다. Svelte는 img 요소 `loading="lazy"`를 만들기 전에 img `src`를 설정합니다. 이는 아마도 너무 늦었을 것입니다. 이미지를 느리게 로드하려면 이것을 `<img loading="lazy" src="...">`로 변경하세요.

---

### Text expressions

```sv
{expression}
```

---

텍스트에는 JavaScript 표현식도 포함될 수 있습니다.

> 정규식(`RegExp`)[문자 표기법](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp#literal_notation_and_constructor)을 사용하는 경우, 괄호로 묶어야 합니다.

```sv
<h1>Hello {name}!</h1>
<p>{a} + {b} = {a + b}.</p>

<div>{(/^[A-Za-z ]+$/).test(value) ? x : y}</div>
```

### Comments

---

구성 요소 내에서 HTML 주석을 사용할 수 있습니다.

```sv
<!-- this is a comment! -->
<h1>Hello world</h1>
```

---

`svelte-ignore`로 시작하는 주석은 다음 마크업 블록에 대한 경고를 비활성화합니다. 일반적으로 이는 접근성 경고입니다. 정당한 이유로 비활성화했는지 확인하십시오.

```sv
<!-- svelte-ignore a11y-autofocus -->
<input bind:value={name} autofocus>
```


### {#if ...}

```sv
{#if expression}...{/if}
```
```sv
{#if expression}...{:else if expression}...{/if}
```
```sv
{#if expression}...{:else}...{/if}
```

---

조건부로 렌더링된 콘텐츠는 if 블록으로 래핑할 수 있습니다.

```sv
{#if answer === 42}
	<p>what was the question?</p>
{/if}
```

---

추가 조건은 `{:else if expression}`으로 추가할 수 있으며 선택적으로 `{:else}` 절로 끝납니다.

```sv
{#if porridge.temperature > 100}
	<p>too hot!</p>
{:else if 80 > porridge.temperature}
	<p>too cold!</p>
{:else}
	<p>just right!</p>
{/if}
```


### {#each ...}

```sv
{#each expression as name}...{/each}
```
```sv
{#each expression as name, index}...{/each}
```
```sv
{#each expression as name (key)}...{/each}
```
```sv
{#each expression as name, index (key)}...{/each}
```
```sv
{#each expression as name}...{:else}...{/each}
```

---

값 목록에 대한 반복은 각 블록에서 수행할 수 있습니다.

```sv
<h1>Shopping list</h1>
<ul>
	{#each items as item}
		<li>{item.name} x {item.qty}</li>
	{/each}
</ul>
```

각 블록을 사용하여 배열 또는 배열과 유사한 값, 즉 `length` 속성이 있는 모든 개체를 반복할 수 있습니다.

---

각 블록은 `array.map(...)` 콜백의 두 번째 인수에 해당하는 *인덱스*를 지정할 수도 있습니다.

```sv
{#each items as item, i}
	<li>{i + 1}: {item.name} x {item.qty}</li>
{/each}
```

---

각 목록 항목을 고유하게 식별해야 하는 *key* 표현식이 제공되는 경우 Svelte는 마지막에 항목을 추가하거나 제거하는 대신 데이터가 변경될 때 이를 사용하여 목록을 비교합니다. 키는 모든 개체가 될 수 있지만 문자열과 숫자는 개체 자체가 변경될 때 ID를 유지할 수 있으므로 권장됩니다.

```sv
{#each items as item (item.id)}
	<li>{item.name} x {item.qty}</li>
{/each}

<!-- or with additional index value -->
{#each items as item, i (item.id)}
	<li>{i + 1}: {item.name} x {item.qty}</li>
{/each}
```

---

각 블록에서 Destructuring 및 Rest 패턴을 자유롭게 사용할 수 있습니다.

```sv
{#each items as { id, name, qty }, i (id)}
	<li>{i + 1}: {name} x {qty}</li>
{/each}

{#each objects as { id, ...rest }}
	<li><span>{id}</span><MyComponent {...rest}/></li>
{/each}

{#each items as [id, ...rest]}
	<li><span>{id}</span><MyComponent values={rest}/></li>
{/each}
```

---

각 블록에는 목록이 비어 있는 경우 렌더링되는 `{:else}` 절도 있을 수 있습니다.

```sv
{#each todos as todo}
	<p>{todo.text}</p>
{:else}
	<p>No tasks today!</p>
{/each}
```


### {#await ...}

```sv
{#await expression}...{:then name}...{:catch name}...{/await}
```
```sv
{#await expression}...{:then name}...{/await}
```
```sv
{#await expression then name}...{/await}
```
```sv
{#await expression catch name}...{/await}
```

---

Await 블록을 사용하면 Promise의 세 가지 가능한 상태(보류, 이행 또는 거부)에서 분기할 수 있습니다. SSR 모드에서는 보류 상태만 서버에서 렌더링됩니다.

```sv
{#await promise}
	<!-- promise is pending -->
	<p>waiting for the promise to resolve...</p>
{:then value}
	<!-- promise was fulfilled -->
	<p>The value is {value}</p>
{:catch error}
	<!-- promise was rejected -->
	<p>Something went wrong: {error.message}</p>
{/await}
```

---

약속이 거부될 때(또는 가능한 오류가 없을 때) 아무것도 렌더링할 필요가 없다면 `catch` 블록을 생략할 수 있습니다.

```sv
{#await promise}
	<!-- promise is pending -->
	<p>waiting for the promise to resolve...</p>
{:then value}
	<!-- promise was fulfilled -->
	<p>The value is {value}</p>
{/await}
```

---

보류 상태에 대해 신경 쓰지 않는다면 초기 블록을 생략할 수도 있습니다.

```sv
{#await promise then value}
	<p>The value is {value}</p>
{/await}
```

---

마찬가지로 오류 상태만 표시하려는 경우 `then` 블록을 생략할 수 있습니다.

```sv
{#await promise catch error}
	<p>The error is {error}</p>
{/await}
```

### {#key ...}

```sv
{#key expression}...{/key}
```

키 블록은 식의 값이 변경될 때 내용을 파괴하고 다시 만듭니다.

---

이는 값이 변경될 때마다 요소가 전환을 재생하도록 하려는 경우에 유용합니다.

```sv
{#key value}
	<div transition:fade>{value}</div>
{/key}
```

---

구성 요소 주변에서 사용하면 구성 요소가 다시 인스턴스화되고 다시 초기화됩니다.

```sv
{#key value}
	<Component />
{/key}
```

### {@html ...}

```sv
{@html expression}
```

---

텍스트 표현식에서 `<` 및 `>`와 같은 문자는 이스케이프됩니다. 그러나 HTML 표현식에서는 그렇지 않습니다.

표현식은 유효한 독립형 HTML이어야 합니다 — `{@html "<div>"}content{@html "</div>"}`는 작동하지 *않습니다*. `</div>`는 유효한 HTML이 아니기 때문입니다. 또한 Svelte 코드를 컴파일하지 *않습니다*.

> Svelte는 HTML을 삽입하기 전에 표현식을 삭제하지 않습니다. 신뢰할 수 없는 소스에서 데이터를 가져온 경우 데이터를 삭제해야 합니다. 그렇지 않으면 사용자가 XSS 취약성에 노출됩니다.

```sv
<div class="blog-post">
	<h1>{post.title}</h1>
	{@html post.content}
</div>
```


### {@debug ...}

```sv
{@debug}
```
```sv
{@debug var1, var2, ..., varN}
```

---

`{@debug ...}` 태그는 `console.log(...)`에 대한 대안을 제공합니다. 특정 변수가 변경될 때마다 값을 기록하고 devtools가 열려 있으면 코드 실행을 일시 중지합니다.

```sv
<script>
	let user = {
		firstname: 'Ada',
		lastname: 'Lovelace'
	};
</script>

{@debug user}

<h1>Hello {user.firstname}!</h1>
```

---

`{@debug ...}`는 쉼표로 구분된 변수 이름 목록을 허용합니다(임의의 표현식 아님).

```sv
<!-- Compiles -->
{@debug user}
{@debug user1, user2, user3}

<!-- WON'T compile -->
{@debug user.firstname}
{@debug myArray[0]}
{@debug !isReady}
{@debug typeof user === 'object'}
```

인수가 없는 `{@debug}` 태그는 지정된 변수와 달리 *모든* 상태가 변경될 때 트리거되는 `debugger` 문을 삽입합니다.


### {@const ...}

```sv
{@const assignment}
```

---

`{@const ...}` 태그는 로컬 상수를 정의합니다.

```sv
<script>
	export let boxes;
</script>

{#each boxes as box}
	{@const area = box.width * box.height}
	{box.width} * {box.height} = {area}
{/each}
```

`{@const}`는 `{#if}`, `{:else if}`, `{:else}`, `{#each}`, `{:then}`, `{:catch}`, `<Component />` 또는 `<svelte:fragment />`의 직계 자식으로만 허용됩니다.


### Element directives

속성뿐만 아니라 요소에는 어떤 방식으로든 요소의 동작을 제어하는 *지시문*이 있을 수 있습니다.


#### on:*eventname*

```sv
on:eventname={handler}
```
```sv
on:eventname|modifiers={handler}
```

---

DOM 이벤트를 수신하려면 `on:` 지시문을 사용하십시오.

```sv
<script>
	let count = 0;

	function handleClick(event) {
		count += 1;
	}
</script>

<button on:click={handleClick}>
	count: {count}
</button>
```

---

처리기는 성능 저하 없이 인라인으로 선언할 수 있습니다. 속성과 마찬가지로 구문 강조 표시를 위해 지시문 값을 인용할 수 있습니다.

```sv
<button on:click="{() => count += 1}">
	count: {count}
</button>
```

---

`|` 문자를 사용하여 DOM 이벤트에 *수정자*를 추가합니다.

```sv
<form on:submit|preventDefault={handleSubmit}>
	<!-- the `submit` event's default is prevented,
	     so the page won't reload -->
</form>
```

다음 수정자를 사용할 수 있습니다.

* `preventDefault` — 핸들러를 실행하기 전에 `event.preventDefault()`를 호출합니다.
* `stopPropagation` — `event.stopPropagation()`을 호출하여 이벤트가 다음 요소에 도달하는 것을 방지합니다.
* `passive` — 터치/휠 이벤트에서 스크롤 성능을 향상시킵니다(Svelte는 안전한 곳에 자동으로 추가합니다).
* `nonpassive` — `passive: false`를 명시적으로 설정
* `capture` — *bubbling* 단계 대신 *capture* 단계에서 핸들러를 실행합니다.
* `once` — 핸들러가 처음 실행된 후 제거
* `self` — `event.target`이 요소 자체인 경우에만 핸들러를 트리거합니다.
* `trusted` — `event.isTrusted`가 `true`인 경우에만 핸들러를 트리거합니다. 즉. 이벤트가 사용자 작업에 의해 트리거된 경우.

수정자는 서로 연결될 수 있습니다. 예: `on:click|once|capture={...}`.

---

값 없이 'on:' 지시문을 사용하면 구성 요소가 이벤트를 *전달*하므로 구성 요소 소비자가 이벤트를 수신할 수 있습니다.

```sv
<button on:click>
	The component itself will emit the click event
</button>
```

---

동일한 이벤트에 대해 여러 이벤트 리스너를 가질 수 있습니다.

```sv
<script>
	let counter = 0;
	function increment() {
		counter = counter + 1;
	}

	function track(event) {
		trackEvent(event)
	}
</script>

<button on:click={increment} on:click={track}>Click me!</button>
```

#### bind:*property*

```sv
bind:property={variable}
```

---

데이터는 일반적으로 부모에서 자식으로 흐릅니다. `bind:` 지시문을 사용하면 데이터가 자식에서 부모로 다른 방향으로 흐를 수 있습니다. 대부분의 바인딩은 특정 요소에 따라 다릅니다.

가장 간단한 바인딩은 `input.value`와 같은 속성 값을 반영합니다.

```sv
<input bind:value={name}>
<textarea bind:value={text}></textarea>

<input type="checkbox" bind:checked={yes}>
```

---

이름이 값과 일치하면 축약형을 사용할 수 있습니다.

```sv
<!-- These are equivalent -->
<input bind:value={value}>
<input bind:value>
```

---

숫자 입력 값은 강제됩니다. `input.value`는 DOM에 관한 한 문자열이지만 Svelte는 이를 숫자로 취급합니다. 입력이 비어 있거나 유효하지 않은 경우(`type="number"`의 경우) 값은 `undefined`입니다.

```sv
<input type="number" bind:value={num}>
<input type="range" bind:value={num}>
```

---

`type="file"`이 있는 `<input>` 요소에서 `bind:files`를 사용하여 선택한 파일의 [`FileList`](https://developer.mozilla.org/en-US/docs/Web/API/FileList). 읽기 전용입니다.

```sv
<label for="avatar">Upload a picture:</label>
<input
	accept="image/png, image/jpeg"
	bind:files
	id="avatar"
	name="avatar"
	type="file"
/>
```

---

`bind:` 지시어를 `on:` 지시어와 함께 사용하는 경우 정의된 순서는 이벤트 핸들러가 호출될 때 바인딩된 변수의 값에 영향을 미칩니다.

```sv
<script>
	let value = 'Hello World';
</script>

<input
	on:input="{() => console.log('Old value:', value)}"
	bind:value
	on:input="{() => console.log('New value:', value)}"
/>
```

여기서 우리는 `input` 이벤트를 사용하는 텍스트 입력 값에 바인딩했습니다. 다른 요소에 대한 바인딩은 `change`와 같은 다른 이벤트를 사용할 수 있습니다.

##### Binding `<select>` value

---

`<select>` 값 결합은 선택된 `<option>`의 `value` 속성에 해당하며, 이는 임의의 값일 수 있습니다(일반적으로 DOM의 경우와 같이 문자열이 아님).

```sv
<select bind:value={selected}>
	<option value={a}>a</option>
	<option value={b}>b</option>
	<option value={c}>c</option>
</select>
```

---

`<select multiple>` 요소는 확인란 그룹과 유사하게 동작합니다. 바인딩된 변수는 선택한 각 `<옵션>`의 `값` 속성에 해당하는 항목이 있는 배열입니다.

```sv
<select multiple bind:value={fillings}>
	<option value="Rice">Rice</option>
	<option value="Beans">Beans</option>
	<option value="Cheese">Cheese</option>
	<option value="Guac (extra)">Guac (extra)</option>
</select>
```

---

`<option>`의 값이 텍스트 내용과 일치하면 속성을 생략할 수 있습니다.

```sv
<select multiple bind:value={fillings}>
	<option>Rice</option>
	<option>Beans</option>
	<option>Cheese</option>
	<option>Guac (extra)</option>
</select>
```

---

`contenteditable` 속성이 있는 요소는 `innerHTML` 및 `textContent` 바인딩을 지원합니다.

```sv
<div contenteditable="true" bind:innerHTML={html}></div>
```

---

`<details>` 요소는 `open` 속성에 대한 바인딩을 지원합니다.

```sv
<details bind:open={isOpen}>
	<summary>Details</summary>
	<p>
		Something small enough to escape casual notice.
	</p>
</details>
```

##### Media element bindings

---

미디어 요소(`<audio>` 및 `<video>`)에는 고유한 바인딩 세트가 있습니다. 6개의 *읽기 전용* 항목...

* `duration`(읽기 전용) — 비디오의 총 재생 시간(초)
* `buffered` (읽기 전용) — `{start, end}` 객체의 배열
* `played` (읽기 전용) — 상동
* `탐색 가능`(읽기 전용) — 상동
* `seeking` (읽기 전용) — 부울
* `ended`(읽기 전용) — 부울

...그리고 5개의 *양방향* 바인딩:

* `currentTime` — 비디오의 현재 재생 시간(초)
* `playbackRate` — 비디오 재생 속도의 속도, 1은 '보통'
* `paused` — 설명이 필요하지 않습니다.
* `volume` — 0과 1 사이의 값
* `muted` — 플레이어가 음소거되었는지 여부를 나타내는 부울 값

비디오에는 읽기 전용 `videoWidth` 및 `videoHeight` 바인딩이 추가로 있습니다.

```sv
<video
	src={clip}
	bind:duration
	bind:buffered
	bind:played
	bind:seekable
	bind:seeking
	bind:ended
	bind:currentTime
	bind:playbackRate
	bind:paused
	bind:volume
	bind:muted
	bind:videoWidth
	bind:videoHeight
></video>
```

##### Block-level element bindings

---

Block-level elements have 4 read-only bindings, measured using a technique similar to [this one]:
블록 수준 요소에는 [이 것](http://www.backalleycoder.com/2013/03/18/cross-browser-event-based-element-resize-detection/)과 유사한 기술을 사용하여 측정된 4개의 읽기 전용 바인딩이 있습니다.

* `clientWidth`
* `clientHeight`
* `offsetWidth`
* `offsetHeight`

```sv
<div
	bind:offsetWidth={width}
	bind:offsetHeight={height}
>
	<Chart {width} {height}/>
</div>
```

#### bind:group

```sv
bind:group={variable}
```

---

함께 작동하는 입력은 `bind:group`을 사용할 수 있습니다.

```sv
<script>
	let tortilla = 'Plain';
	let fillings = [];
</script>

<!-- grouped radio inputs are mutually exclusive -->
<input type="radio" bind:group={tortilla} value="Plain">
<input type="radio" bind:group={tortilla} value="Whole wheat">
<input type="radio" bind:group={tortilla} value="Spinach">

<!-- grouped checkbox inputs populate an array -->
<input type="checkbox" bind:group={fillings} value="Rice">
<input type="checkbox" bind:group={fillings} value="Beans">
<input type="checkbox" bind:group={fillings} value="Cheese">
<input type="checkbox" bind:group={fillings} value="Guac (extra)">
```

#### bind:this

```sv
bind:this={dom_node}
```

---

DOM 노드에 대한 참조를 얻으려면 `bind:this`를 사용하십시오.

```sv
<script>
	import { onMount } from 'svelte';

	let canvasElement;

	onMount(() => {
		const ctx = canvasElement.getContext('2d');
		drawStuff(ctx);
	});
</script>

<canvas bind:this={canvasElement}></canvas>
```


#### class:*name*

```sv
class:name={value}
```
```sv
class:name
```

---

`class:` 지시문은 요소에서 클래스를 토글하는 더 짧은 방법을 제공합니다.

```sv
<!-- These are equivalent -->
<div class="{active ? 'active' : ''}">...</div>
<div class:active={active}>...</div>

<!-- Shorthand, for when name and value match -->
<div class:active>...</div>

<!-- Multiple class toggles can be included -->
<div class:active class:inactive={!active} class:isAdmin>...</div>
```

#### style:*property*

```sv
style:property={value}
```
```sv
style:property="value"
```
```sv
style:property
```

---

`style:` 지시문은 요소에 여러 스타일을 설정하기 위한 축약형을 제공합니다.

```sv
<!-- These are equivalent -->
<div style:color="red">...</div>
<div style="color: red;">...</div>

<!-- Variables can be used -->
<div style:color={myColor}>...</div>

<!-- Shorthand, for when property and variable name match -->
<div style:color>...</div>

<!-- Multiple styles can be included -->
<div style:color style:width="12rem" style:background-color={darkMode ? "black" : "white"}>...</div>

<!-- Styles can be marked as important -->
<div style:color|important="red">...</div>
```

---

`style:` 지시문이 `style` 속성과 결합되면 지시문이 우선합니다.

```sv
<div style="color: blue;" style:color="red">This will be red</div>
```



#### use:*action*

```sv
use:action
```
```sv
use:action={parameters}
```

```js
action = (node: HTMLElement, parameters: any) => {
	update?: (parameters: any) => void,
	destroy?: () => void
}
```

---

액션은 요소가 생성될 때 호출되는 함수입니다. 요소가 마운트 해제된 후 호출되는 `destroy` 메서드가 있는 객체를 반환할 수 있습니다.

```sv
<script>
	function foo(node) {
		// the node has been mounted in the DOM

		return {
			destroy() {
				// the node has been removed from the DOM
			}
		};
	}
</script>

<div use:foo></div>
```

---

조치에는 매개변수가 있을 수 있습니다. 반환된 값에 `update` 메서드가 있는 경우 해당 매개 변수가 변경될 때마다 Svelte가 마크업에 업데이트를 적용한 직후 호출됩니다.

> 모든 구성 요소 인스턴스에 대해 `foo` 함수를 다시 선언한다는 사실에 대해 걱정하지 마십시오. Svelte는 구성 요소 정의에서 로컬 상태에 의존하지 않는 모든 함수를 끌어올립니다.

```sv
<script>
	export let bar;

	function foo(node, bar) {
		// the node has been mounted in the DOM

		return {
			update(bar) {
				// the value of `bar` has changed
			},

			destroy() {
				// the node has been removed from the DOM
			}
		};
	}
</script>

<div use:foo={bar}></div>
```


#### transition:*fn*

```sv
transition:fn
```
```sv
transition:fn={params}
```
```sv
transition:fn|local
```
```sv
transition:fn|local={params}
```


```js
transition = (node: HTMLElement, params: any, options: { direction: 'in' | 'out' | 'both' }) => {
	delay?: number,
	duration?: number,
	easing?: (t: number) => number,
	css?: (t: number, u: number) => string,
	tick?: (t: number, u: number) => void
}
```

---

전환은 상태 변경의 결과로 DOM에 들어오거나 나가는 요소에 의해 트리거됩니다.

블록이 전환될 때 자체 전환이 없는 요소를 포함하여 블록 내부의 모든 요소는 블록의 모든 전환이 완료될 때까지 DOM에 유지됩니다.

`transition:` 지시문은 *양방향* 전환을 나타내며 전환이 진행되는 동안 원활하게 되돌릴 수 있음을 의미합니다.

```sv
{#if visible}
	<div transition:fade>
		fades in and out
	</div>
{/if}
```

> 기본적으로 인트로 전환은 첫 번째 렌더링에서 재생되지 않습니다. [구성 요소를 만들 때](/docs#run-time-client-side-component-api) `intro: true`를 설정하여 이 동작을 수정할 수 있습니다.

##### Transition parameters

---

동작과 마찬가지로 전환에는 매개변수가 있을 수 있습니다.

(더블 `{{curlies}}`는 특별한 구문이 아닙니다. 이것은 표현식 태그 내부의 객체 리터럴입니다.)

```sv
{#if visible}
	<div transition:fade="{{ duration: 2000 }}">
		fades in and out over two seconds
	</div>
{/if}
```

##### Custom transition functions

---

전환은 사용자 지정 기능을 사용할 수 있습니다. 반환된 개체에 'css' 기능이 있는 경우 Svelte는 요소에서 재생되는 CSS 애니메이션을 만듭니다.

`css`에 전달된 `t` 인수는 `easing` 함수가 적용된 후 `0`에서 `1` 사이의 값입니다. *In* 전환은 `0`에서 `1`로 실행되고, *out* 전환은 `1`에서 `0`으로 실행됩니다. 즉, `1`은 전환이 적용되지 않은 것처럼 요소의 자연스러운 상태입니다. `u` 인수는 `1 - t`와 같습니다.

이 함수는 다른 `t` 및 `u` 인수를 사용하여 전환이 시작되기 *전에* 반복적으로 호출됩니다.

```sv
<script>
	import { elasticOut } from 'svelte/easing';

	export let visible;

	function whoosh(node, params) {
		const existingTransform = getComputedStyle(node).transform.replace('none', '');

		return {
			delay: params.delay || 0,
			duration: params.duration || 400,
			easing: params.easing || elasticOut,
			css: (t, u) => `transform: ${existingTransform} scale(${t})`
		};
	}
</script>

{#if visible}
	<div in:whoosh>
		whooshes in
	</div>
{/if}
```

---

사용자 지정 전환 함수는 동일한 `t` 및 `u` 인수를 사용하여 전환 *동안* 호출되는 `tick` 함수를 반환할 수도 있습니다.

> `tick` 대신 `css`를 사용할 수 있는 경우 그렇게 하세요. CSS 애니메이션이 기본 스레드에서 실행되어 느린 장치에서 버벅거림을 방지할 수 있습니다.

```sv
<script>
	export let visible = false;

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
				const i = ~~(text.length * t);
				node.textContent = text.slice(0, i);
			}
		};
	}
</script>

{#if visible}
	<p in:typewriter="{{ speed: 1 }}">
		The quick brown fox jumps over the lazy dog
	</p>
{/if}
```

전환이 전환 개체 대신 함수를 반환하는 경우 다음 마이크로태스크에서 함수가 호출됩니다. 이렇게 하면 여러 전환이 조정되어 [교차 페이드 효과](/tutorial/deferred-transitions)가 가능해집니다.

전환 함수는 전환에 대한 정보를 포함하는 세 번째 인수인 `options`도 받습니다.

`options` 개체에서 사용 가능한 값은 다음과 같습니다.

* `direction` - 전환 유형에 따라 `in`, `out` 또는 `both` 중 하나

##### Transition events

---

전환이 있는 요소는 표준 DOM 이벤트 외에 다음 이벤트를 전달합니다.

* `introstart`
* `introend`
* `outrostart`
* `outroend`

```sv
{#if visible}
	<p
		transition:fly="{{ y: 200, duration: 2000 }}"
		on:introstart="{() => status = 'intro started'}"
		on:outrostart="{() => status = 'outro started'}"
		on:introend="{() => status = 'intro ended'}"
		on:outroend="{() => status = 'outro ended'}"
	>
		Flies in and out
	</p>
{/if}
```

---

로컬 전환은 자신이 속한 블록이 생성되거나 소멸될 때만 재생되며 상위 블록이 생성되거나 소멸될 때는 *아닙니다*.

```sv
{#if x}
	{#if y}
		<p transition:fade>
			fades in and out when x or y change
		</p>

		<p transition:fade|local>
			fades in and out only when y changes
		</p>
	{/if}
{/if}
```


#### in:*fn*/out:*fn*

```sv
in:fn
```
```sv
in:fn={params}
```
```sv
in:fn|local
```
```sv
in:fn|local={params}
```

```sv
out:fn
```
```sv
out:fn={params}
```
```sv
out:fn|local
```
```sv
out:fn|local={params}
```

---

`transition:`과 비슷하지만 DOM에 들어가거나(`in:`) 떠나는(`out:`) 요소에만 적용됩니다.

`transition:`과 달리 `in:` 및 `out:`으로 적용된 전환은 양방향이 아닙니다. 진행 중. 아웃 전환이 중단되면 전환이 처음부터 다시 시작됩니다.

```sv
{#if visible}
	<div in:fly out:fade>
		flies in, fades out
	</div>
{/if}
```



#### animate:*fn*

```sv
animate:name
```

```sv
animate:name={params}
```

```js
animation = (node: HTMLElement, { from: DOMRect, to: DOMRect } , params: any) => {
	delay?: number,
	duration?: number,
	easing?: (t: number) => number,
	css?: (t: number, u: number) => string,
	tick?: (t: number, u: number) => void
}
```

```js
DOMRect {
	bottom: number,
	height: number,
	​​left: number,
	right: number,
	​top: number,
	width: number,
	x: number,
	y: number
}
```

---

[키가 지정된 각 블록](/docs#template-syntax-each)의 내용이 재정렬되면 애니메이션이 트리거됩니다. 애니메이션은 요소가 추가되거나 제거될 때 실행되지 않고 각 블록 내 기존 데이터 항목의 인덱스가 변경될 때만 실행됩니다. Animate 지시문은 키가 지정된 각 블록의 *즉시* 자식인 요소에 있어야 합니다.

애니메이션은 Svelte의 [내장 애니메이션 기능](/docs#run-time-svelte-animate) 또는 [맞춤 애니메이션 기능](/docs#template-syntax-element-directives-animate-fn-custom-animation-functions)과 함께 사용할 수 있습니다.

```sv
<!-- When `list` is reordered the animation will run-->
{#each list as item, index (item)}
	<li animate:flip>{item}</li>
{/each}
```

##### Animation Parameters

---

동작 및 전환과 마찬가지로 애니메이션에도 매개변수가 있을 수 있습니다.

(더블 `{{curlies}}`는 특별한 구문이 아닙니다. 이것은 표현식 태그 내부의 객체 리터럴입니다.)

```sv
{#each list as item, index (item)}
	<li animate:flip="{{ delay: 500 }}">{item}</li>
{/each}
```

##### Custom animation functions

---

애니메이션은 `node`, `animation` 개체 및 모든 `parameters`를 인수로 제공하는 사용자 지정 함수를 사용할 수 있습니다. `animation` 매개변수는 각각 다음을 설명하는 [DOMRect](https://developer.mozilla.org/en-US/docs/Web/API/DOMRect#Properties)를 포함하는 `from` 및 `to` 속성을 포함하는 객체입니다. `시작` 및 `종료` 위치에 있는 요소의 형상입니다. `from` 속성은 시작 위치에 있는 요소의 DOMRect이고 `to` 속성은 목록이 재정렬되고 DOM이 업데이트된 후 최종 위치에 있는 요소의 DOMRect입니다.

반환된 개체에 `css` 메서드가 있는 경우 Svelte는 요소에서 재생되는 CSS 애니메이션을 만듭니다.

`css`에 전달되는 `t` 인수는 `easing` 함수가 적용된 후 `0`에서 `1`로 가는 값입니다. `u` 인수는 `1 - t`와 같습니다.

이 함수는 다른 `t` 및 `u` 인수를 사용하여 애니메이션이 시작되기 *전에* 반복적으로 호출됩니다.


```sv
<script>
	import { cubicOut } from 'svelte/easing';

	function whizz(node, { from, to }, params) {

		const dx = from.left - to.left;
		const dy = from.top - to.top;

		const d = Math.sqrt(dx * dx + dy * dy);

		return {
			delay: 0,
			duration: Math.sqrt(d) * 120,
			easing: cubicOut,
			css: (t, u) =>
				`transform: translate(${u * dx}px, ${u * dy}px) rotate(${t*360}deg);`
		};
	}
</script>

{#each list as item, index (item)}
	<div animate:whizz>{item}</div>
{/each}
```

---


사용자 지정 애니메이션 함수는 애니메이션 *동안* 동일한 `t` 및 `u` 인수를 사용하여 호출되는 `tick` 함수를 반환할 수도 있습니다.

> `tick` 대신 `css`를 사용할 수 있는 경우 그렇게 하세요. CSS 애니메이션이 기본 스레드에서 실행되어 느린 장치에서 버벅거림을 방지할 수 있습니다.

```sv
<script>
	import { cubicOut } from 'svelte/easing';

	function whizz(node, { from, to }, params) {

		const dx = from.left - to.left;
		const dy = from.top - to.top;

		const d = Math.sqrt(dx * dx + dy * dy);

		return {
			delay: 0,
			duration: Math.sqrt(d) * 120,
			easing: cubicOut,
			tick: (t, u) =>
				Object.assign(node.style, { color: t > 0.5 ? 'Pink' : 'Blue' })
		};
	}
</script>

{#each list as item, index (item)}
	<div animate:whizz>{item}</div>
{/each}
```

### Component directives

#### on:*eventname*

```sv
on:eventname={handler}
```

---

구성요소는 [createEventDispatcher](/docs#run-time-svelte-createeventdispatcher)를 사용하거나 DOM 이벤트를 전달하여 이벤트를 내보낼 수 있습니다. 구성 요소 이벤트를 수신하는 것은 DOM 이벤트를 수신하는 것과 같습니다.

```sv
<SomeComponent on:whatever={handler}/>
```

---

DOM 이벤트와 마찬가지로 'on:' 지시문이 값 없이 사용되면 구성 요소가 이벤트를 *전달*하므로 구성 요소의 소비자가 이벤트를 수신할 수 있습니다.

```sv
<SomeComponent on:whatever/>
```

#### --style-props

```sv
--style-props="anycssvalue"
```

---

CSS 사용자 지정 속성을 사용하여 테마 지정을 위해 스타일을 구성 요소에 소품으로 전달할 수도 있습니다.

Svelte의 구현은 기본적으로 래퍼 요소를 추가하기 위한 구문 설탕입니다. 이 예:

```sv
<Slider
  bind:value
  min={0}
  --rail-color="black"
  --track-color="rgb(0, 0, 255)"
/>
```

---

여기서 양념을 좀 빼보면:

```sv
<div style="display: contents; --rail-color: black; --track-color: rgb(0, 0, 255)">
  <Slider
    bind:value
    min={0}
    max={100}
  />
</div>
```

**참고**: 이것은 추가 `<div>`이므로 CSS 구조가 실수로 이것을 대상으로 할 수 있습니다. 이 기능을 사용할 때 이 추가된 래퍼 요소를 염두에 두십시오.

---

SVG 네임스페이스의 경우 위의 예는 대신 `<g>`를 사용하도록 디슈가링합니다.

```sv
<g style="--rail-color: black; --track-color: rgb(0, 0, 255)">
  <Slider
    bind:value
    min={0}
    max={100}
  />
</g>
```

**참고**: 이것은 추가 `<g>`이므로 CSS 구조가 실수로 이것을 대상으로 할 수 있습니다. 이 기능을 사용할 때 이 추가된 래퍼 요소를 염두에 두십시오.

---

Svelte의 CSS 변수 지원을 통해 구성 요소를 쉽게 테마화할 수 있습니다.

```sv
<!-- Slider.svelte -->
<style>
  .potato-slider-rail {
    background-color: var(--rail-color, var(--theme-color, 'purple'));
  }
</style>
```

---

따라서 높은 수준의 테마 색상을 설정할 수 있습니다.

```css
/* global.css */
html {
  --theme-color: black;
}
```

---

또는 소비자 수준에서 재정의하십시오.

```sv
<Slider --rail-color="goldenrod"/>
```

#### bind:*property*

```sv
bind:property={variable}
```

---

요소와 동일한 구문을 사용하여 구성 요소 소품에 바인딩할 수 있습니다.

```sv
<Keypad bind:value={pin}/>
```

#### bind:this

```sv
bind:this={component_instance}
```

---

구성 요소는 `bind:this`도 지원하므로 프로그래밍 방식으로 구성 요소 인스턴스와 상호 작용할 수 있습니다.

> 버튼이 처음 렌더링될 때 `cart`가 `undefined`이고 오류가 발생하므로 `{cart.empty}`를 수행할 수 없습니다.

```sv
<ShoppingCart bind:this={cart}/>

<button on:click={() => cart.empty()}>
	Empty shopping cart
</button>
```



### `<slot>`

```sv
<slot><!-- optional fallback --></slot>
```
```sv
<slot name="x"><!-- optional fallback --></slot>
```
```sv
<slot prop={value}></slot>
```

---

구성 요소는 요소와 같은 방식으로 자식 콘텐츠를 가질 수 있습니다.

콘텐츠는 자식이 제공되지 않은 경우 렌더링되는 대체 콘텐츠를 포함할 수 있는 `<slot>` 요소를 사용하여 자식 구성 요소에 노출됩니다.

```sv
<!-- Widget.svelte -->
<div>
	<slot>
		this fallback content will be rendered when no content is provided, like in the first example
	</slot>
</div>

<!-- App.svelte -->
<Widget></Widget> <!-- this component will render the default content -->

<Widget>
	<p>this is some child content that will overwrite the default slot content</p>
</Widget>
```

참고: 일반 `<slot>` 요소를 렌더링하려면 `<svelte:element this="slot" />`을 사용할 수 있습니다.

#### `<slot name="`*name*`">`

---

명명된 슬롯을 통해 소비자는 특정 영역을 타겟팅할 수 있습니다. 대체 콘텐츠를 가질 수도 있습니다.

```sv
<!-- Widget.svelte -->
<div>
	<slot name="header">No header was provided</slot>
	<p>Some content between header and footer</p>
	<slot name="footer"></slot>
</div>

<!-- App.svelte -->
<Widget>
	<h1 slot="header">Hello</h1>
	<p slot="footer">Copyright (c) 2019 Svelte Industries</p>
</Widget>
```

컴포넌트는 `<Component slot="name" />` 구문을 사용하여 명명된 슬롯에 배치할 수 있습니다.
래퍼 요소를 사용하지 않고 슬롯에 콘텐츠를 배치하려면 특수 요소 `<svelte:fragment>`를 사용할 수 있습니다.

```sv
<!-- Widget.svelte -->
<div>
	<slot name="header">No header was provided</slot>
	<p>Some content between header and footer</p>
	<slot name="footer"></slot>
</div>

<!-- App.svelte -->
<Widget>
	<HeaderComponent slot="header" />
	<svelte:fragment slot="footer">
		<p>All rights reserved.</p>
		<p>Copyright (c) 2019 Svelte Industries</p>
	</svelte:fragment>
</Widget>
```


#### `$$slots`

---

`$$slots`는 키가 부모에 의해 구성 요소에 전달된 슬롯의 이름인 객체입니다. 부모가 특정 이름을 가진 슬롯을 전달하지 않으면 해당 이름은 `$$slots`에 존재하지 않습니다. 이를 통해 구성 요소는 부모가 제공하는 경우에만 슬롯(및 스타일 지정을 위한 래퍼와 같은 다른 요소)을 렌더링할 수 있습니다.

빈 명명된 슬롯을 명시적으로 전달하면 해당 슬롯의 이름이 `$$slots`에 추가됩니다. 예를 들어 부모가 `<div slot="title" />`을 자식 구성 요소에 전달하면 `$$slots.title`이 자식 내에서 true가 됩니다.

```sv
<!-- Card.svelte -->
<div>
	<slot name="title"></slot>
	{#if $$slots.description}
		<!-- This <hr> and slot will render only if a slot named "description" is provided. -->
		<hr>
		<slot name="description"></slot>
	{/if}
</div>

<!-- App.svelte -->
<Card>
	<h1 slot="title">Blog Post Title</h1>
	<!-- No slot named "description" was provided so the optional slot will not be rendered. -->
</Card>
```

#### `<slot key={`*value*`}>`

---

슬롯은 0번 이상 렌더링될 수 있으며 props를 사용하여 부모에게 값을 *다시* 전달할 수 있습니다. 부모는 `let:` 지시문을 사용하여 슬롯 템플릿에 값을 노출합니다.

일반적인 속기 규칙이 적용됩니다. `let:item`은 `let:item={item}`과 같고 `<slot {item}>`은 `<slot item={item}>`과 같습니다.

```sv
<!-- FancyList.svelte -->
<ul>
	{#each items as item}
		<li class="fancy">
			<slot prop={item}></slot>
		</li>
	{/each}
</ul>

<!-- App.svelte -->
<FancyList {items} let:prop={thing}>
	<div>{thing.text}</div>
</FancyList>
```

---

명명된 슬롯은 값을 노출할 수도 있습니다. `let:` 지시문은 `slot` 속성이 있는 요소에 적용됩니다.

```sv
<!-- FancyList.svelte -->
<ul>
	{#each items as item}
		<li class="fancy">
			<slot name="item" {item}></slot>
		</li>
	{/each}
</ul>

<slot name="footer"></slot>

<!-- App.svelte -->
<FancyList {items}>
	<div slot="item" let:item>{item.text}</div>
	<p slot="footer">Copyright (c) 2019 Svelte Industries</p>
</FancyList>
```


### `<svelte:self>`

---

`<svelte:self>` 요소를 사용하면 구성 요소가 재귀적으로 자신을 포함할 수 있습니다.

마크업의 최상위 수준에 나타날 수 없습니다. 무한 루프를 방지하기 위해 if 또는 각 블록 내부에 있거나 구성 요소의 슬롯으로 전달되어야 합니다.

```sv
<script>
	export let count;
</script>

{#if count > 0}
	<p>counting down... {count}</p>
	<svelte:self count="{count - 1}"/>
{:else}
	<p>lift-off!</p>
{/if}
```

### `<svelte:component>`

```sv
<svelte:component this={expression}/>
```

---

`<svelte:component>` 요소는 `this` 속성으로 지정된 구성 요소 생성자를 사용하여 동적으로 구성 요소를 렌더링합니다. 속성이 변경되면 구성 요소가 소멸되고 다시 생성됩니다.

`this`가 거짓이면 구성 요소가 렌더링되지 않습니다.

```sv
<svelte:component this={currentSelection.component} foo={bar}/>
```

### `<svelte:element>`

```sv
<svelte:element this={expression}/>
```

---

`<svelte:element>` 요소를 사용하면 동적으로 지정된 유형의 요소를 렌더링할 수 있습니다. 이는 예를 들어 CMS에서 서식 있는 텍스트 콘텐츠를 표시할 때 유용합니다. 존재하는 모든 속성 및 이벤트 리스너가 요소에 적용됩니다.

유일하게 지원되는 바인딩은 'bind:this'입니다. Svelte가 빌드 시 수행하는 요소 유형별 바인딩(예: 입력 요소에 대한 'bind:value')은 다이내믹 태그 유형에서 작동하지 않기 때문입니다.

`this`에 null 값이 있는 경우 요소와 해당 자식이 렌더링되지 않습니다.

`this`가 [void 요소](https://developer.mozilla.org/en-US/docs/Glossary/Void_element)의 이름(예: `br`)이고 `<svelte:element>`에 자식 요소가 있으면 개발 모드에서 런타임 오류가 발생합니다.

```sv
<script>
	let tag = 'div';
	export let handler;
</script>

<svelte:element this={tag} on:click={handler}>Foo</svelte:element>
```

### `<svelte:window>`

```sv
<svelte:window on:event={handler}/>
```
```sv
<svelte:window bind:prop={value}/>
```

---

`<svelte:window>` 요소를 사용하면 구성 요소가 파괴될 때 이벤트 리스너를 제거하거나 서버 측 렌더링 시 `window`의 존재 여부를 확인할 필요 없이 `window` 객체에 이벤트 리스너를 추가할 수 있습니다.

`<svelte:self>`와 달리 이 요소는 구성 요소의 최상위 수준에만 나타날 수 있으며 블록이나 요소 안에 있으면 안 됩니다.

```sv
<script>
	function handleKeydown(event) {
		alert(`pressed the ${event.key} key`);
	}
</script>

<svelte:window on:keydown={handleKeydown}/>
```

---

다음 속성에 바인딩할 수도 있습니다.

* `innerWidth`
* `innerHeight`
* `outerWidth`
* `outerHeight`
* `scrollX`
* `scrollY`
* `online` — `window.navigator.onLine`의 별칭

`scrollX` 및 `scrollY`를 제외한 모든 항목은 읽기 전용입니다.

```sv
<svelte:window bind:scrollY={y}/>
```

> 접근성 문제를 피하기 위해 페이지가 초기 값으로 스크롤되지 않습니다. `scrollX` 및 `scrollY`의 바운드 변수에 대한 후속 변경만 스크롤을 발생시킵니다. 그러나 스크롤 동작이 필요한 경우 `onMount()`에서 `scrollTo()`를 호출하십시오.

### `<svelte:body>`

```sv
<svelte:body on:event={handler}/>
```

---

`<svelte:window>`와 마찬가지로 이 요소를 사용하면 `window`에서 실행되지 않는 `mouseenter` 및 `mouseleave`와 같은 `document.body`의 이벤트에 리스너를 추가할 수 있습니다. 또한 `<body>` 요소에서 [actions](/docs#template-syntax-element-directives-use-action)를 사용할 수 있습니다.

`<svelte:window>`와 마찬가지로 이 요소는 구성 요소의 최상위 수준에만 나타날 수 있으며 블록이나 요소 내부에 있어서는 안 됩니다.

```sv
<svelte:body
	on:mouseenter={handleMouseenter}
	on:mouseleave={handleMouseleave}
	use:someAction
/>
```


### `<svelte:head>`

```sv
<svelte:head>...</svelte:head>
```

---

이 요소를 사용하면 `document.head`에 요소를 삽입할 수 있습니다. 서버 측 렌더링 중에 `head` 컨텐츠는 기본 `html` 컨텐츠에 별도로 노출됩니다.

`<svelte:window>` 및 `<svelte:body>`와 마찬가지로 이 요소는 구성 요소의 최상위 수준에만 나타날 수 있으며 블록이나 요소 내부에 있으면 안 됩니다.

```sv
<svelte:head>
	<link rel="stylesheet" href="/tutorial/dark-theme.css">
</svelte:head>
```


### `<svelte:options>`

```sv
<svelte:options option={value}/>
```

---

`<svelte:options>` 요소는 [컴파일러 섹션](/docs#compile-time-svelte-compile)에 자세히 설명되어 있는 구성 요소별 컴파일러 옵션을 지정할 수 있는 위치를 제공합니다. 가능한 옵션은 다음과 같습니다.

* `immutable={true}` — 가변 데이터를 사용하지 않으므로 컴파일러는 값이 변경되었는지 확인하기 위해 간단한 참조 동등성 검사를 수행할 수 있습니다.
* `immutable={false}` — 기본값입니다. Svelte는 변경 가능한 객체가 변경되었는지 여부에 대해 더 보수적입니다.
* `accessors={true}` — 구성 요소의 소품에 대한 getter 및 setter를 추가합니다.
* `accessors={false}` — 기본값
* `namespace="..."` — 이 구성 요소가 사용될 네임스페이스, 가장 일반적으로 "svg"; "외부" 네임스페이스를 사용하여 대소문자를 구분하지 않는 속성 이름 및 HTML 관련 경고를 옵트아웃합니다.
* `tag="..."` — 이 구성 요소를 사용자 정의 요소로 컴파일할 때 사용할 이름

```sv
<svelte:options tag="my-custom-element"/>
```

### `<svelte:fragment>`

`<svelte:fragment>` 요소를 사용하면 콘텐츠를 컨테이너 DOM 요소에 래핑하지 않고 [명명된 슬롯](/docs#template-syntax-slot-slot-name-name)에 배치할 수 있습니다. 이렇게 하면 문서의 흐름 레이아웃이 그대로 유지됩니다.

```sv
<!-- Widget.svelte -->
<div>
	<slot name="header">No header was provided</slot>
	<p>Some content between header and footer</p>
	<slot name="footer"></slot>
</div>

<!-- App.svelte -->
<Widget>
	<h1 slot="header">Hello</h1>
	<svelte:fragment slot="footer">
		<p>All rights reserved.</p>
		<p>Copyright (c) 2019 Svelte Industries</p>
	</svelte:fragment>
</Widget>
```
