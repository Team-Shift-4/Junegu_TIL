# Tailwindcss

## **Tailwind**

### Tailwind란?

tailwind는 유틸리티 우선(Utility-First) css 프레임워크로 불립니다.

유틸리티 클래스는 버튼. 모달 등과 같이 주변을 구성하기 위한 css가 아닌 **빨간색 글자 색상, xl 사이즈의 글자 크기 등과 하나의 css에 정의된 속성과 값**을 가지는 걸 의미한다.

유틸리티 우선이라고 하는 것은 bootstrap처럼 기존에 정의된 값을 가져와 사용하는 것이 아닌 사용자가 원하는 대로 각각의 속성값을 적용하는 방식이라고 말할 수 있다.

```css
// css
.container {
    border: 2px solid black;
    background-color: #3B82F6;
    height: 2.5rem;
}

<main>
	<div className=".container" />
</main>

// tailwind
<main>
	<div className="border-2 border-solid border-black bg-blue-500 h-10" />
</main>
```

## Tailwind 특징

### 장점

Tailwind는 여러 가지 장점을 가지고 있지만, 개인적으로 크게 강조하고 싶은 두 가지 장점이 있습니다.

#### 1. 작은 Bundle 사이즈

Tailwind를 사용하면 CSS를 적용하기 위해 별도의 .css, .scss 파일이나 CSS-in-JS를 위한 컴포넌트를 만들 필요가 없습니다. 코드를 확인하시거나 Tailwind를 사용해보면 알겠지만, 동일한 스타일을 서로 다른 컴포넌트에 적용할 때 해당 스타일을 적용하는 소스가 모두 재사용되기 때문에 Bundle 사이즈가 작아지게 됩니다.

#### 2. 네이밍에 대한 고민이 필요 없음

코드를 작성할 때 특정 기능에 대한 네이밍은 누구나 어려움을 겪었을 것입니다. Tailwind는 특별한 네이밍 없이 클래스에 원하는 스타일을 입힐 수 있어 빠른 속도로 개발할 수 있게 도와줍니다. 또한, 팀 내에서 네이밍 컨벤션을 정하는 데에 따른 혼란을 최소화하여 협업에 도움이 됩니다.

### 단점

Tailwind는 다른 기술들과 마찬가지로 단점을 가지고 있습니다. 개인적으로 가장 크게 중요하게 생각하는 단점은 두 가지입니다.

#### **1. HTML 코드의 장황함과 복잡성**

Tailwind를 사용할 때 모든 요소에 스타일을 적용하기 위해서는 지속적으로 class에 값을 추가해야 합니다. 이로 인해 10개 이상의 스타일을 적용할 때마다 코드가 어지러워지고 읽기 어려운 경험이 발생할 수 있습니다.

#### **2. 목적 구분의 어려움**

Tailwind는 네이밍을 사용하지 않는 장점으로 소개되었습니다. 그러나 이로 인해 코드의 목적을 파악하기가 어려워집니다. 빠른 스타일 적용은 개발 속도를 향상시킬 수 있지만, 코드를 수정하거나 유지보수할 때 명확한 네이밍이 없어 세밀한 분석이 필요할 수 있습니다. 주석을 적절히 활용하면 이러한 단점을 어느 정도 보완할 수 있습니다.

이러한 단점들을 고려하면서 Tailwind를 사용할 때는 코드의 가독성과 유지보수성을 위해 주의가 필요합니다.

## Tailwind 사용

#### 1. 패키지 설치

```jsx
$ npm install -D tailwindcss postcss autoprefixer style-loader css-loader postcss-loader
```

#### 2. webpack 설정

```jsx
module.exports = {
    // 로더 등록
    module: {
        rules: [
            {
                test: /\\.css?$/,
                use: ['style-loader', 'css-loader', 'postcss-loader'],
                exclude: ['/node_modules/'],
            },
        ],
    },
};
```

#### 3. tailwind 초기화

```jsx
$ npx tailwindcss init
```

명령어를 입력하면 tailwind 설정 파일인 tailwind.config.js 파일이 생성되는 것을 확인할 수 있다.

```jsx
// tailwind.config.js

/** @type {import('tailwindcss').Config} */
module.exports = {
    content: [],
    theme: {
        extend: {},
    },
    plugins: [],
};
```

#### 4. content 설정

생성된 tailwind.config.js 파일을 보면 content 라는 설정 값이 있다.

content에는 tailwind를 적용할 파일 경로를 넣어주면 되고 일반적으로 다음과 같이 넣어주면 모든 파일에 적용이 된다.

```jsx
/** @type {import('tailwindcss').Config} */
module.exports = {
    content: ['./src/**/*.{js,ts,jsx,tsx}'],
    theme: {
        extend: {},
    },
    plugins: [],
};
```

#### 5. postcss 설정

root 경로에 postcss.config.js 파일을 생성한 뒤 다음과 같이 코드를 작성하면 된다.

```jsx
module.exports = {
    plugins: {
        tailwindcss: {},
        autoprefixer: {},
    },
};
```

#### 6. main.css 설정

```jsx
@tailwind base;
@tailwind components;
@tailwind utilities;
```

해당 파일을 index 파일 등에 추가하여 전역적으로 적용될 수 있도록 한다.

```jsx
// index.tsx

import React from 'react';
import ReactDom from 'react-dom';
import App from './App';
import './main.css';

ReactDom.render(<App />, document.querySelector('#root'));
```

## VSCode 플러그인

### 1. Tailwind CSS IntelliSense

<figure><img src="../../.gitbook/assets/image (111).png" alt=""><figcaption></figcaption></figure>

해당 플러그인은 tailwind의 자동완성을 할 수 있도록 도와줍니다.

기존 css작성 방식과 달리 사용되는 tailwind를 처음 사용할 때 IntelliSense는 코드 작성에 많은 도움을 줍니다.

### 2. Tailwind CSS Highlight Intellisense

<figure><img src="../../.gitbook/assets/image (112).png" alt=""><figcaption></figcaption></figure>

해당 플러그인은 tailwind가 적용된 class가 있을 경우 점선으로 표시를 해줍니다.

코드에 tailwind가 적용되어 있는지 확인 가능하며 또한 동일 class안에 여러 개의 스타일이 적용되었을 때 각각의 스타일을 구분하여 읽을 수 있도록 도와줍니다.
