---
layout: post
title: "[API Mocking] MSW(Mock Service Worker)를 이용하여 FE 업무 효율성을 높여보자. + Postman Mock Server"
author: kimyura
categories: [Tech]
tags: [MSW]
image: assets/images/api-moking.png
beforetoc: "본 게시글은 기본적인 github 블로그 사용 방법에 대해 알려드리는 글입니다."
toc: true
---

## MSW를 사용하게 된 계기

프로젝트를 진행하는 상황에서 보통의 플로우는 기획서가 나오면 해당 기획서를 바탕으로 디자인 작업이 시작이 되고, 백엔드에서 DB설계 및 api를 만들게 되는데요. 그 결과물이 나오면 프론트엔드의 개발이 시작됩니다. 하지만 항상, 모든 프로젝트에서 이 플로우가 정확하게 지켜지지는 않습니다. 불가피하게 백엔드와 프론트의 작업일정이 동시에 시작되는 경우가 있는데, 이런 경우 프론트엔드가 데이터 없이 개발을 해야 하는 상황(병목현상)이 생기곤 합니다.

_해당 포스팅은 백엔드와 프론트엔드간의 작업 일정에 대한 책임을 묻는 포스팅이 아닙니다._

> 1. 마냥 기다리기 (일정이 넉넉하다면)
> 2. 상상하며 구현하기
> 3. 더미 데이터 넣어서 구현하기 (더미 데이터를 활용하는 것도 Mocking의 하나로, 의존성을 낮추었다고 볼 수 있습니다. 하지만 더미 데이터를 걷어내고, api 데이터로 교체하는 작업 시간도 은근히 소요되기 때문에 비효율적이라고 볼 수 있습니다.)

일정은 맞춰져 있고, 그 작업을 기다리며 프론트엔드가 더 효율적으로 대응할 수 있는 방법은 없을까 고민하다 찾아보게 된 방법이, 가상 서버를 이용하여 HTTP api 통신을 구현해보는 것 입니다.

- 미리 약속된 데이터set(key값, value의 type값 등..)이 정의가 되어있어야 좀 더 수월한 개발이 가능합니다.

## MSW 작동원리

![](/assets/images/api-moking-cont-1.png)

**Mock Service Worker 리퀘스트 흐름도 (이미지 출처: https://mswjs.io/docs/)**

> 브라우저에 서비스 워커(Service Woker)를 등록하여 외부로 나가는 네트워크 리퀘스트를 감지한다. 그리고 그 요청을 실제 서버가 아닌 MSW 클라이언트 사이드 라이브러리로 보낸 후, 등록된 핸들러를 통해 모의 응답을 브라우저로 보낸다.

## MSW 사용법

```
npm install msw
```

msw를 설치합니다.

```jsx
import { rest, setupWorker } from "msw";

let data = { title: "hello!" };

const handlers = [
  rest.get("/mock/test", (req, res, ctx) => {
    return res(
      ctx.status(200), // api status code
      ctx.delay(1000), // api delay time
      ctx.json(data)
    );
  }),
];

export default handlers;
export const worker = setupWorker(...handlers);
```

프로젝트의 가장 상위 index 파일에 해당 코드를 추가합니다.
Dev 모드로 실행시에는 msw를 사용하게 되는데요, 해당 방법은 개발기 api와 msw를 구분하기 위해서 서버 설정을 계속 바꿔줘야 하는 문제가 있습니다.

```jsx
if (process.env.NODE_ENV === "development") {
  const { worker } = require("./lib/api/mswApi");
  worker.start();
}
```

그래서 proxy 설정을 통해 /mock/~ 으로 들어오는 api 호출 시에는 msw를 사용하도록 구분해 줍니다.

```jsx
'/mock/': {
  changeOrigin: true,
  target: ,
},
```

하지만 해당 방법은 초기 셋팅을 해야 하는 점들과, localhost 환경에서만 작동하며 기타 제약사항들이 많이 있어 복잡하다고 느꼈고, 비교적 사용이 쉬운 포스트맨에서 mock server를 사용해보았습니다.

## Postman Mock Server 이용방법

❗️FREE plan인 경우 Mock Server 사용 시 1000건의 call limit이 있다고 합니다. 계정 plan 확인 후 사용하시는 것을 추천드립니다.

![](/assets/images/api-moking.png)

mock server 탭에서 새로운 mock server를 생성해줍니다.

![](/assets/images/api-moking-cont-2.png)

![](/assets/images/api-moking-cont-3.png)

사용할 api url 및 code, body값을 입력하고,

![](/assets/images/api-moking-cont-4.png)

서버 네임을 지정하여 create 해줍니다.

![](/assets/images/api-moking-cont-5.png)

그럼 workspace에 생성이 되는데요. 생성된 api에 add example로 원하는 데이터를 설정할 수 있습니다.
저는 body, params 아무것도 넘기지 않은 경우에 200, 데이터 Return 하도록 설정하고,
body값에 error 관련하여 값을 넘기면 해당 코드에 맞춰 에러를 발생하도록 설정해두었습니다.

![](/assets/images/api-moking-cont-6.png)

개발자 도구 network에도 노출되게 됩니다.
실제로 api가 나왔을 경우에도 url만 수정하게 되어 수월한 작업이 가능했습니다.

![](/assets/images/api-moking-cont-7.png)

에러 코드를 직접 컨트롤 가능한 점이 많은 프로젝트 상에서 에러 대응을 프론트엔드가 자유롭게 할 수 있다는 점이 유익했습니다.

## 포스팅을 마치며

위 방법들 말고도 JS 프레임 워크 내에서 사용하는 가상 서버 구현 방법은 많이 있겠지만,
해당 방법들을 이용하여 데이터셋을 역으로 백앤드에게 제안할 수도 있고,
빠른 시일 내에 개발을 완료 할 수 있어서, 정해진 기간 안에 좀 더 완성도 높은 작업물로 테스트 하게 되다 보니 에러도 적고, 작업의 딜레이가 거의 없었습니다.
해당 방법을 실제로 도입하여 프론트앤드로서의 역량을 키울 수 있었습니다.
이 게시물을 보시는 분들께 많은 도움이 되었으면 좋겠습니다.

## 참고문서

[https://tech.kakao.com/2021/09/29/mocking-fe/](https://tech.kakao.com/2021/09/29/mocking-fe/){:target="\_blank"}  
[https://yozm.wishket.com/magazine/detail/1711/](https://yozm.wishket.com/magazine/detail/1711/){:target="\_blank"}  
[https://www.couchcoding.kr/blogs/couchcoding/Postman%20Mock%20Server%EB%A1%9C%20%ED%8C%80%20%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8%EC%9D%98%20%EA%B0%9C%EB%B0%9C%20%EC%86%8D%EB%8F%84%EC%99%80%20%EA%B0%88%EB%93%B1%EC%9D%84%20%EA%B0%9C%EC%84%A0%ED%95%98%EA%B8%B0](https://www.couchcoding.kr/blogs/couchcoding/Postman%20Mock%20Server%EB%A1%9C%20%ED%8C%80%20%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8%EC%9D%98%20%EA%B0%9C%EB%B0%9C%20%EC%86%8D%EB%8F%84%EC%99%80%20%EA%B0%88%EB%93%B1%EC%9D%84%20%EA%B0%9C%EC%84%A0%ED%95%98%EA%B8%B0){:target="\_blank"}
