---
title: useStatic
description: 'サイト生成時に静的なJSONを作り出す非同期通信を定義できます。'
category: API
position: 39
---

`useStatic` を使って処理コストの高い関数をあらかじめ走らせることができます。

```ts
import {
  defineComponent,
  useContext,
  useStatic,
  computed,
} from '@nuxtjs/composition-api'
import axios from 'axios'

export default defineComponent({
  setup() {
    const { params } = useContext()
    const id = computed(() => params.value.id)
    const post = useStatic(
      id => axios.get(`https://jsonplaceholder.typicode.com/posts/${id}`),
      id,
      'post'
    )

    return { post }
  },
})
```

## SSG

もしアプリ全体を生成する（あるいは単にいくつかのルートを `nuxt build && nuxt generate --no-build` でプリレンダリングする）なら、以下の挙動が解禁されます：

- 生成時、 `useStatic` で呼んだ結果はJSONファイルに保存され、 `/dist` ディレクトリの中にコピーされます。
- 生成されたページのハードリロード時、JSONはページにインラインで埋め込まれ、キャッシュされ、キャッシュされます。
- 生成されたページへのクライアント遷移時、このJSONはフェッチされます。一度フェッチされると、後続の遷移のためにキャッシュされます。ページが事前に生成 _されていなかった_ 時など、いかなる理由でこのJSONが存在しないような場合も、オリジナルの生成関数はクライアントサイドで走ります。

<alert>

もしアプリ内でいくつかのページを事前に生成するなら、`generate.interval` を増やす必要があることに注意してください。（[setupの説明](/getting-started/setup) を参照。）

</alert>

## SSR

もしルートが事前に生成されていないなら（devモードの場合を含む）：

- ハードリロード時、サーバーは生成関数を走らせ、`nuxtState` の結果をインラインで埋め込みます。クライアントがAPIリクエストを再び走らせることのないようにするためです。結果は次のリクエストまでの間キャッシュされます。
- クライアント遷移時、クライアントは生成関数を走らせます。

どちらの場合も、 `useStatic` が返す結果は `null` のrefで、生成関数やJSONフェッチが解決されたとき、その結果によって埋められます。
