---
title: "React-Query개념알기\U0001F33A"
layout: post
image: https://velog.velcdn.com/images/soonmac/post/45b191a2-e23a-48b9-aba9-62a6362970f6/image.jpg
customexcerpt: "간단한 쇼핑몰-intro | react-query\U0001F33A"
categories:
- 간단한쇼핑몰
- react-query
description: "간단한 쇼핑몰 만들기 intro / react-query\U0001F33A"
---

![](https://miro.medium.com/max/1400/1*jwd_E0Ibcs7bv6xGxopIrg.jpeg)

인트로에 대해서 간단하게 개념과 사용이유만 포스팅하려고 했는데, 생각보다 내가 모르는게 너무 많음... 
이럴때 내가 개발이 맞나 싶음... 흐엉 ㅜㅜㅜㅜ 리액트 쿼리에 대해서 알아보려고 함!

예전에 한번 사용해 보려고 했는데, NEXT.JS에서는 코드 스플리팅이 되고, 쿼리를 SSR에서 보내고 다시 뿌려지는 방식으로 
데이터를 불러오고, 뿌려줬기 때문에 굳이..?라는 표현이 맞을 지는 모르지만.. ㅎㅎㅎ 굳이 사용하지 않아도 되었다.

하지만 쪼랩 쥴리가 모르고 있었던 사실이 있었다.. 그렇게 된다면 캐싱되지 않고 html을 쿼리가 바뀔때마다 다시 만들어서 클라이언트에 보내 보여주기 때문에, 데이터가 많을 경우 브라우저에 엄청난 부담을 준다는 사실을... 

그래서 이번 기회에 개념도 알아보고, 강의를 들어가기 전 여러가지 예제도 만들어보려함!

# React-query 🌺
<hr />

## ◼ React-query란?
* `React-query`란 비동기 로직을 리액트 스럽게 다뤄 서버의 값을 클라이언트에 가져오거나, 업데이트, 캐싱을 더 효율적이고 편하게 도와주는 라이브러리 이다.

### 🔹 React-query의 장점
<hr />
그렇다면 사람들은 왜 사용하는 것일까... 무슨 장점이 있기에 사용하는 것인가.. 


####  1. 캐싱이 가능함   
그렇다. 캐싱이 가능하다. 캐싱이란 **컴퓨터의 성능을 향상시기기 위해 사용되는 메모리** 를 말한다.

예를 들어 유저의 정보를 불러오는 코드가 있다면, 캐싱이 된다고 한다면 유저의 바뀌지 않는 정보를 데이터 저장소에 저장해 놓고, 빠르게 사용자에게 정보를 제공한다. 

이렇게 데이터 캐싱이 가능하다면, **불필요한 요청을 줄이며 브라우저에 부담을 덜 해 줄 수 있고,**  같은 정보를 데이터 저장소에서 빼내서 보여주므로 **사용자에게 빠르게 정보를 보여 줄 수 있다**.

#### 2. update하면 자동으로 get요청 한다.

포스트에서 글을 작성하고 url을 이동하거나 , 재요청을 하거나, refresh를 하지 않는 이상 같은 페이지에서 사용자는 업데이트 된 데이터가 아닌 기존 데이터를 보게 될 것이다.

`react-query`는 `useMutation`hooks 를 통해 update된 데이터를 다시 get하여 사용자에게 보여준다.
짱이다..

#### 3. 데이터 처리 결과에 따른 action을 취할 수 있다.

`useQuery` hooks 를통해 성공했을 경우, 실패했을 경우 그에 따른 action을 취할 수 있다.

예를 들어 실패했을 때 toast에 error 메세지를 내거나, alret을 띄워 사용자에게 애러나 났다는 것을 인지하게 할 경우 사용할 수 있다.

#### 4. 데이터가 오래되었다고 판단하면 다시 get요청을 한다.
`QueryClient`에서 `invalidateQueries` 메서드를 통해 쿼리를 무효화 시켜 지능적으로 표시하고, 잠재적으로 표시하고 잠재적으로 다시 가져올 수 있도록 사용한다. 

#### 5. useQuery가 갖고 있는 고유의 KEY 값으로 GlobalState 대체 가능

`useQuery` 의 고유의 키값을 통해 다른 컴포넌트에서는 호출이 가능하고 사용할 수 있다. 

이말은 즉슨 `React-query`는 캐싱이 되어 저장소에 변하지 않는 데이터를 저장하는데, 이것에 더해 고유의 KEY값으로 다른 컴포넌트에서 이 저장된 데이터를 사용 할 수 있다는건, **GlobalState를 사용하는 이유 ===  React-query의 이점**이 된다.

### 🔹 React-query 사용 방법
<hr />

`React-query`를 통해 관리하는 쿼리 데이터는 라이프사이클에 따라 **Fetching , Fresh, Stale, Inactive, Delete**상태를 가진다.

> * **Fetching** : 요청중인 쿼리
> * **Fresh** : 만료되지 않은 쿼리. 마운트, 업데이트 되어도 데이터 재요청을 하지 않는다.
> * **Stale** : 만료된 커리. 재요청한다.
> *  **Inactive** : 사용하지 않는 쿼리. 일정 시간이 지나면 가비지 컬렉터가 캐시에서 제거한다.
> *  **Delete** : 캐시에서 제거된 쿼리


### 🔹 React-query 사용하기
<hr />

간단하게 데이터 fetch하는 예시를 만들어 보자  (공홈 중심으로)
본인은 영어를 굉장히 못하기 때문에... 블로그들도 봐야 함..
**우선 공홈에 나와있는 Default 예제로 개념을 잡아보자!**
#### ✔ Provider 설정해주기

Context, Recoil 처럼 최상단 `App.js`에 QUeryProvider를 설정해 주어야 한다. 여기서 `props`로 `client`를 받는데,
이 값은 `new QueryClient`로 할당한 변수가 들어간다.

{% highlight js %}
import "./styles.css";
import Todo from  "./Todo";
import {QueryClientProvider,QueryClient} from "react-query"
export default function App() {
  const client= new QueryClient();
  return <QueryClientProvider client={client}>
    <div className="App">
      <Todo />
      </div>
  </QueryClientProvider>;
}
{% endhighlight %}

`Context`처럼 사용하기 전 최상위 컴포넌트에 Provider로 감싼다. props로 새로운 쿼리를 할당한 변수를 전달해준다. 

#### ✔  자식 Component에서 확인하고 Query 사용하기
{% highlight js %}
import { useQueryClient } from "react-query";
const Todo = () => {
  const queryClient= useQueryClient();
  console.log(queryClient)
  return <li>Todo List</li>;
};
export default Todo;
{% endhighlight %}

![]({{ 'assets/img/query1.PNG' | relative_url }})

이렇게 많은 메서드가 들어온 것을 확인할 수 있음. 이제 데이터 패칭할꺼임.

다음포스팅에서..

<hr />

**출처 🙏🏻**

* [공홈](https://react-query.tanstack.com/guides)
*  [ https://merrily-code.tistory.com/76]( https://merrily-code.tistory.com/76s)
*  [  기억보다 기록을](  https://kyounghwan01.github.io/blog/React/react-query/basic/#%E1%84%89%E1%85%A1%E1%84%8B%E1%85%AD%E1%86%BC%E1%84%92%E1%85%A1%E1%84%82%E1%85%B3%E1%86%AB-%E1%84%8B%E1%85%B5%E1%84%8B%E1%85%B2)
*  [ https://maxkim-j.github.io/posts/react-query-preview]( https://maxkim-j.github.io/posts/react-query-preview)
*  [[React Query] 리액트 쿼리 시작하기 (useQuery)](https://velog.io/@kimhyo_0218/React-Query-%EB%A6%AC%EC%95%A1%ED%8A%B8-%EC%BF%BC%EB%A6%AC-%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0-useQuery)
