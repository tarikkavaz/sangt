<template>
  <div v-if="!$apollo.loading" class="container section">
    <h1 class="mb-10 text-4xl">Homepage</h1>

    <div
      v-for="intro in allIntros"
      class="pt-4 pb-6 mt-10 border-t-2 border-slate-300"
    >
      <h2 class="mb-6 text-2xl font-bold text-slate-600">
        {{ intro.attributes.title }}
      </h2>
      <div
        id="editor"
        v-if="intro.attributes.content"
        v-html="intro.attributes.content"
      ></div>
      <div
        class="p-4 mb-5 bg-center bg-cover rounded-md shadow-lg h-44"
        :style="{
          backgroundImage:
            'url(' + api_url + intro.attributes.image.data.attributes.url + ')',
        }"
      ></div>
    </div>
  </div>
</template>

<script>
import introQuery from '~/apollo/queries/intro/intro'
export default {
  data() {
    return {
      loading: 0,
      api_url: process.env.strapiBaseUri,
      intro: {
        data: [],
      },
    }
  },
  computed: {
    allIntros() {
      return this.intros.data
    },
  },
  apollo: {
    $loadingKey: 'loading',
    intros: {
      prefetch: true,
      query: introQuery,
    },
  },
}
</script>
