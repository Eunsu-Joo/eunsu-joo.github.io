---
title: "React-Query 장점\U0001F680"
image: assets/img/query-benefits.PNG
categories:
- react-query
description: 리액트 쿼리의 장점과 기본 프로세스
---

리액트쿼리를 다시 꺼내본다,... 왜냐면 좋은것 같은데 왜 좋은지 모르기 때문에, 이걸 쫌 알고싶어서 다시 꺼냈다. 한번 잘 파보자

![]({{ 'assets/img/query.png' | relative_url }})

 [React-query 공홈](https://tanstack.com/)
# 🌺 리액트 쿼리
리액트 쿼리는 꽃이 아주 마음에든다... ㅎㅎ 예전에는 공홈에 꽃 로그가 박힌 깔끔하고 예쁜 사이트였는데, 지금은 버전이 바뀌면서 `tanstack`의 여러 라이브러리 들과 합쳐졌당.
아무튼 리액트 쿼리는 **Fetching, 동기화, 캐싱, 서버 쪽 데이터를 업데이트 등을 쉽게 할 수 있도록 도와주는 React 기반의 라이브러리이다.**  state(상태)를 관리하는 `Redux`, `Zustand`, `Recoil` 등 여러 라이브러리가 있는데도 불구하고, 리액트쿼리가 프론트 개발자들에게 큰 인기를 받는 이유는, **서버 상태를 참조하며, 데이터를 캐싱하기 때문이다. ** 

리액트 쿼리의 원리와 장점에 대해서 알려면, 우선 `Server State` 와 `Client State`의 차이에 대해서 알아야 할 것 같다.
## ❗  Client State VS Server State
 
![]({{ 'assets/img/query-states.PNG' | relative_url }})

### ◾ Server State

서버 상태는 말 그대로 서버에서 가져오는 데이터이다. 
> 1. 세션간의 지속되는 데이터
> 2. 클라이언트만 소유하는것이 아닌 공유되는 데이터도 존재하며, 클라이언트에 의해 수정될 수 있음.
> 3. 다른 사용자에 의해 변경될 수 있다.
> 
즉, 서버에서 가져오는 지속적인 데이터이며, 비동기 요청에 의해 패치와 업데이트, delete가 가능하다.  

### ◾ Client State
클라이언트에서 사용하는 동기적이며, 렌더링이 일어나는 항상 최신의 상태이다. 
예를 들어, 버튼을 열고 닫을 때 사용하는 `useState` 나  `Zustand, Recoil,Redux`등이 클라이언트에 전역적인 상태를 관리하는 라이브러리 이다. 


## ⛏ React-query가 만들어진 계기
 Client State와 Server State가 다르기 때문에, 서버에서 받아오는 서버 State는 항상 최신상태를 보장하지 않는다.
 지금 현재 리액트를 사용하면서 데이터 패칭하는 방식을 보면, 서버에서 데이터를 받아와서 `useState`나 다른 전역상태 관리 라이브러리에 저장해 놓은 다음에 사용하는 방식이다.
 이 방식을 사용하면, 서버의 상태 변화를 감지하기 위해서, 강제 리프레쉬를 하거나, state를 변화시켜 업데이트를 한다.
  네트워크 통신은 최소한으로 줄이는게 좋은데, 복수의 컴포넌트에서 최신 데이터를 받아오기 위해 fetching을 여러번 수행하며 낭비가 발생할 수 있다. 
	
 또한 store 내부에 클라이언트와 서버 state가 공존하게 된다. 예를 들어, 서버에서 이미 패치된 데이터가 클라이언트에서는 패치되기 전 데이터가 유저에게 사용될 수 도 있다.
 데이터를 캐싱해 네트워크의 불필요한 요청을 줄이고,  서버와 클라이언트 state를 분리하며, 보다 효율적인 비동기 요청으로 데이터를 불러오기 위해 (?) 만들어 졌다.
 
 
## 👀 React-query 장점
 
 ![]({{ 'assets/img/query-benefits.PNG' | relative_url }})
 
 리액트 쿼리로 여러가지 테스트를 해보니까 아주좋은 장점들이 많다.

### 1️⃣ Loading, Error States
 
 리액트 쿼리의 `useQuery`의 기본 리턴 값으로 `isLoading, isError, isFetching, error `등 로딩과 애러에 관련된 상태값을 리턴한다.
 기존 리액트를 사용했을 때 state를 만들어서 사용하였는데, 이런 불필요한 state사용을 줄여준다. 

### 2️⃣ Pagination & Infinite scroll
	
고유한 키값을 통하여, 페이지 네이션을 쉽게 구현할 수 있으며, 무한스크롤링과 관련된 다양한 hooks와 메서드들을 제공하여, 최신상태에 캐싱된 데이터들을 쉽게 구현할 수 있다.

### 3️⃣ Prefetching
페이지네이션의 다음 페이지를 보여질 경우 등 미리 프리패칭을 하여 데이터를 가져올 경우가 생기는데, 이 때 프리패칭을 가능하게 하는 매서드들이 있어 쉽게 구현할 수 있다.

{% highlight js %}
// App.js
import { Posts } from "./Posts";
import "./App.css";
import { QueryClient, QueryClientProvider } from "react-query";
import { ReactQueryDevtools } from "react-query/devtools";
const App=() => {
 return (
  <QueryClientProvider client={queryClient}>
   <div className="App">
    <h1>Blog Posts</h1>
     <Posts />
   </div>
   <ReactQueryDevtools position={"bottom-left"} initialIsOpen={false} />
 </QueryClientProvider>
)
}
{% endhighlight %}  


{% highlight js %}
// Post.js
//...생략
async function fetchPosts(pageNumber) {
  const response = await fetch(
   `${process.env.REACT_APP_API_URL}/posts?_limit=${MAX_PAGE}&_page=${pageNumber}`
  );
 return response.json();
}
const queryClient= useQueryClient() //프로바이더에 props에 담긴 queryClient의 정보가 담긴다.
const [currentPage,setCurrentPage]= useState(1);
useEffect(() => {
  const nextPage= currentPage+1;
    queryClient.prefetchQuery(["posts", nextCount], () =>
     fetchPosts(nextCount)
  );
},[currentPage,queryClient])
{% endhighlight %}

이렇게 `queryClient.prefetchQuery` 를 통하여, 데이터를 프리패칭 할 수 있다. 실제로 1번째 페이지네이션에서, 2번째 페이지네이션이 미리 가져와져, 
DevTools에 inactive 되어 있는 상태를 볼 수 있다. 옵션에는 useQuery가 갖고 있는 옵션들을 사용할 수 있다.

### 4️⃣  Mutations

useQuery와는 다르게 서버 데이터에 사이드 이펙트를 일으키는 경우 (create, update, delete)에 사용된다. 
useMutation 으로 mutation을 정의하고, mutate 함수로 호출한다. 반환되는 값은 useQuery와 동일하다.

{% highlight js %}
useMutation(addTodo, {
// 뮤테이션 시작
onMutate: (variables) => {
return { id: 1 };
},
//애러났을 때 콜백함수
onError: (error, variables, context) => {
console.log(`rolling back optimistic update with id ${context.id}`);
},
//성공했을 때 콜백함수
onSuccess: (data, variables, context) => {
console.log('success')
},
// 성공이든 에러든 어쨌든 끝났을 때
onSettled: (data, error, variables, context) => {
console.log("finish")
},
});
{% endhighlight %}

이 이외에도 요청을 복사할 수 있거나, Mutation할때나 useQuery를 불러올 때 재요청 처리를 옵션을 통해 쉽게 할 수 있다.
