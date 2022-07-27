<template>
  <div v-if="!$apollo.loading">
    <div class="container section container-large">
      <h1 class="mb-10 text-3xl">
        {{ category.data.attributes.name }}
      </h1>
      <Articles :articles="category.data.attributes.articles"></Articles>
    </div>
  </div>
</template>

<script>
import articlesQuery from '~/apollo/queries/article/articles-categories'
import Articles from '~/components/Articles'
export default {
  data() {
    return {
      loading: 0,
      category: {
        data: [],
      },
    }
  },
  components: {
    Articles,
  },
  apollo: {
    $loadingKey: 'loading',
    category: {
      prefetch: true,
      query: articlesQuery,
      variables() {
        return { id: parseInt(this.$route.params.id) }
      },
    },
  },
}
</script>
