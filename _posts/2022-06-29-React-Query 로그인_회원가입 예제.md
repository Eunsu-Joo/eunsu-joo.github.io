---
title: "React-Query 로그인_회원가입 예제\U0001F33A"
layout: post
categories:
- react-query
image: https://pbs.twimg.com/media/EqfbmltVQAEPM8l.jpg
descrtion: 리액트쿼리로 로그인 구현하려고 했는데.. 안돼.. JWT값이 안담겨...ㅜㅜ
customexcerpt: 리액트쿼리로 로그인 구현하려고 했는데.. 안돼.. JWT값이 안담겨...ㅜㅜ
---

![](https://pbs.twimg.com/media/EqfbmltVQAEPM8l.jpg)
오늘 비가온다.. ㅎㅎㅎ 
아 내가 저번에 예제만든다고 Strapi만들어 놓은게 있어서 로그인 & 회원가입 이 부분 사용해서 확인하고 예제만들어보면 좋을 것 같아서 한번 그거 활용해서 만들어보면 이해가 더 잘 될 것같아서 포스팅을 하려고 한당!!

## 🌺React-query 로그인 &회원가입 만들기

**⛏ : React + Typescript +React-query + Strapi**

뭐든 좋은거 싫은거 육안으로 확인해야 직성 풀리는 스타일... 피곤해..

아 하다보니까 로그인하고 회원가입 폼 마크업하고, hook 만들고 하려면 간단한 예제가 안될 것 같아서 걍 하려고함.. 코드 샌드박스에서 함 만들어봅세~~

스타일아예 안 넣고 기본마크업만 갖고 작업할꺼임.

### 1.  Provider 설정 & Markup
 
 {% highlight js %}
//index.tsx
import * as React from "react";
import ReactDOM from "react-dom/client";
import { BrowserRouter } from "react-router-dom";
import App from "./App";
const rootElement = document.getElementById("root")!;
const root = ReactDOM.createRoot(rootElement);
root.render(
  <BrowserRouter>
    <App />
  </BrowserRouter>
);

{%endhighlight%}

#### typescript에서 ❗ 이 무엇인가
 
1.  **Null이 아닌 어선셜 연산자  (Non-null assertion operator)**
Null이 아닌 어선셜 연산자는 피연산자가 null이 아니라고 컴파일러에게 전달하여 일시적으로 Null 제약조건을 완화한다.
**typescript에게 얘는 null이 아니라고 알려주는 역활을 함.**

2.  **확정 할당 어선셜  (Definite Assignment Assertions)**
 **값이 무조건 할당되어있다고 컴파일러에게 전달하여 값이 없어도 변수 or 객체를 사용 할 수 있게 해줌.**
 
 출처:  [평범한 직장인의 공부 정리:티스토리](https://developer-talk.tistory.com/191)

{% highlight js %}
//App.tsx
import React from "react";
import { QueryClient, QueryClientProvider } from "react-query";
import "./styles.css";
import Home from "./components/Home";
import { Route, Routes, Link } from "react-router-dom";
import Signup from "./components/Signup";
export default function App() {
  const client = new QueryClient();
  return (
    <QueryClientProvider client={client}>
      <div className="App">
			 <Link to="/">LOGIN</Link>
        <Link to="/signup">SiGNUP</Link>
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="/signup" element={<Signup />} />
        </Routes>
      </div>
    </QueryClientProvider>
  );
}
{%endhighlight%}


{% highlight js %}
//Home.tsx
import React from "react";
import Login from "./Login";
const Home = () => {
  return (
    <>
      <Login />
    </>
  );
};
export default Home;
{%endhighlight%}

Home 컴포넌트는 기본 로그인 페이지 , 로그인 성공했을 때 WELCOME `username`을 띄울꺼임.
우선은 로그인 컴포넌트를 보여주자
### 2.  useInputs Hook & 유효성 검사 (비밀번호)

{% highlight js %}
//useInputs.tsx
import { ChangeEvent, useState } from "react"
const useInputs=(keys:Array<string>) => {
  const [inputs,setInputs]= useState(() => {
    const values={};
    keys.forEach((_,index)=>values[keys[index]]='')
    return values
  })
  const onChange=(event:ChangeEvent<HTMLInputElement>)=> {
    const {name,value}= event.target;
    setInputs({
      ...inputs,
      [name]:value
    })
  }
  return {inputs,onChange}
}
export default useInputs;
{%endhighlight%}
	
파라미터를 string배열로 받아서, 그 값을 오브젝트의 KEY로 만드는 Hooks
	
{% highlight js %}
//Signup.tsx
import React, { FormEvent } from "react";
import useInputs from "../hooks/useInputs";
const Signup = () => {
  const { inputs, onChange } = useInputs(["username", "email", "password"]);
  const onSubmit = (event: FormEvent<HTMLFormElement>) => {
    const pwRegExp = /(?=.*[a-z])(?=.*[A-Z]).{6,15}/;
    event.preventDefault();
    const isEmptyField =
      !inputs["username"] || !inputs["email"] || !inputs["password"];
    const isRegExp = !pwRegExp.test(inputs["password"]);
    if (isEmptyField) return alert("빈칸을 모두 채워주세요");
    if (isRegExp) return alert("비밀번호는 대문자 1개이상이 들어가야 합니다.");
  };
  return (
    <section>
      <h1>SIGNUP</h1>
      <form onSubmit={onSubmit}>
        <div>
          <label htmlFor="username">username</label>
          <input
            id="username"
            name="username"
            value={inputs["username"]}
            onChange={onChange}
          />
        </div>
        <div>
          <label htmlFor="userEmail">Email</label>
          <input
            type="email"
            id="userEmail"
            name="email"
            value={inputs["email"]}
            onChange={onChange}
          />
        </div>
        <div>
          <label htmlFor="userPw">Password</label>
          <input
            type="password"
            id="userPw"
            name="password"
            value={inputs["password"]}
            onChange={onChange}
          />
        </div>
        <button>SIGNUP</button>
      </form>
    </section>
  );
};
export default Signup;
{%endhighlight%}
	
### 	3. Config & fetcher 함수 만들기
	
fetcher는 간단한 요청 불러오는 거니까  `axios` 말고 `fetch` 사용할꺼임.
	{% highlight js %}
	//config.ts
export const API_URL= `https://nextdjbackend.herokuapp.com`;
export const QUERY_KEY= 'login';
	{%endhighlight%}
	
편하게 빼서 가져올려고 config 파일만들어서 export함.
		{% highlight js %}
	//fetcher.ts
import { API_URL } from "./config";
type ParamObj = {
  [key: string]: any;
};
type FetcherType = {
  method: "GET" | "POST" | "DELETE" | "PUT";
  path: string;
  body?: ParamObj;
  token?: string;
};
const fetcher = async ({ method, path, body, token }: FetcherType) => {
  const options: RequestInit = {
    method,
    headers: {
      "Content-type": "application/json",
      Authorization: token ? `Bearer ${token}` : ""
    },
    body: JSON.stringify(body)
  };
  const res = await fetch(`${API_URL}${path}`, options);
  const data = await res.json();
  return data;
};
export default fetcher;
	{%endhighlight%}
	
### 	4.useQuery로 데이터 fetching
	
{% highlight js %}
	//Home.tsx
import React from "react";
import { QueryClient, useQuery, useQueryClient } from "react-query";
import { QUERY_KEY } from "../config";
import fetcher from "../fetcher";
import Login from "./Login";
import Welcome from "./Welcome";
const Home = () => {
  const queryClient: QueryClient = useQueryClient();
  const jwt: any = queryClient.getQueryData(QUERY_KEY);
  const { data, isLoading } = useQuery(QUERY_KEY, () =>
    fetcher({
      method: "GET",
      path: "/api/users/me",
      token: jwt ?? ""
    })
  );
  return <>{jwt.data ? <Welcome /> : <Login />}</>;
};
export default Home;
{%endhighlight%}
<br />
{% highlight js %}
	//Login.tsx
import React, { FormEvent } from "react";
import { useMutation, useQueryClient } from "react-query";
import { QUERY_KEY } from "../config";
import fetcher from "../fetcher";
import useInputs from "../hooks/useInputs";
const Login = () => {
  const queryClient = useQueryClient();
  const { inputs, onChange } = useInputs(["identifier", "password"]);
  const loginMutation = useMutation(
    (fileds: { identifier: string; password: string } | {}) =>
      fetcher({
        method: "POST",
        path: "/api/auth/local",
        body: fileds
      }),
    {
      onSuccess: (data) => {
        if (data.jwt) {
          alert("로그인 성공!");
          queryClient.invalidateQueries(QUERY_KEY);
          queryClient.setQueryData(QUERY_KEY, { data: data.jwt });
        } else {
          alert("비밀번호 혹은 아이디가 일치하지 않습니다.");
        }
      }
    }
  );
  const onSubmit = (event: FormEvent<HTMLFormElement>) => {
    event.preventDefault();
    const isEmptyField = !inputs["identifier"] || !inputs["password"];
    if (isEmptyField) {
      alert("빈칸을 모두 채워주세요");
    } else {
      loginMutation.mutate(inputs);
    }
  };
  return (
    <section>
      <h1>LOGIN</h1>
      <form onSubmit={onSubmit}>
        <div>
          <label htmlFor="userEmail">Email</label>
          <input
            type="email"
            id="userEmail"
            name="identifier"
            value={inputs["identifier"]}
            onChange={onChange}
          />
        </div>
        <div>
          <label htmlFor="userPw">Password</label>
          <input
            type="password"
            id="userPw"
            name="password"
            value={inputs["password"]}
            onChange={onChange}
          />
        </div>
        <button>LOGIN</button>
      </form>
    </section>
  );
};
export default Login;
{%endhighlight%}
	
<br />
	로그인 JWT가 Home에서 담기는데 담겼다가 사라진다.. 개념 덜 익혔어... ㅜㅜㅜ
	오늘 에너지 다 써서 내일 한번 뚫어봐야 겠다.
	개념이 안잡힘 ㅜㅜㅜㅜㅜㅜㅜ
