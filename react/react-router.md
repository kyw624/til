# React Router

리액트에서 라우팅을 도와주는 라이브러리로,  
해당 문서는 [벨로퍼트님의 리액트 라우터 v6 튜토리얼](https://velog.io/@velopert/react-router-v6-tutorial) 문서를 보면서 정리한 내용이다.  
리액트에서 라우트 시스템을 구축하기 위한 방법은 다음 2가지가 있다.

### 1. 리액트 라우터 (React Router)

리액트의 라우팅 관련 라이브러리 중 가장 오래됐고, 가장 많이 사용된다.  
컴포넌트 기반으로 라우팅 시스템을 설정할 수 있다.

### 2. Next.js

리액트 프로젝트의 프레임워크로, CRA처럼 리액트 프로젝트 설정 기능, 라우팅 시스템, 최적화, 다국어 시스템 지원, 서버 사이드 렌더링 등 다양한 기능을 제공한다.  
파일 경로 기반으로 작동하며, 리액트 라우터의 대안으로 많이 사용된다.

<br />

## **설치 및 적용**

### 1. 설치

프로젝트 디렉터리에 react-router-dom을 설치하면 된다.

`yarn add react-router-dom`

---

### 2. 프로젝트에 적용

index.js 파일에서 라이브러리에 내장된 `BrowserRouter` 컴포넌트를 사용해 감싸면 된다.  
해당 컴포넌트는 웹 애플리케이션에 HTML5의 History API를 사용해 페이지를 새로 불러오지 않고도 주소를 변경하거나 현재 주소의 경로에 관련된 정보를 리액트 컴포넌트에서 사용할 수 있도록 해준다.

```jsx
// index.js
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';
import { BrowserRouter } from 'react-router-dom';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <BrowserRouter>
    <App />
  </BrowserRouter>
);
```

---

### 3. 라우팅을 위한 페이지 컴포넌트 구성

```jsx
// src/pages/Home.js
const Home = () => {
  return (
    <div>
      <h1>홈</h1>
      <p>가장 먼저 보여지는 페이지입니다.</p>
    </div>
  );
};

export default Home;

// src/pages/About.js
const About = () => {
  return (
    <div>
      <h1>소개</h1>
      <p>리액트 라우터를 사용해 보는 프로젝트입니다.</p>
    </div>
  );
};

export default About;
```

---

### 4. <Route> 컴포넌트로 특정 경로에 특정 컴포넌트 설정

`<Route>` 컴포넌트는 사용자의 주소 경로에 따라 원하는 컴포넌트를 보여주기 위해서 사용한다.

`<Route path="주소규칙" element={<Compoenet />} />`

<br />

또한 `<Route>` 는 `<Routes>` 라는 컴포넌트 내부에서 사용되어야 한다.

```jsx
// src/App.js
import { Route, Routes } from 'react-router-dom';
import About from './pages/About';
import Home from './pages/Home';

const App = () => {
  return (
    <Routes>
      <Route path="/" element={<Home />} />
      <Route path="/about" element={<About />} />
    </Routes>
  );
};

export default App;
```

---

### 5. <Link> 컴포넌트를 사용해 페이지 이동하기

웹 페이지에서는 링크를 보여줄 때 `a` 태그를 사용하는데,  
리액트에서는 페이지를 새로 불러오기 때문에 `a` 태그를 사용하면 안된다.  
이때 사용하는게 `<Link>` 컴포넌트인데,  
`<Link>` 역시 `a` 태그를 사용하지만 페이지를 새로 불러오는 것을 막고 History API를 통해 주소의 경로만 바꾸는 기능이 내장되어 있다.

`<Link to="경로">링크 이름</Link>`

<br />

Home 페이지에서 About 페이지로 이동하는 예제

```jsx
// src/pages/Home.js
import { Link } from 'react-router-dom';

const Home = () => {
  return (
    <div>
      <h1>홈</h1>
      <p>가장 먼저 보여지는 페이지입니다.</p>
      <Link to="/about">소개</Link>
    </div>
  );
};

export default Home;
```

<br />

## **URL 파라미터와 쿼리스트링**

페이지 주소를 정의할 때 다음과 같이 유동적인 값을 사용해야 할 때도 있다.

- **URL 파라미터 예시**: /profile/**velopert**
- **쿼리스트링 예시**: /articles?**page=1&keyword=react**

**URL 파라미터**는 주소의 경로에 유동적인 값을 넣는 형태로,  
ID 또는 이름을 사용해 특정 데이터를 조회할 때 사용한다.

**쿼리스트링**은 주소 뒷부분에 `?` 문자열 이후 key=value로 값을 정의하며 각 값들을 &로 구분하는 형태로,  
키워드 검색, 페이지네이션, 정렬 방식 등 데이터 조회에 필요한 옵션을 전달할 때 사용한다.

---

### 1. URL 파라미터

- URL 파라미터는 `useParams` 라는 Hook을 사용하여 객체 형태로 조회할 수 있다.
- URL 파라미터의 이름은 라우트 설정 시 `Route` 컴포넌트의 `path` 를 통해 설정한다.

```jsx
// src/pages/Profile.js
import { useParams } from 'react-router-dom';

const data = {
  velopert: {
    name: '김민준',
    description: '리액트를 좋아하는 개발자',
  },
  gildong: {
    name: '홍길동',
    description: '고전 소설 홍길동전의 주인공',
  },
};

const Profile = () => {
  const params = useParams();
  const profile = data[params.username];

  return (
    <div>
      <h1>사용자 프로필</h1>
      {profile ? (
        <div>
          <h2>{profile.name}</h2>
          <p>{profile.description}</p>
        </div>
      ) : (
        <p>존재하지 않는 프로필입니다.</p>
      )}
    </div>
  );
};

export default Profile;
```

위 코드에서는 `data` 객체에 예시 프로필 정보들을 key-value 형태로 담아두었다.

---

#### Profile 컴포넌트 라우팅 설정

```jsx
// src/App.js
import { Route, Routes } from 'react-router-dom';
import About from './pages/About';
import Home from './pages/Home';
import Profile from './pages/Profile';

const App = () => {
  return (
    <Routes>
      <Route path="/" element={<Home />} />
      <Route path="/about" element={<About />} />
      <Route path="/profiles/:username" element={<Profile />} />
    </Routes>
  );
};

export default App;
```

URL 파라미터는 `/profiles/:username` 과 같이 경로에 `:` 를 사용하여 설정한다.  
URL 파라미터가 여러개인 경우엔 `/profiles/:username:field` 와 같은 형태로 설정할 수 있다.

<br />

Home 페이지에 Profile Link 추가

```jsx
// src/pages/Home.js
import { Link } from 'react-router-dom';

const Home = () => {
  return (
    <div>
      <h1>홈</h1>
      <p>가장 먼저 보여지는 페이지입니다.</p>
      <ul>
        <li>
          <Link to="/about">소개</Link>
        </li>
        <li>
          <Link to="/profiles/velopert">velopert의 프로필</Link>
        </li>
        <li>
          <Link to="/profiles/gildong">gildong의 프로필</Link>
        </li>
        <li>
          <Link to="/profiles/void">존재하지 않는 프로필</Link>
        </li>
      </ul>
    </div>
  );
};

export default Home;
```

<br />

### 2. 쿼리스트링

쿼리스트링은 URI 파라미터와 달리 `Route` 컴포넌트 사용 시 별도 설정이 필요없다.

```jsx
/// 쿼리스트링을 화면에 띄우는 작업

// src/pages/About.js
import { useLocation } from 'react-router-dom';

const About = () => {
  const location = useLocation();

  return (
    <div>
      <h1>소개</h1>
      <p>리액트 라우터를 사용해 보는 프로젝트입니다.</p>
      <p>쿼리스트링: {location.search}</p>
    </div>
  );
};

export default About;
```

위 컴포넌트에서 `useLocation` 이라는 Hook을 사용했는데,  
이 Hook은 location 객체를 반환하며  
이 객체는 현재 사용자가 보고있는 페이지의 정보를 지니고 있습니다.  
이 객체에는 다음과 같은 값들이 있습니다.

- **pathname**: 현재 주소의 경로 (쿼리스트링 제외)
- **search**: 맨 앞의 ? 문자 포함한 쿼리스트링 값
- **hash**: 주소의 # 문자열 뒤의 값 (주로 History API 가 지원되지 않는 구형 브라우저에서 클라이언트 라우팅을 사용할 때 쓰는 해시 라우터에서 사용합니다.)
- **state**: 페이지로 이동할때 임의로 넣을 수 있는 상태 값
- **key**: location 객체의 고유 값, 초기에는 default 이며 페이지가 변경될때마다 고유의 값이 생성됨

쿼리스트링은 `location.search` 값을 통해 조회가 가능하다.  
위 About 페이지에서 주소창에 쿼리스트링 입력 시 페이지에 표시된다.

```jsx
// http://localhost:3000/about?detail=true&mode=1
// 결과값: ?detail=true&mode=1
```

이 문자열을 분리한 뒤 key와 value 를 파싱해줘야 하는데,  
리액트 라우터 v6부터는 `useSearchParams` 라는 Hook을 통해 쉽게 다룰 수 있다.

---

#### useSearchParams 예제

```jsx
// src/pages/About.js
import { useSearchParams } from 'react-router-dom';

const About = () => {
  const [searchParams, setSearchParams] = useSearchParams();
  const detail = searchParams.get('detail');
  const mode = searchParams.get('mode');

  const onToggleDetail = () => {
    setSearchParams({ mode, detail: detail === 'true' ? false : true });
  };

  const onIncreaseMode = () => {
    const nextMode = mode === null ? 1 : parseInt(mode) + 1;
    setSearchParams({ mode: nextMode, detail });
  };

  return (
    <div>
      <h1>소개</h1>
      <p>리액트 라우터를 사용해 보는 프로젝트입니다.</p>
      <p>detail: {detail}</p>
      <p>mode: {mode}</p>
      <button onClick={onToggleDetail}>Toggle detail</button>
      <button onClick={onIncreaseMode}>mode + 1</button>
    </div>
  );
};

export default About;
```

`useSearchParams` 는 배열 타입을 반환하며,  
첫번째 원소는 쿼리파라미터를 조회, 수정하는 메서드들이 담긴 객체를 반환하며,  
두번째 원소는 쿼리파라미터를 객체형태로 업데이트할 수 있는 함수를 반환한다.

- `get` 메서드: 특정 파라미터를 조회
- `set` 메서드: 특정 파라미터를 업데이트

주의점으로는 쿼리파라미터 조회 시의 값은 무조건 문자열이다.  
boolean 값을 넣게 된다면 따옴표로 감싸야하고,  
숫자를 다루게 된다면 숫자 형변환이 필요하다.

<br />

## **중첩된 라우트 다루기**

다음은 중첩된 라우트 이해를 위한 코드이다.

### 1. 중첩된 라우트 예제

기존 방식처럼 구현을 한다면 페이지마다 공통되게 보여지는 컴포넌트를  
각 페이지에서 사용해야한다.

그러나 리액트 라우터에서 제공하는 `Outlet` 컴포넌트를 사용하면  
 간단한 라우팅 설정 변경만으로도 가능하다.

> **Outlet**
>
> Route의 children으로 들어가는 JSX 엘리먼트를 보여주는 역할.

```jsx
// src/pages/Articles.js
import { Link, Outlet } from 'react-router-dom';

const Articles = () => {
  return (
    <div>
      <Outlet /> {/* 이 자리에 중첩된 라우트가 보여지게 된다. */}
      <ul>
        <li>
          <Link to="/articles/1">게시글 1</Link>
        </li>
        <li>
          <Link to="/articles/2">게시글 2</Link>
        </li>
        <li>
          <Link to="/articles/3">게시글 3</Link>
        </li>
      </ul>
    </div>
  );
};

