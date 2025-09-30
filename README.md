# React2

## 2주차 수업
Installation (설치 관련)

VS Code에서 명령 팔레트 (Ctrl+Shift+P) → TypeScript: TypeScript 버전 선택 → 워크스페이스 버전 선택

Next.js에는 ESLint가 기본 내장되어 있음

기존 프로젝트에 직접 추가할 경우 package.json에 아래 스크립트를 넣으면 됨:
```
{
  "scripts": {
    "lint": "next lint"
  }
}
```

ESLint 설정
create-next-app으로 프로젝트를 생성하면 자동으로 ESLint와 설정이 추가됨

- Strict / Base 설정 중 하나를 선택 가능
- Strict: Core Web Vitals 규칙까지 적용 → 초보자 권장
- Base: 기본 ESLint 구성만 포함

ESLint 설정 방식

- .eslintrc.json : JSON 형식이라 주석, 조건문 불가 → 단순한 설정에 적합
- eslint.config.mjs : JS 모듈 방식, 변수·조건문·동적 로딩 가능 → 유지보수에 유리 (Next.js 14부터 권장)

// .eslintrc.json
```
{
  "extends": "next",
  "rules": {
    "no-console": "warn"
  }
}
```
// eslint.config.mjs
```
import next from 'eslint-config-next';

export default [
  next(),
  {
    rules: {
      'no-console': 'warn',
    },
  },
];
```

import 및 절대 경로 별칭 설정

tsconfig.json 또는 jsconfig.json에서 설정 가능
baseUrl과 paths를 사용하여 import 경로 단축 가능

```
{
  "compilerOptions": {
    "baseUrl": "src/",
    "paths": {
      "@/components/*": ["components/*"],
      "@/styles/*": ["styles/*"]
    }
  }
}
```

예시

// Before
 import Button from "../../../components/button" 
// After
 import Button from "@/components/button" 

Core Web Vitals

- LCP (Largest Contentful Paint): 뷰포트 안에서 가장 큰 요소가 표시되는 시간
- FID (First Input Delay): 사용자가 처음 상호작용을 시도한 순간부터 페이지가 응답하는 시간
- CLS (Cumulative Layout Shift): 화면 레이아웃이 갑자기 움직이는 빈도

프로젝트 생성 시 선택 항목

- 프로젝트 이름 입력
- TypeScript, ESLint, Tailwind 사용 여부
- src/ 디렉토리 구조 선택
- App Router 사용 여부
- import alias 사용 여부 (@/* 기본값)

pnpm (패키지 매니저)
npm, yarn보다 빠르고 효율적인 패키지 관리자
전역 저장소에 한 번만 설치 후 각 프로젝트에는 하드 링크 방식으로 연결

주요 명령어:

+ cpnpm install → node_modules 설치
+ pnpm add [패키지명] → 새 패키지 추가
+ pnpm remove [패키지명] → 패키지 제거
+ pnpm update → 업데이트
+ pnpm list → 설치된 패키지 확인

Hard Link vs Symbolic Link

Hard Link

* 동일한 inode를 참조 → 원본과 링크가 완전히 같은 파일
* 하나를 삭제해도 inode 카운트가 0이 아니면 데이터 유지됨
* pnpm은 하드 링크를 이용해서 저장 공간 절약

Symbolic Link (Soft Link)

- 경로 문자열만 저장 → 원본 파일을 참조
- 원본이 삭제되면 링크는 끊어짐
- 윈도우의 "바로가기"와 비슷한 개념


## 3주차 수업

◦ Route / Routing / Routes

Route: 경로를 의미하며, 특정 URL과 컴포넌트를 연결하는 역할을 한다.
Routing: 사용자가 요청한 URL에 따라 해당 URL에 맞는 페이지를 보여주는 과정이다.
Routes: 여러 개의 Route를 묶어주는 역할을 하며, 기존의 switch와 같은 기능을 한다.
Routes를 사용하지 않고 Route만 나열하면 조건에 맞는 모든 Route가 렌더링된다.

실제 화면 전환의 핵심 역할은 Route가 담당한다.

◦ Router
리액트는 SPA (Single Page Application) 구조를 기반으로 하며, 하나의 index.html에서 여러 화면을 전환한다.
이러한 화면 전환을 담당하는 것이 **라우팅 (Routing)**이며, 이를 쉽게 도와주는 라이브러리가 React Router이다.

React Router 설치

npm install react-router-dom

설치 후 package.json의 dependencies에 react-router-dom이 추가된다.

◦ 라우터 기본 설정
라우터를 사용하려면 프로젝트 최상위 컴포넌트를 BrowserRouter로 감싸야 한다.

// main.jsx
import React from "react";
import ReactDOM from "react-dom/client";
import { BrowserRouter } from "react-router-dom";
import App from "./App";

ReactDOM.createRoot(document.getElementById("root")).render(
  <BrowserRouter>
    <App />
  </BrowserRouter>
);


◦ 라우팅 구현하기
페이지 이동은 Routes와 Route 컴포넌트를 사용한다.

// App.jsx
import { Routes, Route, Link } from "react-router-dom";
import Home from "./pages/Home";
import About from "./pages/About";

function App() {
  return (
    <div>
      <nav>
        <Link to="/">홈</Link>
        <Link to="/about">소개</Link>
      </nav>

      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
      </Routes>
    </div>
  );
}

export default App;


◦ 주요 기능

<Link>: a 태그와 비슷하지만 새로고침 없이 컴포넌트 전환이 가능하다.
중첩 라우트 (Nested Routes): 하나의 페이지 안에서 하위 라우트를 구성할 수 있다.
useParams(): URL 파라미터를 가져올 수 있다. (예: /user/:id)
useNavigate(): 코드로 페이지 이동을 제어할 수 있다.

◦ 정리

React Router는 SPA 환경에서 화면 전환을 담당한다.
BrowserRouter → 프로젝트에 라우팅 기능을 부여한다.
Routes & Route → 경로에 따른 컴포넌트 매핑을 처리한다.
Link, useNavigate, useParams 등 다양한 기능을 통해 유연한 화면 전환이 가능하다.