---
layout: post
title: "React Query + Next.js"
author: sowonjin
categories: [React Query]
tags: [react-query, next.js]
image: assets/images/react-query.png
toc: true
---

## React Query 소개

`React Query`란 서버에서 받아온 데이터 fetching, 동기화, 캐싱 등을 보다 편리하게 관리해주는 라이브러리입니다.

## Next.js 프로젝트에서의 React Query의 사용

`Next.js`는 페이지 진입을 요청받게 되면 Server Side에서 해당 페이지를 데이터와 함께 Pre Rendering하고, 이를 props를 통하여 Client Side에 전달하며 Client Side에서는 전달받은 데이터를 기반으로 페이지를 출력하는 구조입니다.

만약 어떤 페이지가 `서버에서 받아온 데이터를 목록 형태(Paging 포함)로 출력`시켜야 한다면, 이를 Next.js + React Query로 구현 시에 아래와 같은 형태로 만들 수 있습니다.

1. Server Side에서는 첫번째 page 목록에 관련된 데이터 정보를 API 통신으로 받아온 후 Client Side에 props로 넘겨준다.
2. Client Side에서는 사용자의 인터렉션에 따라 두번째 이후의 page 정보를 React Query의 `useQuery Hook`을 이용하여 호출한다.

하지만 useQuery Hook은 `enabled` 옵션을 별도로 설정하지 않는 이상, Client Side에서 페이지가 출력된 후 최초로 한번은 호출되기 때문에 서버에서 내려받은 데이터와 똑같은 데이터를 또 다시 호출한다는 문제점이 있습니다.

## 해결방안

### 1) Server Side에서 API 호출 생략

결론부터 말씀드리면 이 방식은 추천드리지 않습니다.
Client Side에서 첫번째 page 정보를 호출하게 되면 사용자의 화면에서는 페이지가 부분적으로 로딩이 되는 것처럼 보이기 때문에 이를 보완하기 위해서 기존 CSR(React, Vue.. 등등) 프로젝트처럼 목록에 적용될 스켈레톤 이미지를 추가해야 할 수도 있기 때문입니다.

### 2) useQuery Hook의 enabled 옵션 사용

처음에 언급한 방법입니다. enabled옵션에 false가 되는 조건을 줘서 Client Side에서 첫번째 page 데이터를 호출하는 문제점을 수정할 수 있지만, 이 옵션을 사용하면 해당 useQuery 자체가 비활성화 되기 때문에 별도로 true 값을 주는 작업이 필요합니다.

```jsx
const { data } = useQuery(["loadListQuery", form], loadListAPI, {
  enabled: form.pg !== "1",
  refetchOnWindowFocus: false,
  onSuccess: (response) => {
    console.log("success", response);
  },
  onError: (error) => {
    console.log("onError", error);
  },
  staleTime: Infinity,
});
```

### 3) initialData 사용

useQuery Hook에서는 `initialData` 옵션 또한 존재하는데, Server Side에서 내려받은 props을 해당 useQuery Hook의 initialData에 넣어주면 초기 호출을 방지할 수 있습니다. 하지만 해당 hook이 중첩된 구조 안에 있는 컴포넌트에 위치한다면 해당 hook이 위치한 컴포넌트까지 `props drilling`을 해주어야 한다는 번거로움이 존재합니다. 또한 다음 페이지 정보를 호출하기 위해선 별도로 initialData를 비활성화하는 작업을 해야 합니다.

```jsx
export default function InitialData(props: any) {
  // props.data는 server side에서 받아온 데이터
  return <Overlap1 data={props.data} />;
}

function Overlap1(props: any) {
  return <Overlap2 data={props.data} />;
}

function Overlap2(props: any) {
  return <Overlap3 data={props.data} />;
}

function Overlap3(props: any) {
  return <Overlap4 data={props.data} />;
}

function Overlap4(props: any) {
  const { data } = useQuery(["loadListQuery", form], loadListAPI, {
    refetchOnWindowFocus: false,
    onSuccess: (response) => {
      console.log("success", response);
    },
    onError: (error) => {
      console.log("onError", error);
    },
    initialData: form.pg === "1" ? props.data : undefined, // Server Side에서 받아온 데이터를 여기까지 넘겨주어야 함.
    staleTime: Infinity,
  });
}
```

### 4) hydrate 사용