export default Articles;

// src/pages/Article.js
import { useParams } from 'react-router-dom';

const Article = () => {
  const { id } = useParams();
  return (
    <div>
      <h2>게시글 {id}</h2>
    </div>
  );
};

export default Article;
```

두 컴포넌트 작성 후 `App` 컴포넌트에서 라우팅 설정

```jsx
// src/App.js
import { Route, Routes } from 'react-router-dom';
import About from './pages/About';
import Article from './pages/Article';
import Articles from './pages/Articles';
import Home from './pages/Home';
import Profile from './pages/Profile';

const App = () => {
  return (
    <Routes>
      <Route path="/" element={<Home />} />
      <Route path="/about" element={<About />} />
      <Route path="/profiles/:username" element={<Profile />} />
      <Route path="/articles" element={<Articles />}>
        // 중첩된 라우트 설정
        <Route path="/:id" element={<Article />} />
      </Route>
    </Routes>
  );
};

export default App;
```

Home 컴포넌트에서 게시글 목록 페이지 링크 추가

```jsx
// src/pages/Home.js
import { Link } from 'react-router-dom';

const Home = () => {
  return (
    <div>
      <h1>홈</h1>
      <p>가장 먼저 보여지는 페이지입니다.</p>
      <ul>
        <li>
          <Link to="/about">소개</Link>
        </li>
        <li>
          <Link to="/profiles/velopert">velopert의 프로필</Link>
        </li>
        <li>
          <Link to="/profiles/gildong">gildong의 프로필</Link>
        </li>
        <li>
          <Link to="/profiles/void">존재하지 않는 프로필</Link>
        </li>
        <li>
          <Link to="/articles">게시글 목록</Link>
        </li>
      </ul>
    </div>
  );
};

