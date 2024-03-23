## redux slice 예시

```tsx
import { createSlice } from "@reduxjs/toolkit";

const initialState = {
    isLoggedIn: false,
    email: null,
    userName: null,
    userID: null,
}

const authSlice = createSlice({
    name: 'auth',
    initialState,
    reducers: {
        SET_ACTIVE_USER: (state, action) => {
            const { email, userName, userID } = action.payload;
            state.isLoggedIn = true;
            state.email = email;
            state.userName = userName;
            state.userID = userID;
        },
        REMOVE_ACTIVE_USER: (state) => {
            state.isLoggedIn = false;
            state.email = null;
            state.userName = null;
            state.userID = null;
        }
    }
})

export const { SET_ACTIVE_USER, REMOVE_ACTIVE_USER } = authSlice.actions;

export const selectIsLoggedIn = (state) => state.auth.isLoggedIn;
export const selectEmail = (state) => state.auth.email;
export const selectUserName = (state) => state.auth.userName;
export const selectUserID = (state) => state.auth.userID;

export default authSlice.reducer;

```

## 로딩 상태 관리

loading 상태관리를 hook으로 따로 빼서 관리

react-query 추천: 위 기능 + 여러 강력한 기능, 서버 상태 관리에 좋음

```tsx
const app = () => {
    const [loading, products] = useLoading('/api/products')
    if(loading) return <p>loading...</p>
    
    return <>{products?.map(product => <div key={product.id}>{product.name}</div>)</>
}
    
const useLoading = (url: string, data: Record<string, unknown>, method: HttpMethod) => {
    const [response, setResponse] = useState(null)
    const [loading, setLoading] = useState(false)
    
    useEffect(() => {
        setLoading(true)
        fetch(url, {body: json.stringify(data), method})
        .then(res => res.json())
        .then((response) => {
            setResponse(response)
            setLoading(false)
        })
    }, [])
    
    return [loading, response]
}
```

Next에서는 Suspense로 감싸주고 fallback에 loading시 보여줄 ui를 준다.

```tsx
<Suspense fallback={<Skeleton/>}>
	{products}
</Suspense>
```

## Skeleton

1) MUI, shadcn/ui의 Skeleton 컴포넌트 활용
2) react-loading-skeleton 사용
3) react-content-loader 사용
4) 직접 구현

```tsx
import './Skeleton.css';

import React from 'react';

const Skeleton = () => {
  return (
    <li className="skeleton-item">
      <p className="skeleton-email" />
    </li>
  );
);

export default Skeleton;
```

```css
@keyframes loading {
  0% {
    transform: translateX(0);
  }
  50%,
  100% {
    transform: translateX(460px);
  }
}

.skeleton-item {
  display: flex;
  align-items: center;
  margin: 15px 0;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 4px;
  position: relative;
}

.skeleton-info {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  margin-left: 10px;
}

.skeleton-email {
  width: 85%;
  height: 18px;
  background: #f2f2f2;
  margin-top: 3px;
  position: relative;
  overflow: hidden;
}

.skeleton-email::before {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  width: 30px;
  height: 100%;
  background: linear-gradient(to right, #f2f2f2, #ddd, #f2f2f2);
  animation: loading 2s infinite linear;
}
```

## API 많은 데이터 호출 시 최적화 방식

1. 페이지네이션 (내 repo nextjs-dashboard)
   - 구조, 계층 제공
   - 레이아웃 유지
   - CMS 에 적합
2. 무한 스크롤 (내 repo react-utils useIntersectionObserver 참조)
   - 쉬움, 사용자 경험
   - 모바일에 적합
   - 사용자가 맨 하단 볼 시 맨 하단에 빈 컴포넌트 놓고, intersectionObserver 활용하여 보일 시에 fetch
   - windowing 활용해서 너무 많은 dom 렌더링 하지 않도록 해야함

상황에 따라 적절한 방식을 사용하자

- 백엔드에 페이지네이션 용으로 만든 api를 써서 2가지 방식 다 구현할 수 있다.

## 유용한 라이브러리

- React Icons: https://react-icons.github.io/react-icons/ (icon 다 모아놈)
