---
title: useAsync
description: '一度だけ走ってクライアントサイドでデータを維持する非同期関数を定義できます。'
category: API
fullscreen: True
position: 35
---

`useAsync` を用いて非同期通信に依存するリアクティブな値を作成できます。

サーバー上では、このヘルパーは非同期通信の結果をHTMLに埋め込み、またクライアントモードに自動的に注入します。`asyncData` とまったく同様に、クライアントサイドで非同期通信を再び走らせません。

しかしながら、SSRで通信が実行されなければ（たとえば初期ロードの後でページを遷移した場合）、非同期通信が解決されたときにその結果を埋めるような `null` のrefを返します。

```ts
import { defineComponent, useAsync, useContext } from '@nuxtjs/composition-api'

export default defineComponent({
  setup() {
    const { $http } = useContext()
    const posts = useAsync(() => $http.$get('/api/posts'))

    return { posts }
  },
})
```

<alert>

そのとき、 `useAsync` は1回限りの用途にのみ適しており、その限りでユニークなキーを提供します。 [詳しい情報](/getting-started/gotchas#keyed-functions).

</alert>
