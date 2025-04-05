## Set Up

依存関係をインストールします。

### Install Dependencies
```shell
pnpm install
```

### Change Placeholder
使用する際に、`example`が付けられている値を正しいものに変更してください。

## How to use in Hono

```typescript
app.use(async (c, next) => {
  const authClient = createAuthClient({
    baseURL: 'https://auth.example.com',
  })

  const result = await authClient.getSession({
    fetchOptions: {
      headers: c.req.raw.headers,
    },
  })

  if (!result.data) {
    return c.json({error: 'Unauthorized'}, 401)
  }

  await next()
})
```

## How to use in React

```tsx
// src/lib/auth.ts
import {createAuthClient} from 'better-auth/react'
import {env} from '@/lib/env.ts'

export const authClient = createAuthClient({
  baseURL: 'https://auth.example.com',
})
```

```tsx
// src/layouts/auth-layout.tsx
import {Navigate, Outlet} from 'react-router-dom'
import {authClient} from '@/lib/auth.ts'

export const AuthLayout = () => {
  const {data: session, isPending, error} = authClient.useSession()

  if (isPending) {
    return <div>Loading...</div>
  }

  if (error || !session) {
    return <Navigate to="/login" replace/>
  }

  return <Outlet/>
}
```


## Commands

### better-auth:generate

better-authのDDLをdumpします。wranglerを使用してマイグレーションを行えるようにするため、 適当にdatabase.sqliteというファイルを指定し、`pnpm dlx @better-auth/cli@latest generate`を実行します。

```shell
pnpm run better-auth:generate
```

### wrangler:migrate:local:list
ローカルデータベースへのマイグレーションを一覧表示します。

```shell
pnpm run wrangler:migrate:local:list
```

### wrangler:migrate:local:apply
ローカルデータベースへのマイグレーションを適用します。

```shell
pnpm run wrangler:migrate:local:apply
```

### wrangler:migrate:production:list
リモートデータベースへのマイグレーションを一覧表示します。

```shell
pnpm run wrangler:migrate:production:list
```

### wrangler:migrate:production:apply
リモートデータベースへのマイグレーションを適用します。

```shell
pnpm run wrangler:migrate:production:apply
```
