<template>
  <div>
    <div v-if="!$apollo.loading">
      <div class="grid grid-cols-1 gap-4 md:grid-cols-2">
        <router-link
          v-for="article in allArticles"
          :to="{
            name: 'articles-slug',
            params: { slug: article.attributes.slug },
          }"
          class="grid-cols-1"
          :key="article.id"
        >
          <div
            class="p-4 mb-5 bg-center bg-cover rounded-md shadow-lg h-44 card"
            :style="{
              backgroundImage:
                'url(' +
                api_url +
                article.attributes.image.data.attributes.url +
                ')',
            }"
          >
            <p
              class="px-6 py-3 mt-5 text-2xl font-bold text-gray-200 rounded-md md:inline-flex bg-slate-600 hover:bg-gray-200 hover:text-slate-600"
            >
              {{ article.attributes.title }}
            </p>
          </div>
        </router-link>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      loading: 0,
      api_url: process.env.strapiBaseUri,
    }
  },
  apollo: {
    $loadingKey: 'loading',
  },
  props: {
    articles: Object,
  },
  computed: {
    allArticles() {
      return this.articles.data
    },
  },
}
</script>
