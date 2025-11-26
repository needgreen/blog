## 1. Next.js

React 기반 풀스택 웹 프레임워크

### 1.1. Next.js 정의

Next.js는 **Vercel이 만든, React 기반의 풀스택 웹 프레임워크**이다.

- **React (Library):** 개발자가 필요한 도구(라우터, 상태 관리 등)를 직접 선택하고 조립해야 하는 '도구 모음'이다. 자유도가 높지만 그만큼 설정해야 할 것이 많다.
- **Next.js (Framework):** 웹 개발에 필요한 필수 기능(라우팅, 번들링, 최적화 등)이 미리 조립되어 정해진 규칙에 따라 작동하는 '완성된 체계'이다.

> **제어의 역전 (Inversion of Control)**
>
> 라이브러리는 우리가 코드를 호출하지만, 프레임워크는 프레임워크가 우리의 코드(파일, 함수)를 호출한다. Next.js가 정한 규칙(파일명, 폴더 구조)을 따라야만 앱이 작동하는 이유이다.

---

### 1.2. React만으로 충분하지 않은 이유

현업에서 단순 React(SPA)만으로 서비스를 운영할 때 마주하는 **치명적인 한계점**들이 있다.

#### 1. SEO (검색 엔진 최적화)의 취약성

React는 초기에 빈 HTML(`<div id="root"></div>`)만 로드한다. 구글을 제외한 대부분의 검색 봇(네이버, 카카오 등)은 해당 페이지의 정확한 내용(컨텐츠)을 읽지 못해 검색 결과 상위 노출이 어렵다.

#### 2. 느린 초기 로딩 속도 (TTV: Time To View)

React는 CSR(Client Side Rendering)이다. 빈 HTML을 받고 방대한 JavaScript 파일이 다 다운로드되고 실행될 때까지 흰 화면만 봐야 한다. 인터넷이 느린 환경에서는 치명적이다.

<details>
<summary><strong>CSR (Client-Side Rendering)</strong></summary>

**클라이언트 사이드 렌더링**은 브라우저에서 JavaScript를 실행하여 페이지를 렌더링하는 방식이다.

**장점**

- 동적 인터랙션 가능
- SPA(Single Page Application) 구현 가능
- 서버 부하 감소
- 풍부한 인터랙티브 경험 제공 가능

**단점**

- 초기 로딩 시간이 길 수 있음
- SEO에 불리할 수 있음
- JavaScript가 비활성화된 환경에서 작동하지 않음

> **대표적인 사례:** React, Vue, Angular 기반 애플리케이션

</details>

#### 3. 개발 복잡도 증가

라우팅(`react-router-dom`), 이미지 최적화, 코드 스플리팅 등을 개발자가 일일이 수동으로 설정해야 하므로 비즈니스 로직보다 환경 설정에 쏟는 시간이 많아진다.

---

### 1.3. Next.js의 핵심 해결책

Next.js는 위의 문제들을 **프레임워크 차원에서 해결**한다.

#### 1. 다양한 렌더링 방식 (SSR / SSG / ISR)

서버에서 미리 HTML을 완성해서 보내주므로(Pre-rendering), 검색 엔진이 내용을 완벽하게 읽을 수 있고 초기 로딩 속도가 매우 빠르다. 페이지별로 **SSR**(서버 사이드 렌더링), **SSG**(정적 사이트 생성), **ISR**(증분 정적 재생성)을 선택하여 적용할 수 있다.

<details>
<summary><strong>SSR (Server-Side Rendering)</strong></summary>

**서버 사이드 렌더링**은 서버에서 완성된 HTML을 클라이언트에 전송하는 방식이다.

**장점**

- 빠른 초기 로딩
- SEO 친화적 (검색 엔진 최적화)
- 모든 디바이스/브라우저에서 기본적인 콘텐츠 접근 가능

**단점**

- 서버 부하 증가
- 동적 기능 구현 복잡
- 페이지 요청마다 서버 작업 필요

> **대표적인 사례:** Next.js, Nuxt.js, 서버 기반 PHP/Ruby 등

</details>

<details>
<summary><strong>SSG (Static Site Generation)</strong></summary>

**정적 사이트 생성**은 빌드 시점에 모든 페이지를 미리 생성하는 방식이다.

**장점**

- 매우 빠른 로딩 속도
- 서버 비용 절약
- 높은 안정성
- 보안 강화 (서버 측 로직 노출 감소)
- 확장성 및 안정성 우수

**단점**

- 동적 콘텐츠 처리 제한
- 빌드 시간 증가
- 자주 변경되는 콘텐츠에 적합하지 않음

> **대표적인 사례:** Gatsby, Next.js(정적 생성 모드), Jekyll

</details>

<details>
<summary><strong>ISR (Incremental Static Regeneration)</strong></summary>

**점진적 정적 재생성**은 정적 페이지를 주기적으로 업데이트하는 방식이다.

**장점**

