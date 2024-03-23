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

## 유용한 라이브러리

- React Icons: https://react-icons.github.io/react-icons/ (icon 다 모아놈)
