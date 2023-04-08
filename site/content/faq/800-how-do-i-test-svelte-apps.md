---
question: How do I test Svelte apps?
---

뷰 로직을 비즈니스 로직과 분리하는 것이 좋습니다. 데이터 변환 또는 교차 구성 요소 상태 관리는 Svelte 구성 요소 외부에서 가장 잘 유지됩니다. 그런 식으로 JavaScript 기능을 테스트하는 것처럼 해당 부분을 테스트할 수 있습니다. 구성 요소를 테스트할 때 구성 요소의 논리를 테스트하는 것이 가장 좋습니다. Svelte 라이브러리에는 자체 테스트가 있으며 Svelte에서 제공하는 구현 세부 정보를 테스트할 필요가 없습니다.

사람들이 테스트할 때 취하는 몇 가지 접근 방식이 있지만 일반적으로 구성 요소를 컴파일하고 이를 무언가에 마운트한 다음 테스트를 수행하는 것이 포함됩니다. 기본적으로 테스트 중인 각 구성 요소에 대한 번들을 만든 다음(svelte는 일반 라이브러리가 아닌 컴파일러이므로) 마운트해야 합니다. JSDOM 인스턴스에 마운트할 수 있습니다. 또는 Playwright, Puppeteer, WebdriverIO 또는 Cypress와 같은 라이브러리에서 제공하는 실제 브라우저를 사용할 수 있습니다.

유닛 테스트를 시작하기 위한 일부 리소스:
- [Svelte Testing Library](https://testing-library.com/docs/svelte-testing-library/example/)
- [Example using vitest](https://github.com/vitest-dev/vitest/tree/main/examples/svelte)
- [Example using uvu test runner with JSDOM](https://github.com/lukeed/uvu/tree/master/examples/svelte)
- [Component testing in real browser](https://webdriver.io/docs/component-testing/svelte)
