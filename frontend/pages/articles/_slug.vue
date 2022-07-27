<template>
  <div v-if="!$apollo.loading">
    <h1 class="mb-10 text-3xl">
      {{ articles.data[0].attributes.title }}
    </h1>
    <div
      class="bg-center bg-cover rounded-md h-96"
      v-bind:style="{
        backgroundImage:
          'url(' +
          api_url +
          articles.data[0].attributes.image.data.attributes.url +
          ')',
      }"
    ></div>

    <div
      class="mt-12 rounded-md"
      id="editor"
      v-if="articles.data[0].attributes.content"
      v-html="articles.data[0].attributes.content"
    ></div>
    <NuxtLink
      :to="{
        name: 'categories-slug',
        params: {
          slug: articles.data[0].attributes.category.data.attributes.slug,
        },
      }"
      v-if="articles.data[0].attributes.category.data.attributes.name"
      class="text-sm uppercase"
    >
      <code class="text-xs">{{
        articles.data[0].attributes.category.data.attributes.name
      }}</code>
    </NuxtLink>
  </div>
</template>

<script>
import articlesQuery from '~/apollo/queries/article/article'

export default {
  data() {
    return {
      loading: 0,
      categories: {
        data: [],
      },
      article: {
        data: [],
      },
      api_url: process.env.strapiBaseUri,
    }
  },

  apollo: {
    $loadingKey: 'loading',
    articles: {
      prefetch: true,
      query: articlesQuery,
      variables() {
        return { slug: this.$route.params.slug }
      },
    },
  },
}
</script>
