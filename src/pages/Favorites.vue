<script setup>
import { computed, onMounted, ref } from 'vue'
import axios from 'axios'

import InfoBlock from '../components/InfoBlock.vue'
import CardList from '../components/CardList.vue'

const favorites = ref([])
const isHaveFavorites = computed(() => favorites.value.length > 0)

onMounted(async () => {
  try {
    const { data } = await axios.get(
      'https://847f8446d0bdc264.mokky.dev/favorites?_relations=items'
    )

    favorites.value = data.map(({ item }) => ({
      ...item,
      isFavorite: true
    }))
  } catch (err) {
    console.log(err)
  }
})
</script>

<template>
  <div v-if="isHaveFavorites">
    <div class="flex items-center gap-3 mb-12">
      <router-link to="/">
        <img
          src="/arrow-right.svg"
          alt=""
          class="border p-4 rotate-180 rounded-xl cursor-pointer hover:-translate-x-0.5 transition-all"
        />
      </router-link>
      <h2 class="text-3xl font-bold">Обрана кава</h2>
    </div>

    <CardList :items="favorites" />
  </div>
  <div v-else>
    <InfoBlock
      title="Обраного немає"
      description="Ви нічого не додавали."
      image-url="/emoji-1.png"
    />
  </div>
</template>