export default Home;
```

---

### 2. 공통 레이아웃 컴포넌트에 활용

중첩된 라우트와 `Outlet`은 페이지마다 공통적으로 보여지는 레이아웃이 있을 때도 유용하게 사용할 수 있다.

> ex) 각 페이지마다 상단에 헤더를 보여주는 상황

따로 `Header` 컴포넌트를 작성해 사용할 수도 있지만 이런 방법도 있고,  
이 방법을 사용하면 컴포넌트를 한번만 사용해도 된다는 장점이 있다.

```jsx
// 공통 레이아웃을 위한 `Layout` 컴포넌트 작성

// src/Layout.js
import { Outlet } from 'react-router-dom';

const Layout = () => {
  return (
    <div>
      <header style={{ background: 'lightgray', padding: 16, fontSize: 24 }}>
        Header
      </header>
      <main>
        <Outlet />
      </main>
    </div>
  );
};

export default Layout;

// App 컴포넌트에서 헤더를 적용할 Home, About, Profile 컴포넌트 감싸주기
import { Route, Routes } from 'react-router-dom';
import Layout from './Layout';
import About from './pages/About';
import Article from './pages/Article';
import Articles from './pages/Articles';
import Home from './pages/Home';
import Profile from './pages/Profile';

const App = () => {
  return (
    <Routes>
      <Route element={<Layout />}>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/profiles/:username" element={<Profile />} />
      </Route>
      <Route path="/articles" element={<Articles />}>
        <Route path=":id" element={<Article />} />
      </Route>
    </Routes>
  );
};

