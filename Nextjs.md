## Route group에서 공유하는 notfound 만들기
1. 해당 라우트그룹 디렉토리에 [...not-found] 디렉토리 생성 (catch-all segment 활용)
2. page.tsx 생성
```tsx
import { notFound } from "next/navigation"

export default function NotFoundDummy() {
  notFound() // notFound로 라우팅
}
```
3. 라우트그룹 디렉토리에 notFound.tsx 생성
```tsx
function NotFound() {
  return (
    <div className="flex size-full flex-col items-center justify-center gap-2">
      페이지가 없어요~
    </div>
  )
}

export default NotFound
```  
