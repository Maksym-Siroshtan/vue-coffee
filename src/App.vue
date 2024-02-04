<script setup>
import { onMounted, reactive, ref, watch } from 'vue'
import axios from 'axios'
import debounce from 'lodash.debounce'

import Header from './components/Header.vue'
import CardList from './components/CardList.vue'
import Drawer from './components/Drawer.vue'

const items = ref([])
const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})

const onChangeSelect = (event) => {
  filters.sortBy = event.target.value
}

const onChangeSearchInput = debounce((event) => {
  filters.searchQuery = event.target.value
}, 500)

const onClickFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      const favorite = {
        item_id: item.id
      }

      item.isFavorite = true
      const { data } = await axios.post('https://847f8446d0bdc264.mokky.dev/favorites', favorite)
      item.favoriteId = data.id
    } else {
      item.isFavorite = false
      await axios.delete(`https://847f8446d0bdc264.mokky.dev/favorites/${item.favoriteId}`)
      item.favoriteId = null
    }
  } catch (err) {
    console.log(err)
  }
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
    items.value = data.map((item) => ({
      ...item,
      isFavorite: false,
      favoriteId: null,
      isAdded: false
    }))
  } catch (err) {
    console.log(err)
  }
}

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get('https://847f8446d0bdc264.mokky.dev/favorites')

    items.value = items.value.map((item) => {
      const favorite = favorites.some((favorite) => {
        if (favorite.item_id === item.id) {
          item.favoriteId = favorite.id
          return true
        } else {
          return false
        }
      })

      if (!favorite) {
        return item
      }

      return {
        ...item,
        isFavorite: true
      }
    })
  } catch (err) {
    console.log(err)
  }
}

onMounted(async () => {
  await fetchItems()
  await fetchFavorites()
})

watch(filters, async () => {
  await fetchItems()
  await fetchFavorites()
})
</script>

<template>
  <!-- <Drawer /> -->
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

      <CardList :items="items" @on-click-favorite="onClickFavorite" />
    </div>
  </div>
</template>
