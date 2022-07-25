<template>
  <h1>{{ title }}</h1>
  <div v-html="contents"></div>
  <span v-if="phoneNumber">{{ phoneNumber }}</span>
</template>

<script setup>
import useFetch from '@/composables/use-fetch'
import { throwError } from '@/modules/utils'

const title = ref(null)
const contents = ref(null)

const phoneNumber = ref(null)

const { data } = await useFetch(`/wp-json/api/v1/page/contact`)
if (data === null) {
  throwError('Sorry, we cannot find the requested page.', 404)
}

title.value = data.title
contents.value = data.contents
phoneNumber.value = data.number

useHead({
  title: data.meta.primary.title,
  meta: [
    {
      name: 'description',
      content: data.meta.primary.description
    },
    {
      property: 'og:title',
      content: data.meta.open_graph.title
    },
    {
      property: 'og:description',
      content: data.meta.open_graph.description
    },
    {
      property: 'og:image',
      content: data.meta.open_graph.image
    },
  ],
})
</script>

