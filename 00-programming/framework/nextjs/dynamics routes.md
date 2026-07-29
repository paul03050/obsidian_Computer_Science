
使用方括號來參數化段落。使用 ` `[segment]` ` 表示單個參數，` `[...segment]` ` 表示通配符，` `[[...segment]]` ` 表示可選的通配符。通過 ` [`params`](https://nextjs.org/docs/app/api-reference/file-conventions/page#params-optional) ` 屬性訪問值。

| 路徑                              | URL 模式                                                               |
| ------------------------------- | -------------------------------------------------------------------- |
| `app/blog/[slug]/page.tsx`      | `/blog/my-first-post`                                                |
| `app/shop/[...slug]/page.tsx`   | `/shop/clothing`, `/shop/clothing/shirts`                            |
| `app/docs/[[...slug]]/page.tsx` | `/docs`, `/docs/layouts-and-pages`, `/docs/api-reference/use-router` |

---

在 JavaScript 中，`const {id} = await params` 使用了 **[[物件解構賦值]]**。是否能不加 `{}` 取決於 `await params` 解析出來的資料型態：