해당 포스트의 메인이 되는 주제입니다. 앞서 언급한 문제를 해결하기 위해 React Query에서는 이 방법을 추천하고 있습니다. hydrate를 사용한다면 초기 설정을 해야 하지만 이후에는 별도로 관리를 하지 않아도 된다는 장점이 있습니다.

## hydrate 소개 및 적용방법

위에서도 언급했듯이 Next.js에서는 페이지가 로드될때 Server Side에서 Client Side로 데이터를 전달하는데, Pre Rendering된 정적 페이지와 번들링된 javascript 데이터를 각각 따로 전달하는 방식을 가지고 있습니다.

따라서 Client Side에서 처음에 전달받은 정적 페이지에는 아무런 Javascript 요소(이벤트 리스너, 변수 및 함수 등등)들이 없는 상태인데, 전달받은 Javascript 데이터들을 DOM에 덧붙여서 정상적인 기능을 하는 페이지를 만들어내게 됩니다.
이러한 과정을 `hydrate`라고 합니다.

React Query에서도 Next.js의 hydrate 처럼 Server Side에서 미리 데이터를 prefetch한 후, 캐시된 데이터를 Client Side에서 내려받아 재사용하는 방식을 지원합니다.
Next.js에서 React Query의 hydrate를 사용하려면 우선 `_app.tsx` 파일에 Hydrate 컴포넌트를 추가해야 합니다.

```jsx
const queryClient = new QueryClient();

export default function App({ Component, pageProps }: AppProps) {
  return (
    <QueryClientProvider client={queryClient}>
      <Hydrate state={pageProps.dehydratedState}>
        <Component {...pageProps} />
      </Hydrate>
    </QueryClientProvider>
  );
}
```

그리고 Server Side에서 api를 handling하는 부분에도 queryClient를 추가하여 데이터를 preFetching 합니다.

```jsx
// pages/api/hydrate.tsx
async function getData(req: NextApiRequest, res: NextApiResponse) {
  const queryClient = new QueryClient();
  await queryClient.prefetchQuery(["loadListQuery", req.query!], loadListAPI);

  res.send({
    dehydratedState: dehydrate(queryClient),
  });
}

export default function handler(req: NextApiRequest, res: NextApiResponse) {
  getData(req, res);
}
```

또한 Server Side에서 Client Side에서 props을 내려줄때 반드시 dehydratedState를 넘겨주어야 합니다.

```jsx
// Server Side
export async function getServerSideProps(ctx: any) {
  const fetchAPI = async () => {
    const res = await fetch(
      `http://localhost:3000/api/hydrate?` + new URLSearchParams(schForm)
    );

    return await res.json();
  };

  const data = await fetchAPI();
  return {
    props: {
      dehydratedState: data.dehydratedState,
    },
  };
}

// Client Side
export default function Hydrate(props: any) {
  const { data } = useQuery(["loadListQuery", form], loadListAPI, {
    refetchOnWindowFocus: false,
    onSuccess: (response) => {
      console.log("success", response);
    },
    onError: (error) => {
      console.log("onError", error);
    },
    staleTime: Infinity,
  });
}
```

<p class="font-weight-bold">※ 주의사항</p>

- useQuery Hook의 옵션 중에 `staleTime`이 있는데 이는 해당 Query가 "신선한(fresh)" 시간을 의미합니다. 따라서 staleTime을 너무 짧게 지정하면 useQuery Hook은 해당 Query가 신선하지 않은 상태라고 판단하여 새로 API를 호출하게 됩니다.

[📁샘플코드 다운로드](https://exit365-my.sharepoint.com/personal/swj907_exit365_onmicrosoft_com/_layouts/15/download.aspx?SourceUrl=%2Fpersonal%2Fswj907%5Fexit365%5Fonmicrosoft%5Fcom%2FDocuments%2FMicrosoft%20Teams%20%EC%B1%84%ED%8C%85%20%ED%8C%8C%EC%9D%BC%2Fnext%2Dtest%20%281%29%2Ezip)

## 참고 문서

1. [React Query와 SSR](https://velog.io/@eomttt/React-Query-%EC%99%80-SSR#hydrate-%EB%9E%80){:target="\_blank"}
2. [Next.js Hydrate 개념](https://helloinyong.tistory.com/315){:target="\_blank"}
3. [tanStack Query 공식 문서](https://tanstack.com/query/v4/docs/react/guides/ssr#using-nextjs){:target="\_blank"}
