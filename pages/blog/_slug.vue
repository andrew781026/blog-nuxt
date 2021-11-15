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

<style scoped>
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

.icon.icon-link {
  /*background-image: url('~assets/svg/icon-hashtag.svg');*/
  display: inline-block;
  width: 20px;
  height: 20px;
  background-size: 20px 20px;
}
</style>
