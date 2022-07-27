<template>
  <div v-if="!$apollo.loading">
    <div class="container section container-large">
      <h1 class="mb-10 text-3xl">
        Category: {{ categories.data[0].attributes.name }}
      </h1>

      <div>
        <Articles :articles="categories.data[0].attributes.articles"></Articles>
      </div>
    </div>
  </div>
</template>

<script>
import categoriesQuery from '~/apollo/queries/article/articles-categories'
import Articles from '~/components/Articles'
export default {
  data() {
    return {
      loading: 0,
      categories: {
        data: [],
      },
    }
  },
  components: {
    Articles,
  },
  apollo: {
    $loadingKey: 'loading',
    categories: {
      prefetch: true,
      query: categoriesQuery,
      variables() {
        return { slug: this.$route.params.slug }
      },
    },
  },
}
</script>
