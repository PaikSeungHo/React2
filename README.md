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

- Route: 경로를 의미하며, 특정 URL과 컴포넌트를 연결하는 역할을 한다.
- Routing: 사용자가 요청한 URL에 따라 해당 URL에 맞는 페이지를 보여주는 과정이다.
- Routes: 여러 개의 Route를 묶어주는 역할을 하며, 기존의 switch와 같은 기능을 한다.
- Routes를 사용하지 않고 Route만 나열하면 조건에 맞는 모든 Route가 렌더링된다.

실제 화면 전환의 핵심 역할은 Route가 담당한다.

◦ Router
- 리액트는 SPA (Single Page Application) 구조를 기반으로 하며, 하나의 index.html에서 여러 화면을 전환한다.
- 이러한 화면 전환을 담당하는 것이 **라우팅 (Routing)**이며, 이를 쉽게 도와주는 라이브러리가 React Router이다.

React Router 설치

npm install react-router-dom

설치 후 package.json의 dependencies에 react-router-dom이 추가된다.

◦ 라우터 기본 설정
라우터를 사용하려면 프로젝트 최상위 컴포넌트를 BrowserRouter로 감싸야 한다.

// main.jsx
```
import React from "react";
import ReactDOM from "react-dom/client";
import { BrowserRouter } from "react-router-dom";
import App from "./App";

ReactDOM.createRoot(document.getElementById("root")).render(
  <BrowserRouter>
    <App />
  </BrowserRouter>
);
```

◦ 라우팅 구현하기
페이지 이동은 Routes와 Route 컴포넌트를 사용한다.

// App.jsx
```
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
```

◦ 주요 기능

- <Link>: a 태그와 비슷하지만 새로고침 없이 컴포넌트 전환이 가능하다.
- 중첩 라우트 (Nested Routes): 하나의 페이지 안에서 하위 라우트를 구성할 수 있다.
- useParams(): URL 파라미터를 가져올 수 있다. (예: /user/:id)
- useNavigate(): 코드로 페이지 이동을 제어할 수 있다.

◦ 정리

- React Router는 SPA 환경에서 화면 전환을 담당한다.
- BrowserRouter → 프로젝트에 라우팅 기능을 부여한다.
- Routes & Route → 경로에 따른 컴포넌트 매핑을 처리한다.
- Link, useNavigate, useParams 등 다양한 기능을 통해 유연한 화면 전환이 가능하다.


## 4주차 수업

Git: checkout vs switch

- 과거에는 git checkout 하나로 브랜치 이동, 브랜치 생성, 커밋 이동까지 모두 처리했으나 기능이 많아 혼란을 주었다.

브랜치 이동

git checkout feature/login
git switch feature/login


브랜치 생성 + 이동

git checkout -b feature/signup
git switch -c feature/signup


특정 커밋으로 이동

git checkout <commit-id>


특징

- checkout은 다기능이지만 복잡하고 실수 위험이 있음
- switch는 브랜치 전용 명령어라 더 직관적이고 안전함
- 최신 Git 환경에서는 switch 사용을 권장
- branch 명령어는 브랜치 생성·삭제·확인 전용

◦ Layout (레이아웃)

- 웹사이트는 공통적으로 반복되는 구조(헤더, 사이드바, 푸터 등)가 있다.

- React에서는 이 구조를 레이아웃 컴포넌트로 만들고 각 페이지를 그 안에 넣어 재사용한다.

예시 구조:

Header
 └─ Main Content (변경되는 부분)
Footer

// Layout.jsx
```
import Header from "./Header";
import Footer from "./Footer";

function Layout({ children }) {
  return (
    <div>
      <Header />
      <main style={{ minHeight: "80vh" }}>
        {children} {/* 페이지별 내용 */}
      </main>
      <Footer />
    </div>
  );
}

export default Layout;
```

◦ 라우터와 함께 사용하기
공통 레이아웃을 Route에 적용하여 페이지를 감싼다.

// App.jsx
```
import { Routes, Route } from "react-router-dom";
import Layout from "./components/Layout";
import Home from "./pages/Home";
import About from "./pages/About";

function App() {
  return (
    <Routes>
      <Route path="/" element={<Layout />}>
        <Route index element={<Home />} />
        <Route path="about" element={<About />} />
      </Route>
    </Routes>
  );
}

export default App;
```

◦ Outlet 사용 예시
Outlet은 라우터에서 지정된 페이지를 레이아웃 안에 삽입한다.

// Layout.jsx
```
import { Outlet } from "react-router-dom";
import Header from "./Header";
import Footer from "./Footer";

function Layout() {
  return (
    <div>
      <Header />
      <main>
        <Outlet /> {/* 라우터에서 지정된 페이지 */}
      </main>
      <Footer />
    </div>
  );
}

export default Layout;
```

◦ 스타일링 방법

- CSS 모듈: Layout.module.css로 분리
- Styled-components: JS 안에서 스타일 정의
- Tailwind CSS: 클래스명으로 빠르게 레이아웃 구성

◦ 레이아웃 정리

- 레이아웃은 공통 구조(헤더, 푸터, 사이드바)를 관리하는 컴포넌트
- children 또는 Outlet을 활용해 페이지를 삽입
- 반복되는 코드를 줄이고 유지보수를 쉽게 만든다
- 루트 레이아웃은 필수이며 html 및 body 태그를 포함해야 한다
- 중첩 라우트는 다중 URL 세그먼트로 구성된다