export default App;
```

---

#### index props

`Route` 컴포넌트에는 `path='/'` 와 동일한 의미의 `index` 라는 props가 있다.

```jsx
// index props 적용

// src/App.js
import { Route, Routes } from 'react-router-dom';
import Layout from './Layout';
import About from './pages/About';
import Article from './pages/Article';
import Articles from './pages/Articles';
import Home from './pages/Home';
import Profile from './pages/Profile';

const App = () => {
  return (
    <Routes>
      <Route path="/" element={<Layout />}>
        <Route index element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/profiles/:username" element={<Profile />} />
      </Route>
      <Route path="/articles" element={<Articles />}>
        <Route path=":id" element={<Article />} />
      </Route>
    </Routes>
  );
};

export default App;
```

---

<br />

## **리액트 라우터 부가기능**

리액트 라우터는 웹 애플리케이션에서 라우팅 관련 작업을 할 때 유용한 API들을 제공하는데, 자주 사용되는 것들을 알아보자.

### 1. useNavigate

`useNavigate`는 `Link` 컴포넌트를 사용하지 않고 다른 페이지로 이동해야 할 때 사용하는 Hook이다.

- 옵션
  - navigate(**-1**): 한 번 뒤로가기
  - navigate(**-2**): 두 번 뒤로가기
  - navigate(**1**): 한 번 앞으로가기 (뒤로가기를 한 상태로)
  - **{ replace: true }**: 페이지 이동 시 현재 페이지를 페이지 기록에 남기지 않음.

```jsx
// useNavigate 사용 예제

// src/Layout.jsx
import { Outlet, useNavigate } from 'react-router-dom';

const Layout = () => {
  const navigate = useNavigate();

  const goBack = () => {
    // 이전 페이지로 이동
    navigate(-1);
  };

  const goArticles = () => {
    // articles 경로로 이동
    navigate('/articles');
  };

  return (
    <div>
      <header style={{ background: 'lightgray', padding: 16, fontSize: 24 }}>
        <button onClick={goBack}>뒤로가기</button>
        <button onClick={goArticles}>게시글 목록</button>
      </header>
      <main>
        <Outlet />
      </main>
    </div>
  );
};

export default Layout;
```

---

### 2. NavLink

`NavLink` 컴포넌트는 링크에서 사용하는 경로가 현재 라우트 경로와 일치 할 경우 특정 스타일 또는 CSS 클래스를 적용하는 컴포넌트이다.

> 사용 방법

```jsx
<NavLink
  style={({isActive}) => isActive ? activeStyle : undefined}
/>

<NavLink
  className={({isActive}) => isActive ? 'active' : undefined}
