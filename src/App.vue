<script setup>
import { computed, onMounted, reactive, ref, provide, watch } from 'vue'
import axios from 'axios'
import debounce from 'lodash.debounce'

import Header from './components/Header.vue'
import CardList from './components/CardList.vue'
import Drawer from './components/Drawer.vue'

const items = ref([])
const filters = reactive({
  sortBy: '',
  searchQuery: ''
})

const cartItems = ref([])
const isDrawerOpen = ref(false)
const isOrderCreating = ref(false)
const orderId = ref(null)
const cartIsEmpty = computed(() => cartItems.value.length === 0 && !orderId.value)

const totalPrice = computed(() => cartItems.value.reduce((total, item) => total + item.price, 0))
const vatPrice = computed(() => Math.round((totalPrice.value * 5) / 100))

const openTheDrawer = () => {
  isDrawerOpen.value = true
}
const closeTheDrawer = () => {
  isDrawerOpen.value = false
}

const createOrder = async () => {
  try {
    isOrderCreating.value = true
    const { data } = await axios.post('https://847f8446d0bdc264.mokky.dev/orders', {
      order: cartItems.value,
      totalPrice: totalPrice.value
    })

    cartItems.value = []

    orderId.value = data.id
  } catch (err) {
    console.log(err)
  } finally {
    isOrderCreating.value = false
    setTimeout(() => {
      orderId.value = null
    }, 5000)
  }
}

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

const addItemToCart = (item) => {
  cartItems.value.push(item)
  item.isAdded = true
}

const removeItemFromCart = (item) => {
  cartItems.value.splice(cartItems.value.indexOf(item), 1)
  item.isAdded = false
}

const onCLickOnPlus = (item) => {
  if (!item.isAdded) {
    addItemToCart(item)
  } else {
    removeItemFromCart(item)
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

const checkAddedToCart = () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: cartItems.value.some((cartItem) => cartItem.id === item.id)
  }))
}

onMounted(async () => {
  const localCartItems = localStorage.getItem('cart')
  cartItems.value = localCartItems ? JSON.parse(localCartItems) : []

  const localSortByFilter = localStorage.getItem('filters')
  filters.sortBy = localSortByFilter ? JSON.parse(localSortByFilter) : 'title'

  await fetchItems()
  await fetchFavorites()

  checkAddedToCart()
})

watch(filters, async () => {
  localStorage.setItem('filters', JSON.stringify(filters.sortBy))

  await fetchItems()
  await fetchFavorites()

  checkAddedToCart()
})

watch(
  cartItems,
  () => {
    localStorage.setItem('cart', JSON.stringify(cartItems.value))
  },
  {
    deep: true
  }
)

watch(cartItems, () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: false
  }))
})

provide('cart', {
  cartItems,
  closeTheDrawer,
  removeItemFromCart,
  createOrder,
  isOrderCreating,
  totalPrice,
  vatPrice
})
</script>

<template>
  <Drawer v-if="isDrawerOpen" :cart-is-empty="cartIsEmpty" :order-id="orderId" />
  <div class="max-w-7xl bg-white rounded-2xl mx-auto my-12">
    <Header @open-the-drawer="openTheDrawer" :total-price="totalPrice" />

    <div class="p-12">
      <div class="flex items-center justify-between mb-12">
        <h2 class="text-3xl font-bold">Наш асортимент кави</h2>
        <div class="flex gap-8">
          <select
            @change="onChangeSelect"
            v-model="filters.sortBy"
            class="border outline-none px-4 py-2 rounded-lg"
          >
            <option value="title">За назвою</option>
            <option value="price">За ціною (зростання)</option>
            <option value="-price">За ціною (зменшення)</option>
          </select>
          <div class="relative">
            <img src="/search.svg" alt="Search" class="absolute top-3 left-4" />
            <input
              @input="onChangeSearchInput"
              v-model="filters.searchQuery"
              type="text"
              placeholder="Пошук..."
              class="border rounded-lg pl-10 pr-4 py-2 outline-none focus:border-slate-500"
            />
          </div>
        </div>
      </div>

      <CardList
        :items="items"
        @on-click-favorite="onClickFavorite"
        @on-click-on-plus="onCLickOnPlus"
      />
    </div>
  </div>
</template>
