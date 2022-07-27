<template>
  <div v-if="!$apollo.loading" class="min-h-screen pt-10 bg-slate-200">
    <div class="md:container md:mx-auto">
      <div class="bg-slate-600">
        <nav class="container flex px-5 py-5 mx-auto">
          <ul class="flex flex-grow space-x-4">
            <!-- logo -->
            <li>
              <NuxtLink
                to="/"
                class="px-4 py-2 text-2xl font-semibold text-white rounded-md"
                >LOGO
              </NuxtLink>
            </li>
          </ul>
          <!-- pages -->
          <ul class="flex items-center flex-grow space-x-4">
            <li
              v-for="page in pages.data"
              :key="page.slug"
              class="hidden sm:flex"
            >
              <NuxtLink
                :to="{
                  name: 'pages-slug',
                  params: { slug: page.attributes.slug },
                }"
                class="px-3 py-1 font-semibold text-gray-200 hover:text-gray-400 text"
                >{{ page.attributes.title }}
              </NuxtLink>
            </li>
          </ul>
          <!-- dropdown -->
          <div class="relative">
            <button
              class="hidden sm:flex items-center justify-center text-white font-semibold rounded-md px-4 py-2 | hover:bg-slate-500"
              @click="dropDownCategory = true"
            >
              Articles
            </button>

            <div
              v-if="dropDownCategory"
              class="fixed inset-0 z-20 w-full h-screen bg-black opacity-25"
              @click="dropDownCategory = false"
            ></div>
            <div
              v-if="dropDownCategory"
              class="absolute right-0 z-30 mt-2"
              :class="{ hidden: !dropDownCategory }"
            >
              <div class="w-48 py-2 bg-white rounded-md shadow-lg">
                <div class="text-center text-slate-500">Select Catgory</div>
                <NuxtLink
                  class="block text-slate-600 font-semibold px-4 py-2 | hover:text-white hover:bg-slate-500"
                  to="/articles/"
                >
                  All
                </NuxtLink>
                <div v-for="category in categories.data" :key="category.slug">
                  <NuxtLink
                    :to="{
                      name: 'categories-slug',
                      params: { slug: category.attributes.slug },
                    }"
                    class="block text-slate-600 font-semibold px-4 py-2 | hover:text-white hover:bg-slate-500"
                  >
                    {{ category.attributes.name }}
                  </NuxtLink>
                </div>
              </div>
            </div>
          </div>
          <!-- mobile -->
          <ul class="flex sm:hidden" @click="showMenu = !showMenu">
            <li>
              <a href="#" class="absolute right-0 z-30 flex-none px-5">
                <p
                  class="px-2 py-2 font-semibold tracking-wide text-gray-200 rounded hover:bg-gray-900 hover:bg-opacity-30"
                >
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    fill="none"
                    viewBox="0 0 24 24"
                    stroke="currentColor"
                    class="w-6 h-6"
                  >
                    <path
                      stroke-linecap="round"
                      stroke-linejoin="round"
                      stroke-width="2"
                      d="M4 6h16M4 12h16m-7 6h7"
                    ></path>
                  </svg>
                </p>
              </a>
              <div
                class="absolute top-0 right-0 z-10 w-full h-screen transition bg-black"
                :class="{
                  'invisible opacity-0': !showMenu,
                  'visible opacity-40': showMenu,
                }"
              ></div>
              <div
                class="absolute right-0 z-20 w-full px-5 mt-16 transition"
                :class="{
                  'invisible opacity-0': !showMenu,
                  'visible opacity-100': showMenu,
                }"
              >
                <ul
                  class="flex flex-col w-full overflow-hidden bg-gray-100 rounded shadow-md"
                >
                  <li
                    v-for="page in pages.data"
                    :key="page.slug"
                    class="sm:flex"
                  >
                    <NuxtLink
                      :to="{
                        name: 'pages-slug',
                        params: { slug: page.attributes.slug },
                      }"
                      class="flex flex-1 px-8 py-4 text-lg font-semibold text-gray-700 hover:bg-gray-200"
                      >{{ page.attributes.title }}
                    </NuxtLink>
                  </li>
                </ul>
              </div>
            </li>
          </ul>
        </nav>
      </div>
    </div>
    <div class="container p-10 mx-auto rounded-md bg-slate-100">
      <div class="mt-12">
        <Nuxt />
      </div>
    </div>
  </div>
</template>

<script>
import categoriesQuery from '~/apollo/queries/category/category'
import pagesQuery from '~/apollo/queries/page/page'
export default {
  data() {
    return {
      loading: 0,
      categories: {
        data: [],
      },
      pages: {
        data: [],
      },
      showMenu: false,
      dropDownCategory: false,
    }
  },
  apollo: {
    $loadingKey: 'loading',
    categories: {
      prefetch: true,
      query: categoriesQuery,
    },
    pages: {
      prefetch: true,
      query: pagesQuery,
    },
  },
}
</script>

<style lang="scss">
body,
html {
  font-family: 'Roboto', sans-serif;
  overflow-x: hidden;
}
h1 {
  span {
    @apply text-sm;
  }
}
#editor {
  h2 {
    @apply text-slate-900 text-2xl mb-8;
  }
  p,
  li {
    @apply text-slate-800;
  }
  p {
    @apply mb-3;
  }
}
code {
  font-family: 'Fira Code', monospace;
}

@supports (font-variation-settings: normal) {
  code {
    font-family: 'Fira Code VF', monospace;
  }
}
</style>
