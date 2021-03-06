---
title: "React-Query Hooks & 데이터불러오기\U0001F33A"
layout: post
categories:
- 간단한쇼핑몰
- react-query
description: "간단한 쇼핑몰-intro | react-query-hooks , example\U0001F33A"
image: https://images.velog.io/images/jkl1545/post/fdd77104-35d5-440b-a12f-5f94e1f1ea81/Group%2012.png
customexcerpt: 간단한 쇼핑몰 |  React-query / Hooks / Examples
---

지식이 부족해서 쓸게 넘 많음.. 그리고 블로그 적응안되서 시간이 오래 걸림... (물론 핑계 ㅎㅎ)
뭔가 하나하나 개념잡고 만들면 시간이 너무 많이 걸릴 것 같아서 두가지 예제로 개념을 잡으려고 함.

사실  Update되거나 Delete되었을때 다시 get하는 Mutation이나 , 전역 State로 사용하는 예제를 만들어보고 싶은데 그럼 strapi 설정해서 만들어야 함...ㅜㅜㅜ 강의 들으면서 그 부분은 다시 블로깅하고 ,지금은 간단한 예제를 통해 장점을 알아볼꺼다. 

더미데이터는 `jsonplaceholder`를 사용해보자. 

##  React-query의 세가지 컨셉
**✔ Queries**
**✔ Mutations**
**✔ Query Invalidation**

## ❓ Quries

보통 `GET`으로 받아올 때 사용한다고 함.

{% highlight js %}
 const info= useQuery('todo',fetcher);
{%  endhighlight  %}
- 첫번째 파라미터 : `unique key` => 다른컴포넌트에서도 해당 키를 호출하면 호출가능하다. `unique key`는 **String or 배열** type으로 받으며, 배열의 요소로 쿼리의 이름을 나타내는 문자열과 프로미스를 리턴하는 함수의 인자로 쓰이는 값을 넣는다.
- 두번째 파라미터 : `비동기 함수` => 만약 한 컴포넌트에 여러개 `useQuery`가 있다면 두개가 동시에 비동기적으로 실행된다.
- 세번째 파라미터 (optional) : `option`
	>* onSuccess | onError( callback) : 성공 실패 시 함수
	>* enabled (boolean) : 자동으로 query 실행 시킬 지 여부
	>* retry (boolean): 동작 실패 시 retry 할 지 여부
	>* select  (callback) : 데이터 가공 callback

	등등 여러 option들이 있다. 더 자세한 option은 [공식홈페이지 useQuery Option](https://react-query.tanstack.com/reference/useQuery#_top) 참고

### useQuery Return
요청한 쿼리는 여러가지 반환값을 갖는다. 대표적으로 

> * `data` : resolve 된 데이터
> * `error` : 에러가 발생했을 때 반환되는 객체
> * `isFetching` :  데이터가 받아오는 중일때 boolean
> * `status,isLoading,isError` : fetching status와 관련된 query의 상태 


## ❓ Mutations
Update할 때 사용한다고 한다. post, put method일 때 사용할 듯.

{% highlight js %}
 const info= useMutation(newTodo => axios.post('/todos',newTodo));
{% endhighlight %}

### useMutation 반환값
대표적으로 

> * `mutate` : mutation 실행 함수
> * `mutateAsync` :  mutate와 비슷하지만 promise 반환
> * `reset` :  mutate 내부 상태를 정리하는 함수(즉, mutate 초기 상태로 재설정).
> * `data,error,isLoading,isError,isIdle`

## ❓  Query Invalidation
어떤 분 말이 썩은 ㅋㅋㅋㅋㅋㅋㅋ 썩은 쿼리 폐기하고 함.. 
쿼리의 데이터가 요청을 통해 서버에서 바뀌었다면, 백그라운드에 남아있는 데이터는 과거의 것이 되어 앱에서 쓸모없어지는 상황이 발생하는데, 이 경우  `invalidateQueries` 메서드를 사용하여 폐기한다. 

{% highlight js %}
const queryClient= useQueryClient()
// 캐시가 있는 모든 쿼리들을 폐기.
queryClient.invalidateQueries()
// 'todos'로 시작하는 모든 쿼리들을 폐기
queryClient.invalidateQueries('todos')
//mutation 성공 할 시 이전 쿼리 폐기하고 리패칭
 const mutation = useMutation(addTodo, {
   onSuccess: () => {
     queryClient.invalidateQueries('todos')
     queryClient.invalidateQueries('reminders')
   },
 })
{% endhighlight %}

## 🗨 예시 - TodoList
{% highlight js %}
//App.js
import "./styles.css";
import Todo from  "./Todo";
import {QueryClientProvider,QueryClient} from "react-query"
export default function App() {
  const getClient= () => {
    let client=null;
    return !client? new QueryClient() :client
  }
	//나중에 파일 따로 만들어서 빼고 재사용하려고 함수로 만듬.
  return <QueryClientProvider client={getClient()}>
    <div className="App">
      <Todo />
      </div>
  </QueryClientProvider>;
}
{% endhighlight %}

{% highlight js %}
//Todo.js
import axios from "axios";
import { useQuery } from "react-query";
const Todo = () => {
const fetcher=async() => {
  const BASE_URL= `https://jsonplaceholder.typicode.com/todos`;
  const {data}= await axios.get(BASE_URL);
  return data
}
const {data,error,isLoading}= useQuery('todo',fetcher);
  if(isLoading) return <p>Loading..</p>
  if(error) return <p>Fuck ! Error! </p>
  return <ul>{
    data?.map(todo => <li key={todo.id}>{todo.title}</li>)
  }</ul>;
};
export default Todo;
{% endhighlight %}

![]({{ 'assets/img/query-todo.PNG' | relative_url }})

잘 불러와진다! 더 자세한 내용은 강의를 들으면서 포스팅 하기로!
