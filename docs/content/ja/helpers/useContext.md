---
title: useContext
description: 'Composition API中のNuxtコンテキストにアクセスできます'
category: API
fullscreen: True
position: 36
---

`useContext` はNuxtのコンテキストを返します。これを使うことでNuxtコンテキストにより容易にアクセスできます。

```ts
import { defineComponent, useContext } from '@nuxtjs/composition-api'

export default defineComponent({
  setup() {
    const { store } = useContext()
    store.dispatch('myAction')
  },
})
```

<alert type="info">`route`、 `query`、 `from` および `params` はリアクティブなref（`.value` でアクセスできる）ですが、それ以外のコンテキストは異なることに注意してください。</alert>

<alert type="warning">Nuxt 3へのアップグレードをスムーズにするため、 it is recommended not to access `route`, `query`, `from` および `params` には `useContext` からアクセスせずに  `useRoute` ヘルパー関数を使うことが推奨されます。</alert>
