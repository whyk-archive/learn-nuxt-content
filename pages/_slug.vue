<template>
  <div class="container">
    <nuxt-content :document="roots" />

    <aside>
      <h2>直近の更新</h2>
      <section v-for="(article, index) in articles" :key="index">
        <h3>{{ article.title }}</h3>
        <nuxt-link :to="article.path">リンク先へ行く</nuxt-link>
      </section>
    </aside>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
  async asyncData({ $content, params }) {
    const roots = await $content(params.slug || 'index').fetch()
    const articles = await $content('articles')
      .sortBy('updatedAt')
      .limit(5)
      .fetch()
    return {
      roots,
      articles,
    }
  },
})
</script>

<style>
.container {
  max-width: 1000px;
  width: 100%;
  margin: 0 auto;
}
</style>
