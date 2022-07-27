<template>
  <div v-if="!$apollo.loading" class="container section">
    <h1 class="mb-10 text-4xl">Articles</h1>
    <Articles :articles="articles"></Articles>
  </div>
</template>

<script>
import articlesQuery from '~/apollo/queries/article/articles'
import Articles from '~/components/Articles'
export default {
  data() {
    return {
      loading: 0,
      articles: {
        data: [],
      },
    }
  },
  components: {
    Articles,
  },
  apollo: {
    $loadingKey: 'loading',
    articles: {
      prefetch: true,
      query: articlesQuery,
    },
  },
}
</script>
