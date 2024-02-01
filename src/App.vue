<script setup>
import { onMounted, ref } from 'vue'

import Header from './components/Header.vue'
import CardList from './components/CardList.vue'

const items = ref([])

onMounted(() => {
  fetch('https://847f8446d0bdc264.mokky.dev/items')
    .then((res) => res.json())
    .then((data) => {
      items.value = data
    })
})
</script>

<template>
  <div class="max-w-7xl bg-white rounded-2xl mx-auto my-12">
    <Header />

    <div class="p-12">
      <h2 class="text-3xl font-bold mb-12">Вся кава</h2>

      <CardList :items="items" />
    </div>
  </div>
</template>
