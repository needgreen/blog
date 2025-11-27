# Next.js 라우팅 시스템 정리

- 10주 차 학습 : 2025.11.25-2025.11.28

## 파일 시스템 기반 라우팅이란?

React에서는 `react-router-dom`으로 일일이 경로를 설정해야 했지만 Next.js는 **폴더 구조가 곧 URL**이다.

**장점:**

- 프로젝트 구조만 봐도 사이트맵이 한눈에 파악됨
- 별도의 라우팅 설정 파일 관리 불필요

---

## App Router vs Pages Router

### 1) Pages Router (구방식)

```
📂 pages
    ├── index.tsx      → /
    ├── about.tsx      → /about
```

### 2) App Router (신방식 ⭐️ 권장)

```
📂 app
    ├── page.tsx           → /
    ├── layout.tsx         → 공통 레이아웃
    ├── about
    │   └── page.tsx       → /about
```

**App Router의 장점:**

- **Colocation**: 페이지 관련 컴포넌트, 스타일, 테스트를 한 폴더에 관리 가능
- **Layouts**: 중첩 레이아웃 구현이 훨씬 쉬워짐

---

## 핵심 특수 파일

- `page.tsx`: 해당 경로의 실제 화면
- `layout.tsx`: 공통 UI (헤더, 푸터 등)
- `not-found.tsx`: 404 페이지

---

## 동적 라우팅

### 1. 기본 동적 라우팅 `[id]`

```
app/products/[id]/page.tsx → /products/1, /products/galaxy
```

**⭐️ `[id]`는 URL의 길이(Depth)가 딱 1단계일때만 유효**

- 대괄호`[ ]`는 동적세그먼트라는 의미

**Next.js 15 중요 변경사항:**

```tsx
// params가 Promise로 변경됨!
export default async function Page({ params }: { params: Promise<{ id: string }> }) {
  const { id } = await params; // await 필수!
  return <div>Product ID: {id}</div>;
}
```

### 2. Catch-all Segments `[...slug]`

가변적인 깊이 처리 (최소 1단계 필요)

- `...` 은 가변이라는 의미

```
app/docs/[...slug]/page.tsx
→ /docs/a → ['a']
→ /docs/a/b/c → ['a', 'b', 'c']
```

### 3. Optional Catch-all `[[...slug]]`

루트 경로도 매칭

```
app/docs/[[...slug]]/page.tsx
→ /docs → undefined
→ /docs/a → ['a']
```

---

## 라우트 그룹 `(folderName)`

**URL에 영향 없이** 폴더로 그룹화

```
📂 app
    ├── 📂(marketing)
    │   ├── layout.tsx         → 마케팅용 헤더
    │   └── 📂about
    │       └── page.tsx       → /about
    └── 📂(auth)
        ├── layout.tsx         → 로그인용 레이아웃
        └── 📂login
            └── page.tsx        → /login
```

**사용 목적:**

- 레이아웃 분리
- 논리적 코드 그룹화

---

## 페이지 이동

### `<Link>` 컴포넌트 (권장)

```tsx
<Link href="/about">소개</Link>
```

- Prefetching 자동 지원
- SEO 친화적
- **일반적인 링크는 무조건 Link 사용!**

### `useRouter` 훅

```tsx
'use client';
import { useRouter } from 'next/navigation'; // ⚠️ next/router 아님!

const router = useRouter();
router.push('/dashboard');
```

- 로그인 후 리다이렉트 등 로직 처리 후 이동 시 사용

---

## 데이터 전달 방법

| 방식              | 예시                      | 용도       | 필수 여부         |
| ----------------- | ------------------------- | ---------- | ----------------- |
| **Path Variable** | `/products/123`           | 자원 식별  | 필수 (없으면 404) |
| **Query String**  | `/search?q=넥스트&page=1` | 옵션, 필터 | 선택적            |

### Query String 받기

**서버 컴포넌트:**

```tsx
export default async function SearchPage({
  searchParams,
}: {
  searchParams: Promise<{ q: string }>;
}) {
  const { q } = await searchParams;
  return <div>검색어: {q}</div>;
}
```

**클라이언트 컴포넌트:**

```tsx
'use client';
import { useSearchParams } from 'next/navigation';
import { Suspense } from 'react';

function SearchComponent() {
  const searchParams = useSearchParams();
  const q = searchParams.get('q');
  return <div>검색어: {q}</div>;
}

// ⚠️ Suspense로 감싸야 함!
export default function Page() {
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <SearchComponent />
    </Suspense>
  );
}
```

---

## 핵심 요약

✅ Next.js는 폴더 구조 = URL  
✅ App Router 사용 권장 (Colocation, Layouts)  
✅ Next.js 15부터 params가 Promise로 변경 → `await` 필수  
✅ 페이지 이동은 `<Link>` 우선, 로직 후 이동은 `useRouter`  
✅ Query String 받을 때 서버/클라이언트 컴포넌트 방법 다름  
✅ `useSearchParams` 사용 시 반드시 `<Suspense>` 필요
