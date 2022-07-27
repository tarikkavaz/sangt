<template>
  <div>
    <div v-if="getArticle">
      <p>{{ getArticle.attributes.slug }}</p>
      <h1 class="mb-10 text-3xl" v-if="getArticle">
        {{ getArticle.attributes.title }}
      </h1>

      <img
        :src="api_url + article.data.attributes.image.data.attributes.url"
        class="h-28"
      />
    </div>
    <div class="section">
      <div class="container mt-12" id="editor">
        <div
          v-if="article.data.attributes.content"
          v-html="article.data.attributes.content"
        ></div>
        <p v-if="article.data.attributes.publishedAt">
          {{ article.data.attributes.publishedAt }}
        </p>
      </div>
    </div>
  </div>
</template>

<script>
import articleQuery from '~/apollo/queries/article/article'

export default {
  data() {
    return {
      article: {
        data: [],
      },
      api_url: process.env.strapiBaseUri,
    }
  },
  computed: {
    getArticle() {
      return this.article.data.attributes ? this.article.data : null
    },
  },
  apollo: {
    article: {
      prefetch: true,
      query: articleQuery,
      variables() {
        return { id: parseInt(this.$route.params.id) }
      },
    },
  },
}
</script>
