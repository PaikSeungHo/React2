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

