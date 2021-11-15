---
title: 利用 Nust.JS 建立個人 BLOG
alt: my first blog post
---
# 利用 Nust.JS 建立個人 BLOG

## 1.利用 Vue CLI 建立 Nuxt 專案

```bash
~$ npm install -g vue-cli
~$ vue init nuxt-community/starter-template <my-nuxt-app>
~$ cd <my-nuxt-app> && npm install
```

## 2.追加 @nuxt/content plugin

```bash
~$ npm i -s @nuxt/content
```

## 3.建立文章資料夾 `content` 與撰寫第一篇文章 `first-article.md`

> 檔案位置 : <my-nuxt-app>/content/articles

```markdown
---
title: My first Blog Post
description: Learning how to use @nuxt/content to create a blog
img: first-blog-post.jpg
alt: my first blog post
this is front-matter , more info : https://ithelp.ithome.com.tw/articles/10242863?sc=rss.iron
---
# 第一篇文章

這是我的第一篇 Markdown 文章 
```

## 4.加上 _slug.vue , 讀取 markdown 檔案的內容

> 檔案位置 : <my-nuxt-app>/pages/blog/_slug.vue

```vue
<template>
  <article>
    <nuxt-content :document="article" />
  </article>
</template>

<script>
    export default {
        async asyncData({ $content, params }) {
          // fetch our article here
          const article = await $content('articles', params.slug).fetch()

          return { article }
        }
    }
</script>

<style scoped>

</style>
```

## 5.查看文章內容

> 用 chrome 訪問 http://localhost:3000/blog/first-article

![](/img.png)

## 參考資料

- [Create a Blog with Nuxt Content](https://nuxtjs.org/tutorials/creating-blog-with-nuxt-content/)
- [nust 安装步骤](https://www.jianshu.com/p/d1133e980b9d)
