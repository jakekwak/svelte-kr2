---
question: What about TypeScript support?
---

[svelte-preprocess](https://github.com/sveltejs/svelte-preprocess)와 같은 전처리기를 설치해야 합니다. [svelte-check](https://www.npmjs.com/package/svelte-check)를 사용하여 명령줄에서 유형 검사를 실행할 수 있습니다.

Svelte 템플릿에서 반응 변수의 유형을 선언하려면 다음 구문을 사용해야 합니다.

```ts
let x: number;
$: x = count + 1;
```

유형이나 인터페이스를 가져오려면 [TypeScript의 `type` 모디파이어](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-8.html#type-only-imports-and-export)를 사용해야 합니다.

```ts
import type { SomeInterface } from './SomeFile';
```

`svelte-preprocess`는 가져오기가 유형인지 값인지 알지 못하므로 `type` 모디파이어를 사용해야 합니다. — 다른 파일에 대한 지식 없이 한 번에 하나의 파일만 변환하므로 이 모디파이어가 없는 유형만 포함하는 가져오기를 안전하게 지울 수 없습니다.