- 빠른 로딩 + 최신 콘텐츠
- SEO 친화적
- 서버 부하 최소화
- SSG의 성능 이점 유지
- 데이터 업데이트 반영 가능

**단점**

- 복잡한 구현
- 캐시 관리 필요
- 즉각적인 콘텐츠 업데이트가 필요한 경우 적합하지 않음

> **대표적인 사례:** Next.js의 ISR 기능

</details>

#### 2. 파일 시스템 기반 라우팅

복잡한 라우터 설정 코드 없이, 폴더와 파일을 만드는 것만으로 자동으로 경로가 생성된다.

#### 3. 자동 최적화 (Zero Config)

이미지 용량 최적화, 폰트 최적화, 스크립트 로딩 최적화 기능이 내장되어 있어 성능 점수(Lighthouse)를 쉽게 높일 수 있다.

---

### React vs Next.js 비교

| **비교 항목**      | **React**                          | **Next.js**                                        |
| ------------------ | ---------------------------------- | -------------------------------------------------- |
| **분류**           | 라이브러리 (Library)               | 프레임워크 (Framework)                             |
| **주 렌더링**      | **CSR** (Client Side Rendering)    | **SSR, SSG, ISR** + CSR                            |
| **SEO (검색노출)** | 불리함 (추가 설정 필요)            | **매우 유리함** (기본 지원)                        |
| **라우팅**         | `react-router-dom` 별도 설치       | **파일 시스템 기반** (폴더=경로)                   |
| **백엔드 기능**    | 불가능 (별도 서버 필요)            | **Route Handlers** (API 서버 기능 내장)            |
| **초기 로딩**      | JS 다운로드 후 화면 표시 (느림)    | 완성된 HTML 즉시 표시 (빠름)                       |
| **추천 대상**      | 관리자 페이지, 사내 도구, 대시보드 | **이커머스, 블로그, 마케팅 페이지, 대국민 서비스** |

---

## 2. 개발 환경 구축 및 아키텍처

### 2.1. 프로젝트 생성 및 설정 (Setup)

#### 1. 설치 명령어

터미널에서 아래 명령어를 실행하여 최신 버전의 Next.js 프로젝트를 생성한다.

```bash
npx create-next-app@latest
```

#### 2. 핵심 설치 옵션 가이드

실무 표준인 **TypeScript**와 최신 기술인 **App Router**를 기반으로 설정한다.

| **질문 (Prompt)**                                                | **의미**                 | **설명**                                                                               | **추천 선택** |
| ---------------------------------------------------------------- | ------------------------ | -------------------------------------------------------------------------------------- | ------------- |
| **What is your project name?**                                   | 프로젝트 폴더명          | 영문 소문자와 대시(-) 권장                                                             |               |
| **Would you like to use TypeScript?**                            | 타입스크립트 사용 여부   | 강력한 타입 안정성을 위해 TypeScript를 사용할껀지에 대한 여부                          | **Yes / No**  |
| **Would you like to use ESLint?**                                | 코드 린팅 도구 사용 여부 | 코드의 잠재적 오류와 스타일을 잡아주는 도구                                            | **Yes**       |
| **Would you like to use Tailwind CSS?**                          | CSS 프레임워크 사용 여부 | 빠른 스타일링을 원하면 Yes                                                             | **Yes / No**  |
| **Would you like to use `src/` directory?**                      | 소스 폴더 분리 여부      | 프로젝트 루트에 설정 파일들이 많아지므로, 코드는 `src` 폴더 안에 모아두는 것이 깔끔함  | **Yes**       |
| **Would you like to use App Router?**                            | 앱 라우터 사용 여부      | Next.js 13버전 이후의 새로운 표준. 기존 `Pages Router` 방식보다 훨씬 강력한 기능 제공  | **Yes**       |
| **Would you like to customize the default import alias (@/\*)?** | 경로 별칭 사용 여부      | `../../../components/Button` 대신 `@/components/Button`처럼 깔끔하게 import 할 수 있음 | **Yes**       |

#### 3. 프로젝트 폴더 구조

```
my-next-app/
├── node_modules/             # npm 패키지 설치 폴더 (자동 생성)
├── .next/                    # Next.js 빌드 결과물 (자동 생성)
├── public/                   # 정적 파일 폴더 (이미지, 아이콘 등)
├── src/                      # 소스 코드 폴더
│   └── app/                  # App Router (Next.js 13+ 라우팅 방식)
│   └── pages/                # Pages Router (구 라우팅 방식)
│
├── package.json              # 프로젝트 메타데이터 및 의존성 관리
├── tsconfig.json             # TypeScript 컴파일러 설정
├── next.config.ts            # Next.js 프레임워크 설정
├── next-env.d.ts             # Next.js 자동 생성 타입 정의
├── postcss.config.mjs        # PostCSS 설정 (Tailwind CSS용)
├── eslint.config.mjs         # ESLint 코드 검사 설정
├── .gitignore                # Git 제외 파일 목록
└── README.md                 # 프로젝트 설명 문서
```

---