/>
```

```jsx
/// NavLink 사용 예제

// src/pages/Articles.js
import { NavLink, Outlet } from 'react-router-dom';

const ArticleItem = ({ id }) => {
  const activeStyle = {
    color: 'green',
    fontSize: 21,
  };
  return (
    <li>
      <NavLink
        to={`/articles/${id}`}
        style={({ isActive }) => (isActive ? activeStyle : undefined)}
      >
        게시글 {id}
      </NavLink>
    </li>
  );
};

const Articles = () => {
  return (
    <div>
      <Outlet />
      <ul>
        <ArticleItem id={1} />
        <ArticleItem id={2} />
        <ArticleItem id={3} />
      </ul>
    </div>
  );
};

export default Articles;
```

---

### 3. NotFound 페이지 만들기

`NotFound` 페이지는 사전에 정의되지 않은 경로에 사용자가 진입했을 때 보여주는 페이지이다. 즉 페이지를 찾을 수 없을 때 보여주는 페이지이다.

```jsx
// NotFound 페이지 컴포넌트 작성

// src/pages/NotFound.js
const NotFound = () => {
  return (
    <div
      style={{
        display: 'flex',
        alignItems: 'center',
        justifyContent: 'center',
        fontSize: 64,
        position: 'absolute',
        width: '100%',
        height: '100%',
      }}
    >
      404
    </div>
  );
};

export default NotFound;

// Not Found 페이지 라우트 설정
import { Route, Routes } from 'react-router-dom';
import Layout from './Layout';
import About from './pages/About';
import Article from './pages/Article';
import Articles from './pages/Articles';
import Home from './pages/Home';
import NotFound from './pages/NotFound';
import Profile from './pages/Profile';

const App = () => {
  return (
    <Routes>
      <Route path="/" element={<Layout />}>
        <Route index element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/profiles/:username" element={<Profile />} />
      </Route>
      <Route path="/articles" element={<Articles />}>
        <Route path=":id" element={<Article />} />
      </Route>
      {/*
        상단의 규칙들을 모두 확인 후 일치하지 않는 그 외의
        모든 경로들을 적용
        */}
      <Route path="*" element={<NotFound />} />
    </Routes>
  );
};

export default App;
```

---

### 4. Navigate 컴포넌트

컴포넌트를 화면에 보여주는 순간 다른 페이지로 이동하고 싶을 때 사용.  
(= 리다이렉트)

> Navigate 예제

```jsx
// src/pages/Login.js
const Login = () => {
  return <div>로그인 페이지</div>;
};

export default Login;

// src/pages/MyPage.js
import { Navigate } from 'react-router-dom';

const MyPage = () => {
  const isLoggedIn = false;

  if (!isLoggedIn) {
    return <Navigate to="/login" replace={true} />;
  }

  return <div>마이 페이지</div>;
};

export default MyPage;

// src/App.js

import { Route, Routes } from 'react-router-dom';
import Layout from './Layout';
import About from './pages/About';
import Article from './pages/Article';
import Articles from './pages/Articles';
import Home from './pages/Home';
import Login from './pages/Login';
import MyPage from './pages/MyPage';
import NotFound from './pages/NotFound';
import Profile from './pages/Profile';

const App = () => {
  return (
    <Routes>
      <Route path="/" element={<Layout />}>
        <Route index element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/profiles/:username" element={<Profile />} />
      </Route>
      <Route path="/articles" element={<Articles />}>
        <Route path=":id" element={<Article />} />
      </Route>
      <Route path="/login" element={<Login />} />
      <Route path="/mypage" element={<MyPage />} />
      <Route path="*" element={<NotFound />} />
    </Routes>
  );
};

export default App;
```

<br />

## **정리**

리액트 라우터에 대한 전반적인 지식 습득을 위해 진행한 이 튜토리얼 프로젝트에는 문제점이 있다.  
바로 사용자가 `About` 페이지에 들어왔을 때 지금 당장 필요없는 `Profile`, `Articles` 등의 컴포넌트 코드까지 불러오는 것.

규모가 큰 프로젝트에서는 이렇게하면 매우 비효율적이므로 `코드 스플리팅` 이라는 기술을 활용해  
라우트에 따라 필요한 컴포넌트들만 불러오고, 페이지 이동 후 필요할 때만 불러오는 식의 더 효율적인 코드를 짤 수가 있다.

<br />

## **참고**

---

[velopert.log >> React Router v6 튜토리얼](https://velog.io/@velopert/react-router-v6-tutorial)
