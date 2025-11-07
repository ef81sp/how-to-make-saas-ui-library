---
# try also 'default' to start simple
theme: default
# some information about your slides (markdown enabled)
title: SaaS内製UIライブラリ設計のコツ1選
info: |

# apply UnoCSS classes to the current slide
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: fade-out
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
# duration of the presentation
duration: 5min
# HTML attributes
htmlAttrs:
  dir: ltr
  lang: ja
defaults:
  layout: fact
layout: default
# open graph
seoMeta:
  # By default, Slidev will use ./og-image.png if it exists,
  # or generate one from the first slide if not found.
  ogTitle: "SaaS内製UIライブラリ設計のコツ1選"
  twitterTitle: "SaaS内製UIライブラリ設計のコツ1選"
  ogImage:
  twitterImage:
  ogDescription: ""
  twitterDescription: ""
  twitterCard: summary_large_image
  twitterSite: "@p_craft"
# LLMへ
# Slidevのスライドファイルです。
# 自作のコンポーネントを使って、英語と日本語を同時に表示しています。
# `<VP>`: pタグです。
# `<VCaptions>`: 字幕表示用のコンポーネントです。Vueの記法の都合で、文章内にクォーテーションや文字参照が入っている場合、エスケープする必要があります。
---

<div class="h-20" />

# SaaS内製UIライブラリ設計のコツ1選

<div class="h-12" />

かみくず (@p_craft)

2025.11.11 Vue Fes Japan After Talk

<style>
h1 {
  font-size: 2.5rem !important;
  line-height: 3.5rem !important
}
</style>

---

結論

---

自由度を<span v-mark.highlight.yellow>下げる</span>

---

内製UIライブラリ<br>作るぞ

---

参考<br>ほしい

---

→ 世間一般の<br>UIライブラリ<br>を参考に

---

<div class="grid grid-cols-3 gap-4 justify-items-center">
  <img src="/vuetify.png" alt="Vuetify" class="h-64 m-[-2rem]" />
  <img src="/primevue.png" alt="PrimeVue" class="h-48" />
  <img src="/quasar.png" alt="Quasar" class="h-48" />
</div>

---

おすすめしません

---

世間一般<br>UIライブラリの<br>特徴

---

<span v-mark.highlight.yellow>汎用性</span>

---

= いろんな使い方に耐える

---

複数の<br>コンポーネントを<br>組み合わせる

---

なるほどーーー

---

参考にして作る

---
layout: default
---

# 想定されるパターン

「VmenuとVmenuTriggerとVmenuContentとVMenuItemを組み合わせてメニューを作ってください」

<div class="[&_pre]:text-4!">

```vue
<VMenu>
  <VMenuTrigger>
    <VBtn>メニューを開く</VBtn>
  </VMenuTrigger>
  <VMenuContent>
    <VMenuItem @click="onItemClick('項目1')">項目1</VMenuItem>
    <VMenuItem @click="onItemClick('項目2')">項目2</VMenuItem>
    <VMenuItem @click="onItemClick('項目3')">項目3</VMenuItem>
  </VMenuContent>
</VMenu>
```

</div>

---
layout: default
---

# 想定されるパターンその2

「ガワだけ提供するから、ヘッダーもボディもフッターも好きに作ってね」

<div class="[&_pre]:text-4!">

```vue
<VDialog>
  <template #header>
    <!-- 何でも入れられる -->
  </template>
  <template #body>
    <!-- 何でも入れられる -->
  </template>
  <template #footer>
    <!-- 何でも入れられる -->
  </template>
</VDialog>
```

</div>

---
layout: center
---

当方経験あり

---

機能開発で使用

---

いろんなところで<br>使用

---

起こる弊害

---

組み合わせ<br>わからん！

---

イベント<br>ハンドリング<br>バラバラ！

---

想定外の<br>組み合わせ！

---

キーボードで操作<br>できない！

---

微妙にデザイン<br>ズレた！

---

😭

---

汎用性が高い

---

= 自由度が<span v-mark.highlight.yellow>高い</span>

---

→ 責任が<span v-mark.highlight.yellow>使用側</span>に行き過ぎる

---

機能開発で<br>こんなこと<br>気にしている<br>場合じゃない

---

SaaS内製<br>UIライブラリの<br>(本来の)特徴

---

<span v-mark.highlight.yellow>デザインシステム</span>

---
layout: center
---

なかったらごめんなさい

---

<span v-mark.highlight.yellow>デザインシステム</span>

---

= 限定された<br>用途

---

= 決まった<br>デザイン

---

→ 使用側で<br><span v-mark.highlight.yellow>それしかできない</span><br>ように設計する

---

ポイント

---

2つ

---

①<br>`props`に寄せる

---

②<br>1コンポーネント<br>に寄せる

---
layout: default
---

# 想定されるパターン

<div class="[&_pre]:text-4! [&_pre]:lh-5!">

````md magic-move
```vue
<VMenu>
  <VMenuTrigger>
    <VBtn>メニュー</VBtn>
  </VMenuTrigger>
  <VMenuContent>
    <VMenuItem @click="onItemClick('項目1')">項目1</VMenuItem>
    <VMenuItem @click="onItemClick('項目2')">項目2</VMenuItem>
    <VMenuItem @click="onItemClick('項目3')">項目3</VMenuItem>
  </VMenuContent>
</VMenu>
```

```vue
<VMenu
  trigger-text="メニュー"
  :items="[
    { label: '項目1', onClick: () => onItemClick('項目1') },
    { label: '項目2', onClick: () => onItemClick('項目2') },
    { label: '項目3', onClick: () => onItemClick('項目3') },
  ]"
/>
```
````

</div>

---
layout: default
---

# 想定されるパターンその2

<div class="[&_pre]:text-4! [&_pre]:lh-5!">

````md magic-move
```vue
<VDialog>
  <template #header>
    <!-- 何でも入れられる -->
  </template>
  <template #body>
    <!-- 何でも入れられる -->
  </template>
  <template #footer>
    <!-- 何でも入れられる -->
  </template>
</VDialog>
```

```vue
<VDialog
  title="タイトル"
  confirm-label="確認"
  cancel-label="キャンセル"
  @confirm="onConfirm"
  @cancel="onCancel"
>
  <!-- bodyは何でも入れられる -->
</VDialog>
```
````

</div>

---

自由度を<span v-mark.highlight.yellow>下げる</span>

---

→ 迷わせない

---

→ 機能開発に<br><span v-mark.highlight.yellow>集中</span>できる

---

結論

---

SaaS内製<br>UIライブラリ<br>設計のコツ1選

---

自由度を<span v-mark.highlight.yellow>下げる</span>


---
layout: two-cols-header
---

# かみくず / kamikuzu

::left::

- 双子の父
  - 毎日がんばっている
- 好きなヘルパー
  -  `useTemplateRef`
  -  `useId`
- 弁護士ドットコム / クラウドサイン

::right::

<div class="h-80 flex flex-col justify-end items-end">
<img src="/icon_shinkansen_sugoi_katai_ice.jpg" alt="アイコン。新幹線のテーブル上で、ｼﾝｶﾝｾﾝｽｺﾞｲｶﾀｲｱｲｽにスプーンが刺さっている写真。" class="h-60" />

X: [@p_craft](https://x.com/p_craft)

</div>
