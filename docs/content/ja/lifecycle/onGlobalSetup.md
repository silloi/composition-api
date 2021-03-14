---
title: onGlobalSetup
description: 'グローバルなNuxtのsetup()関数で関数（またはdata）を走らせます。'
category: Lifecycle
fullscreen: True
position: 21
---

グローバルなsetup関数でコールバック関数を走らせます。

```ts [~/plugins/myPlugin.js]
import { onGlobalSetup, provide } from '@nuxtjs/composition-api'

export default () => {
  onGlobalSetup(() => {
    provide('globalKey', true)
  })
}
```

<alert>componentコンテキストの中ではなくpluginの中で呼ばないといけません。</alert>
