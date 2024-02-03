<script setup>
import axios from 'axios'
import { onMounted, reactive, ref, watch } from 'vue'

import Header from './components/Header.vue'
import CardList from './components/CardList.vue'

const items = ref([])
const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})

const onChangeSelect = (event) => {
  filters.sortBy = event.target.value
}

const onChangeSearchInput = (event) => {
  filters.searchQuery = event.target.value
}

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy
    }

    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`
    }

    const { data } = await axios.get('https://847f8446d0bdc264.mokky.dev/items', {
      params
    })
    items.value = data
  } catch (err) {
    console.log(err)
  }
}

onMounted(fetchItems)
watch(filters, fetchItems)
</script>

<template>
  <div class="max-w-7xl bg-white rounded-2xl mx-auto my-12">
    <Header />

    <div class="p-12">
      <div class="flex items-center justify-between mb-12">
        <h2 class="text-3xl font-bold">Наш асортимент кави</h2>
        <div class="flex gap-8">
          <select @change="onChangeSelect" class="border outline-none px-4 py-2 rounded-lg">
            <option value="title">За назвою</option>
            <option value="price">За ціною (зростання)</option>
            <option value="-price">За ціною (зменшення)</option>
          </select>
          <div class="relative">
            <img src="/search.svg" alt="Search" class="absolute top-3 left-4" />
            <input
              @input="onChangeSearchInput"
              type="text"
              placeholder="Пошук..."
              class="border rounded-lg pl-10 pr-4 py-2 outline-none focus:border-slate-500"
            />
          </div>
        </div>
      </div>

      <CardList :items="items" />
    </div>
  </div>
</template>