◦ Slug의 이해

- Slug는 URL이나 데이터에서 고유 식별자 역할을 한다.
- 데이터 소스가 크다면 단순 .find는 시간 복잡도 **O(n)**이므로 비효율적 → DB 쿼리로 대체하는 것이 좋다.
- O(n): 입력 데이터 크기 n에 비례하여 실행 시간이 선형적으로 증가한다는 의미.


## 5주차 수업

CSS 스타일링 방법
◦ CSS 모듈

- Layout.module.css처럼 파일명을 지어 스타일을 분리합니다.
- -> 클래스명이 고유해져서 스타일 충돌을 방지할 수 있습니다.

◦ Styled-components

- JS 파일 안에서 컴포넌트 단위로 스타일을 바로 정의하는 방식입니다. (CSS-in-JS)

◦ Tailwind CSS

- flex, p-4, text-blue-500 같은 미리 정의된 유틸리티 클래스명을 사용합니다.
- -> HTML/JSX 내에서 빠르고 간편하게 레이아웃을 구성할 수 있습니다.

레이아웃(Layout) 정리
◦ 레이아웃이란?

- -> 여러 페이지에서 공통적으로 사용되는 구조(헤더, 푸터, 사이드바 등)를 관리하는 컴포넌트입니다.

◦ 페이지 삽입 방식

- -> children 프롭(Props)이나 Outlet(React Router)을 활용해 실제 페이지 콘텐츠를 삽입합니다.

◦ 장점

- -> 반복되는 코드를 줄여서 유지보수성을 높입니다.

◦ 루트 레이아웃 (App Router 기준)

- -> 모든 페이지를 감싸는 최상위 레이아웃으로, <html>과 <body> 태그를 반드시 포함해야 합니다.

◦ 중첩 라우트 (Nested Routes)

- -> /dashboard/settings처럼 여러 URL 세그먼트(조각)로 구성된 경로 구조를 의미합니다.


Slug의 이해
◦ Slug란?
- -> URL이나 데이터에서 고유 식별자 역할을 하는 값입니다.
- -> (예: .../posts/my-first-post 에서 my-first-post 부분)

◦ 성능과 O(n)

- 데이터 소스(배열 등)가 클 때, 단순 .find() 메서드로 데이터를 찾는 것은 비효율적일 수 있습니다.
- -> 시간 복잡도 $O(n)$: 입력 데이터 크기(n)에 비례하여 실행 시간이 선형적으로 증가한다는 의미입니다. (데이터 1000개면 최대 1000번 비교)
- -> 개선: 이런 경우, DB 쿼리(인덱싱 활용) 등으로 대체하여 $O(n)$보다 효율적으로 데이터를 찾는 것이 좋습니다.



## 6주차 수업

Client-side Transitions (클라이언트 측 전환)

◦ 기존 서버 렌더링의 문제점

- 페이지 이동 시, 브라우저가 **전체 페이지를 새로 로드(Full Page Load)**합니다.
- 이로 인해 기존 페이지의 **클라이언트 상태(state)**가 모두 삭제됩니다.
- 스크롤 위치가 항상 맨 위로 재설정됩니다.
- 페이지를 로드하는 동안 사용자의 다른 상호작용이 잠시 차단됩니다.

◦ Next.js의 <Link> 컴포넌트

- Next.js는 <Link> 컴포넌트를 통해 클라이언트 측 전환을 제공하여 이 문제를 방지합니다.
- 페이지 이동 시, 내비게이션 바나 푸터 같은 공유 레이아웃과 UI를 그대로 유지시킵니다.
- 변경되는 부분만 새로 가져와 교체하므로 SPA(싱글 페이지 앱)처럼 부드러운 경험을 제공합니다.

◦ 성능 (Prefetching)

- Next.js는 <Link>가 가리키는 페이지를 미리 가져와(prefetching) 로딩을 대비합니다.
- 프리페칭, 스트리밍 기능과 함께 사용하면 동적 경로에서도 매우 빠른 페이지 전환이 가능합니다.


generateStaticParams

◦ 역할
- Next.js의 동적 라우트([slug])에서 빌드 시점에 미리 생성해 둘 페이지(HTML) 목록을 알려주는 함수입니다.


◦ 작동 방식
- export async function generateStaticParams()로 함수를 export하면, Next.js가 빌드 시점에 이 함수를 호출합니다.
- 함수는 [{ slug: '1' }, { slug: '2' }]와 같이 경로 파라미터(slug) 객체들의 배열을 반환해야 합니다.

// data.jsx
```
export const posts = [
  { id: "1", title: "첫 번째 글", content: "내용 1" },
  { id: "2", title: "두 번째 글", content: "내용 2" }];

```

// page.jsx
```
import { posts } from "./data";

interface BlogPageProps {
  params: { slug: string };
}

export async function generateStaticParams() {
  return posts.map(post => ({
    slug: post.id,
  }));
}

export default function BlogPage({ params }: BlogPageProps) {
  const post = posts.find(p => p.id === params.slug);

  if (!post) return <p>글을 찾을 수 없습니다.</p>;

  return (
    <div>
      <h1>{post.title}</h1>
      <p>{post.content}</p>
    </div>
  );
}

```

## 7주차 수업

### 시험

## 8주차 수업

### 병결


















