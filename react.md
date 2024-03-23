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

MUI, shadcn/ui의 Skeleton 컴포넌트 활용

```tsx
import './Skeleton.css';

import React from 'react';

const Skeleton = () => {
  return (
    <li className="skeleton-item">
      <div>
        <div className="skeleton-img" />
      </div>
      <div className="skeleton-info">
        <p className="skeleton-name" />
        <p className="skeleton-email" />
      </div>
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

.skeleton-img::before {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  width: 30px;
  height: 100%;
  background: linear-gradient(to right, #f2f2f2, #ddd, #f2f2f2);
  animation: loading 2s infinite linear;
}

.skeleton-img {
  width: 50px;
  height: 50px;
  border-radius: 50%;
  background: #f2f2f2;
  position: relative;
  overflow: hidden;
}

.skeleton-info {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  margin-left: 10px;
}

.skeleton-name {
  width: 70%;
  height: 18px;
  background: #f2f2f2;
  position: relative;
  overflow: hidden;
}

.skeleton-name::before {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  width: 30px;
  height: 100%;
  background: linear-gradient(to right, #f2f2f2, #ddd, #f2f2f2);
  animation: loading 2s infinite linear;
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



## 유용한 라이브러리

- React Icons: https://react-icons.github.io/react-icons/ (icon 다 모아놈)
