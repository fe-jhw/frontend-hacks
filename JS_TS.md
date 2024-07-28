## api url 에 searchParams 넣는 2가지 방법
1. String literal 활용
   `baseurl/domain?query=${query}&param=${param}` 
3. URL 객체 활용
   ```typescript
   const url = new URL(domain ,base_url)
   url.searchParams.set('query',query)
   url.searchParams.set('param',param)
   ```
