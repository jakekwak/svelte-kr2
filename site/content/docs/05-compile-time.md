---
title: Compile time
---

일반적으로 Svelte 컴파일러와 직접 상호 작용하지 않고 대신 번들러 플러그인을 사용하여 빌드 시스템에 통합합니다. Svelte 팀이 가장 추천하고 투자하는 번들러 플러그인은 [vite-plugin-svelte](https://github.com/sveltejs/vite-plugin-svelte)입니다. [SvelteKit](https://kit.svelte.dev/) 프레임워크는 `vite-plugin-svelte`를 활용하여 애플리케이션을 구축하는 설정과 [Svelte 구성 요소 라이브러리 패키징 도구](https://kit. svelte.dev/docs/packaging). Svelte Society는 Rollup 및 Webpack과 같은 추가 도구에 대한 [기타 번들러 플러그인](https://sveltesociety.dev/tools/#bundling) 목록을 유지 관리합니다.

그럼에도 불구하고 번들러 플러그인은 일반적으로 컴파일러 옵션을 노출하므로 컴파일러 사용 방법을 이해하는 것이 유용합니다.



### `svelte.compile`

```js
result: {
	js,
	css,
	ast,
	warnings,
	vars,
	stats
} = svelte.compile(source: string, options?: {...})
```

---

이것은 마술이 일어나는 곳입니다. `svelte.compile`은 구성 요소 소스 코드를 가져와서 클래스를 내보내는 JavaScript 모듈로 바꿉니다.

```js
const svelte = require('svelte/compiler');

const result = svelte.compile(source, {
	// options
});
```

다음 옵션을 컴파일러에 전달할 수 있습니다. 필요 없음:

<!-- | option | type | default
| --- | --- | --- |
| `filename` | string | `null`
| `name` | string | `"Component"`
| `format` | `"esm"` or `"cjs"` | `"esm"`
| `generate` | `"dom"` or `"ssr"` or `false` | `"dom"`
| `errorMode` | `"throw"` or `"warn"` | `"throw"`
| `varsReport` | `"strict"` or `"full"` or `false` | `"strict"`
| `dev` | boolean | `false`
| `immutable` | boolean | `false`
| `hydratable` | boolean | `false`
| `legacy` | boolean | `false`
| `customElement` | boolean | `false`
| `tag` | string | null
| `accessors` | boolean | `false`
| `css` | boolean | `true`
| `loopGuardTimeout` | number | 0
| `preserveComments` | boolean | `false`
| `preserveWhitespace` | boolean | `false`
| `outputFilename` | string | `null`
| `cssOutputFilename` | string | `null`
| `sveltePath` | string | `"svelte"` -->

| option | default | description |
| --- | --- | --- |
| `filename` | `null` | 디버깅 힌트 및 소스 맵에 사용되는 `string`. 번들러 플러그인이 자동으로 설정합니다.
| `name` | `"Component"` | 결과 JavaScript 클래스의 이름을 설정하는 `문자열`(컴파일러가 범위 내의 다른 변수와 충돌하는 경우 이름을 바꾸지만). 일반적으로 `filename`에서 유추됩니다.
| `format` | `"esm"` | `"esm"`인 경우 JavaScript 모듈을 생성합니다(`import` 및 `export` 사용). `"cjs"`인 경우 일부 서버 측 렌더링 상황이나 테스트에 유용한 CommonJS 모듈(`require` 및 `module.exports` 포함)을 생성합니다.
| `generate` | `"dom"` | `"dom"`인 경우 Svelte는 DOM에 마운트하기 위한 JavaScript 클래스를 내보냅니다. `"ssr"`인 경우 Svelte는 서버 측 렌더링에 적합한 `render` 방법으로 객체를 내보냅니다. `false`인 경우 JavaScript 또는 CSS가 반환되지 않습니다. 그냥 메타데이터.
| `errorMode` | `"throw"` | `"throw"`인 경우 Svelte는 컴파일 오류가 발생했을 때 throw합니다. "warn"`인 경우 Svelte는 오류를 경고로 처리하고 경고 보고서에 추가합니다.
| `varsReport` | `"strict"` | `"strict"`인 경우 Svelte는 전역 변수도 내부 변수도 아닌 변수만 포함된 변수 보고서를 반환합니다. `"가득"`인 경우 Svelte는 감지된 모든 변수가 포함된 변수 보고서를 반환합니다. `false`인 경우 변수 보고서가 반환되지 않습니다.
| `dev` | `false` | `true`인 경우 런타임 검사를 수행하고 개발 중에 디버깅 정보를 제공하는 구성 요소에 추가 코드가 추가됩니다.
| `immutable` | `false` | `true`이면 어떤 객체도 변경하지 않겠다고 컴파일러에 알립니다. 이를 통해 값이 변경되었는지 확인하는 데 덜 보수적일 수 있습니다.
| `hydratable` | `false` | DOM 코드를 생성할 때 `true`인 경우 `hydrate: true` 런타임 옵션을 활성화하여 구성 요소가 처음부터 새 DOM을 생성하는 대신 기존 DOM을 업그레이드할 수 있습니다. SSR 코드를 생성할 때 이것은 `<head>` 요소에 마커를 추가하여 hydration이 대체할 항목을 알 수 있도록 합니다.
| `legacy` | `false` | `true`인 경우 `element.dataset`과 같은 항목을 지원하지 않는 IE9 및 IE10에서 작동하는 코드를 생성합니다.
| `accessors` | `false` | `true`이면 구성 요소의 소품에 대해 getter 및 setter가 생성됩니다. `false`인 경우 읽기 전용으로 내보낸 값(즉 `const`, `class` 및 `function`으로 선언된 값)에 대해서만 생성됩니다. `customElement: true`로 컴파일하는 경우 이 옵션의 기본값은 `true`입니다.
| `customElement` | `false` | 'true'인 경우 일반 Svelte 구성 요소 대신 사용자 지정 요소 생성자를 생성하도록 컴파일러에 지시합니다.
| `tag` | `null` | 사용자 정의 요소를 등록할 태그 이름을 Svelte에 알려주는 '문자열'입니다. 하이픈이 하나 이상 포함된 소문자 영숫자 문자열이어야 합니다. 예: `"my-element"`.
| `css` | `'injected'` | `'주입'`(이전에는 `true`)인 경우 스타일이 JavaScript 클래스에 포함되고 런타임 시 실제로 렌더링되는 구성 요소에 삽입됩니다. `'외부'`(이전에는 `false`)인 경우 CSS는 컴파일 결과의 `css` 필드에 반환됩니다. 대부분의 Svelte 번들러 플러그인은 이것을 `'외부'`로 설정하고 더 나은 성능을 위해 정적으로 생성되는 CSS를 사용합니다. 그 이유는 더 작은 JavaScript 번들을 생성하고 출력이 캐시 가능한 `.css` 파일로 제공될 수 있기 때문입니다. `'none'`인 경우 스타일을 완전히 피하고 CSS 출력이 생성되지 않습니다.
| `cssHash` | See right | `{ hash, css, name, filename }` 인수를 사용하고 범위가 지정된 CSS의 클래스 이름으로 사용되는 문자열을 반환하는 함수입니다. 기본적으로 `svelte-${hash(css)}`를 반환합니다.
| `loopGuardTimeout` | 0 | Svelte가 `loopGuardTimeout` ms보다 더 오랫동안 스레드를 차단하는 경우 루프를 중단하도록 지시하는 `숫자`입니다. 이는 무한 루프를 방지하는 데 유용합니다. **`dev: true`인 경우에만 사용 가능**
| `preserveComments` | `false` | 'true'이면 서버 측 렌더링 중에 HTML 주석이 보존됩니다. 기본적으로 제거됩니다.
| `preserveWhitespace` | `false` | `true`인 경우 가능한 한 단일 공백으로 축소되거나 제거되지 않고 입력한 대로 요소 내부 및 요소 사이의 공백이 유지됩니다.
| `sourcemap` | `object \| string` | 최종 출력 소스 맵에 병합될 초기 소스 맵입니다. 이것은 일반적으로 전처리기 소스 맵입니다.
| `enableSourcemap` | `boolean \| { js: boolean; css: boolean; }` | `true`인 경우 Svelte는 구성 요소에 대한 소스 맵을 생성합니다. 소스 맵 생성을 보다 세밀하게 제어하려면 `js` 또는 `css`와 함께 개체를 사용하십시오. 기본적으로 이것은 '참'입니다.
| `outputFilename` | `null` | JavaScript 소스 맵에 사용되는 `string`입니다.
| `cssOutputFilename` | `null` | CSS 소스맵에 사용되는 `string`입니다.
| `sveltePath` | `"svelte"` | `svelte` 패키지의 위치입니다. 이에 따라 `svelte` 또는 `svelte/[module]`에서 가져오기가 수정됩니다.
| `namespace` | `"html"` | 요소의 네임스페이스. 예: `"mathml"`, `"svg"`, `"foreign"`.

---

반환된 `result` 개체에는 유용한 메타데이터와 함께 구성 요소에 대한 코드가 포함되어 있습니다.

```js
const {
	js,
	css,
	ast,
	warnings,
	vars,
	stats
} = svelte.compile(source);
```

* `js` and `css` are objects with the following properties:
	* `code` is a JavaScript string
	* `map` is a sourcemap with additional `toString()` and `toUrl()` convenience methods
* `ast` is an abstract syntax tree representing the structure of your component.
* `warnings` is an array of warning objects that were generated during compilation. Each warning has several properties:
	* `code` is a string identifying the category of warning
	* `message` describes the issue in human-readable terms
	* `start` and `end`, if the warning relates to a specific location, are objects with `line`, `column` and `character` properties
	* `frame`, if applicable, is a string highlighting the offending code with line numbers
* `vars` is an array of the component's declarations, used by [eslint-plugin-svelte3](https://github.com/sveltejs/eslint-plugin-svelte3) for example. Each variable has several properties:
	* `name` is self-explanatory
	* `export_name` is the name the value is exported as, if it is exported (will match `name` unless you do `export...as`)
	* `injected` is `true` if the declaration is injected by Svelte, rather than in the code you wrote
	* `module` is `true` if the value is declared in a `context="module"` script
	* `mutated` is `true` if the value's properties are assigned to inside the component
	* `reassigned` is `true` if the value is reassigned inside the component
	* `referenced` is `true` if the value is used in the template
	* `referenced_from_script` is `true` if the value is used in the `<script>` outside the declaration
	* `writable` is `true` if the value was declared with `let` or `var` (but not `const`, `class` or `function`)
* `stats` is an object used by the Svelte developer team for diagnosing the compiler. Avoid relying on it to stay the same!

* `js` 및 `css`는 다음 속성을 가진 개체입니다.
	* `code`는 JavaScript 문자열입니다.
	* `map`은 `toString()` 및 `toUrl()` 편의 메서드가 추가된 소스 맵입니다.
* `ast`는 구성 요소의 구조를 나타내는 추상 구문 트리입니다.
* `warnings`는 컴파일 중에 생성된 경고 개체의 배열입니다. 각 경고에는 여러 속성이 있습니다.
	* `code`는 경고 범주를 식별하는 문자열입니다.
	* `message`는 사람이 읽을 수 있는 용어로 문제를 설명합니다.
	* `start` 및 `end`는 경고가 특정 위치와 관련된 경우 `line`, `column` 및 `character` 속성을 가진 개체입니다.
	* `frame` 해당되는 경우, 문제가 되는 코드를 줄 번호로 강조 표시하는 문자열입니다.
* `vars`는 예를 들어 [eslint-plugin-svelte3](https://github.com/sveltejs/eslint-plugin-svelte3)에서 사용되는 구성 요소 선언의 배열입니다. 각 변수에는 여러 속성이 있습니다.
	* `name`은 자명합니다.
	* `export_name`은 값을 내보내는 경우 값을 내보내는 이름입니다(`export...as`를 수행하지 않는 한 `name`과 일치함).
	* 선언이 작성한 코드가 아니라 Svelte에 의해 삽입된 경우 `injected`는 `true`입니다.
	* `context="module"` 스크립트에서 값이 선언된 경우 `module`은 `true`입니다.
	* `mutated`는 값의 속성이 구성 요소 내부에 할당된 경우 `true`입니다.
	* 값이 구성 요소 내부에서 재할당된 경우 `reassigned`는 `true`입니다.
	* 값이 템플릿에서 사용되는 경우 `referenced`는 `true`입니다.
	* `referenced_from_script`는 값이 선언 외부의 `<script>`에서 사용되는 경우 `true`입니다.
	* 값이 `let` 또는 `var`로 선언된 경우 `writable`은 `true`입니다(그러나 `const`, `class` 또는 `function`은 아님).
* `stats`는 컴파일러 진단을 위해 Svelte 개발자 팀에서 사용하는 개체입니다. 동일하게 유지하기 위해 그것에 의존하지 마십시오!


<!--

```js
compiled: {
	// `map` is a v3 sourcemap with toString()/toUrl() methods
	js: { code: string, map: {...} },
	css: { code: string, map: {...} },
	ast: {...}, // ESTree-like syntax tree for the component, including HTML, CSS and JS
	warnings: Array<{
		code: string,
		message: string,
		filename: string,
		pos: number,
		start: { line: number, column: number },
		end: { line: number, column: number },
		frame: string,
		toString: () => string
	}>,
	vars: Array<{
		name: string,
		export_name: string,
		injected: boolean,
		module: boolean,
		mutated: boolean,
		reassigned: boolean,
		referenced: boolean,
		referenced_from_script: boolean,
		writable: boolean
	}>,
	stats: {
		timings: { [label]: number }
	}
} = svelte.compile(source: string, options?: {...})
```

-->


### `svelte.parse`

```js
ast: object = svelte.parse(
	source: string,
	options?: {
		filename?: string,
		customElement?: boolean
	}
)
```

---

`parse` 함수는 구성 요소를 구문 분석하여 추상 구문 트리만 반환합니다. `generate: false` 옵션으로 컴파일하는 것과 달리 구성 요소를 구문 분석하는 것 외에는 유효성 검사나 기타 분석을 수행하지 않습니다. 반환된 AST는 공개 API로 간주되지 않으므로 언제든지 주요 변경 사항이 발생할 수 있습니다.


```js
const svelte = require('svelte/compiler');

const ast = svelte.parse(source, { filename: 'App.svelte' });
```


### `svelte.preprocess`

TypeScript, PostCSS, SCSS 및 Less와 같은 도구와 함께 Svelte를 사용할 수 있도록 [커뮤니티에서 관리하는 전처리 플러그인](https://sveltesociety.dev/tools#preprocessors)이 많이 있습니다.

`svelte.preprocess` API를 사용하여 고유한 전처리기를 작성할 수 있습니다.

```js
result: {
	code: string,
	dependencies: Array<string>
} = await svelte.preprocess(
	source: string,
	preprocessors: Array<{
		markup?: (input: { content: string, filename: string }) => Promise<{
			code: string,
			dependencies?: Array<string>
		}>,
		script?: (input: { content: string, markup: string, attributes: Record<string, string>, filename: string }) => Promise<{
			code: string,
			dependencies?: Array<string>
		}>,
		style?: (input: { content: string, markup: string, attributes: Record<string, string>, filename: string }) => Promise<{
			code: string,
			dependencies?: Array<string>
		}>
	}>,
	options?: {
		filename?: string
	}
)
```

---

`preprocess` 기능은 컴포넌트 소스 코드를 임의로 변환하기 위한 편리한 후크를 제공합니다. 예를 들어 `<style lang="sass">` 블록을 바닐라 CSS로 변환하는 데 사용할 수 있습니다.

첫 번째 인수는 구성 요소 소스 코드입니다. 두 번째는 *preprocessors*의 배열(또는 하나뿐인 경우 단일 전처리기)입니다. 여기서 전처리기는 `markup`, `script` 및 `style` 기능이 있는 개체이며 각각은 선택 사항입니다.

각 `마크업`, `스크립트` 또는 `style` 함수는 변환된 소스 코드를 나타내는 `code` 속성과 선택적 `dependencies` 배열이 있는 객체(또는 객체로 확인되는 약속)를 반환해야 합니다.

`markup` 함수는 세 번째 인수에 지정된 경우 구성 요소의 `filename`과 함께 전체 구성 요소 소스 텍스트를 받습니다.

> 전처리기 함수는 `code` 및 `dependencies`와 함께 `map` 개체를 추가로 반환해야 합니다. 여기서 `map`은 변환을 나타내는 소스 맵입니다.

```js
const svelte = require('svelte/compiler');
const MagicString = require('magic-string');

const { code } = await svelte.preprocess(source, {
	markup: ({ content, filename }) => {
		const pos = content.indexOf('foo');
		if(pos < 0) {
			return { code: content }
		}
		const s = new MagicString(content, { filename })
		s.overwrite(pos, pos + 3, 'bar', { storeName: true })
		return {
			code: s.toString(),
			map: s.generateMap()
		}
	}
}, {
	filename: 'App.svelte'
});
```

---

`script` 및 `style` 함수는 각각 `<script>` 및 `<style>` 요소의 내용(`content`)과 전체 구성 요소 소스 텍스트(`markup`)를 수신합니다. `filename` 외에도 요소 속성의 개체를 얻습니다.

`dependencies` 배열이 반환되면 결과 객체에 포함됩니다. 이것은 [rollup-plugin-svelte](https://github.com/sveltejs/rollup-plugin-svelte)와 같은 패키지에서 `<style>` 태그에 `@import`가 있는 경우(예:) 추가 파일에서 변경 사항을 감시하는 데 사용됩니다.

```js
const svelte = require('svelte/compiler');
const sass = require('node-sass');
const { dirname } = require('path');

const { code, dependencies } = await svelte.preprocess(source, {
	style: async ({ content, attributes, filename }) => {
		// only process <style lang="sass">
		if (attributes.lang !== 'sass') return;

		const { css, stats } = await new Promise((resolve, reject) => sass.render({
			file: filename,
			data: content,
			includePaths: [
				dirname(filename),
			],
		}, (err, result) => {
			if (err) reject(err);
			else resolve(result);
		}));

		return {
			code: css.toString(),
			dependencies: stats.includedFiles
		};
	}
}, {
	filename: 'App.svelte'
});
```

---

여러 전처리기를 함께 사용할 수 있습니다. 첫 번째의 출력은 두 번째의 입력이 됩니다. `markup` 함수가 먼저 실행된 다음 `script` 및 `style` 함수가 실행됩니다.

```js
const svelte = require('svelte/compiler');

const { code } = await svelte.preprocess(source, [
	{
		markup: () => {
			console.log('this runs first');
		},
		script: () => {
			console.log('this runs third');
		},
		style: () => {
			console.log('this runs fifth');
		}
	},
	{
		markup: () => {
			console.log('this runs second');
		},
		script: () => {
			console.log('this runs fourth');
		},
		style: () => {
			console.log('this runs sixth');
		}
	}
], {
	filename: 'App.svelte'
});
```


### `svelte.walk`

```js
walk(ast: Node, {
	enter(node: Node, parent: Node, prop: string, index: number)?: void,
	leave(node: Node, parent: Node, prop: string, index: number)?: void
})
```

---

`walk` 함수는 컴파일러 자체 내장 [estree-walker](https://github.com/Rich-Harris/estree-walker) 인스턴스를 사용하여 파서가 생성한 추상 구문 트리를 탐색하는 방법을 제공합니다. .

walker는 걷기 위한 추상 구문 트리와 `enter` 및 `leave`라는 두 가지 선택적 메소드가 있는 객체를 사용합니다. 각 노드에 대해 `enter`가 호출됩니다(있는 경우). 그러면 `enter` 중에 `this.skip()`이 호출되지 않으면 각 자식을 순회한 다음 노드에서 `leave`가 호출됩니다.


```js
const svelte = require('svelte/compiler');
svelte.walk(ast, {
	enter(node, parent, prop, index) {
		do_something(node);
		if (should_skip_children(node)) {
			this.skip();
		}
	},
	leave(node, parent, prop, index) {
		do_something_else(node);
	}
});
```


### `svelte.VERSION`

---

package.json에 설정된 현재 버전입니다.

```js
const svelte = require('svelte/compiler');
console.log(`running svelte version ${svelte.VERSION}`);
```
