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

> 檔案位置 : `<my-nuxt-app>/content/articles`

```markdown
# 第一篇文章

這是我的第一篇 Markdown 文章 
```

## 4.加上 _slug.vue , 讀取 markdown 檔案的內容

> 檔案位置 : `<my-nuxt-app>/pages/blog/_slug.vue`

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

![](https://i.imgur.com/FolHEyy.png)


## 6.追加 front-matter 來設定文章的頁面訊息

- front-matter 的相關訊息 : 
- this is front-matter , more info : https://ithelp.ithome.com.tw/articles/10242863?sc=rss.iron

> In first-article.md

```markdown
---
title: My first Blog Post
description: Learning how to use @nuxt/content to create a blog
img: first-blog-post.jpg
alt: my first blog post  
---


# 第一篇文章

這是我的第一篇 Markdown 文章 
```

> In _slug.vue

在 `_slug.vue` 我們需要將 front-matter 的資訊取出 , 並將其放入 head 中

```vue
<template>
  <article>
    <nuxt-content :document="article"/>
  </article>
</template>

<script>
export default {
  async asyncData({$content, params}) {
    // fetch our article here
    const article = await $content('articles', params.slug).fetch()

    return {article}
  },
  // 用於設定 head 訊息 
  head() {

    // Add dynamic metas based on title and description defined in the front-matter:
    return {
      title: this.article.title,
      meta: [
        {hid: 'description', name: 'description', content: this.article.description},
        // Open Graph
        {hid: 'og:title', property: 'og:title', content: this.article.title},
        {hid: 'og:description', property: 'og:description', content: this.article.description},
        // Twitter Card
        {hid: 'twitter:title', name: 'twitter:title', content: this.article.title},
        {hid: 'twitter:description', name: 'twitter:description', content: this.article.description}
      ]
    }
  }
}
</script>
```

## 樣式設定

`<nuxt-content :document="article"/>` 中的內容都會在 `<div class="nuxt-content">` 裡面

因此如下設定設定 h1 . h2 . h3 . h4 . h5 . h6 的樣式 

```vue
<style>
/deep/ .nuxt-content h1 {
  font-weight: bold;
  color:#333;
  font-size: 36px;
}

::v-deep .nuxt-content h2 {
  font-weight: bold;
  font-size: 28px;
}

>>> .nuxt-content h3 {
  font-weight: bold;
  font-size: 22px;
}

/deep/ .nuxt-content p {
  margin-bottom: 20px;
}

</style>
```

## 參考資料

- [Create a Blog with Nuxt Content](https://nuxtjs.org/tutorials/creating-blog-with-nuxt-content/)
- [nuxt 安装步骤](https://www.jianshu.com/p/d1133e980b9d)
