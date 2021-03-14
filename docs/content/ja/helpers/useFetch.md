---
title: useFetch
description: 'Composition APIの中でNuxtのfetch()フックにアクセスできます'
category: Lifecycle
fullscreen: True
position: 37
---

v2.12よりも新しいNuxtのバージョンは、サーバーサイドとクライアントサイドの非同期データフェッチングが可能な [`fetch` と呼ばれるカスタムフック](https://nuxtjs.org/api/pages-fetch/) をサポートしています。

このパッケージでは以下のようにアクセスできます：

```ts
import { defineComponent, ref, useFetch } from '@nuxtjs/composition-api'
import axios from 'axios'

export default defineComponent({
  setup() {
    const name = ref('')

    const { fetch, fetchState } = useFetch(async () => {
      name.value = await axios.get('https://myapi.com/name')
    })

    // Manually trigger a refetch
    fetch()

    // Access fetch error, pending and timestamp
    fetchState

    return { name }
  },
})
```

<alert>

`useFetch` must be called synchronously within `setup()`. Any changes made to component data - that is, to properties _returned_ from `setup()` - will be sent to the client and directly loaded. Other side-effects of `useFetch` hook will not be persisted.
`useFetch` は `setup()` の中で同期的に呼ばれなければなりません。コンポーネントのdataに対して、つまり、`setup()` から _返却される_ プロパティに対してなされるいかなる変更も、クライアントに送られ直接読み込まれます。`useFetch` フックのその他の副作用は後に残りません。

</alert>

<alert type="info">

`$fetch` と `$fetchState` はインスタンスにすでに定義されています。つまり、setupから`fetch` や `fetchState` を返却する必要はありません。

</alert>

<alert type="info">

`useFetch` は `onGlobalSetup` の中で使えないことに注意してください。

</alert>
