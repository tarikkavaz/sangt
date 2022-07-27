<template>
  <div v-if="!$apollo.loading">
    <h1 class="mb-10 text-3xl">
      {{ pages.data[0].attributes.title }}
    </h1>
    <div
      class="bg-center bg-cover rounded-md h-96"
      v-bind:style="{
        backgroundImage:
          'url(' +
          api_url +
          pages.data[0].attributes.image.data[0].attributes.url +
          ')',
      }"
    ></div>
    <div
      class="mt-12 rounded-md"
      id="editor"
      v-if="pages.data[0].attributes.content"
      v-html="pages.data[0].attributes.content"
    ></div>
  </div>
</template>

<script>
import pagesQuery from '~/apollo/queries/page/page'

export default {
  data() {
    return {
      loading: 0,
      page: {
        data: [],
      },
      image: {
        data: [],
      },
      api_url: process.env.strapiBaseUri,
    }
  },

  apollo: {
    $loadingKey: 'loading',
    pages: {
      prefetch: true,
      query: pagesQuery,
      variables() {
        return { slug: this.$route.params.slug }
      },
    },
  },
}
</script>
