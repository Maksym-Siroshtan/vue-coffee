<script setup>
import { computed, ref, provide, watch } from 'vue'
import axios from 'axios'

import Header from './components/Header.vue'
import Drawer from './components/Drawer.vue'

/* Cart Logic start*/
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

const addItemToCart = (item) => {
  cartItems.value.push(item)
  item.isAdded = true
}

const removeItemFromCart = (item) => {
  cartItems.value.splice(cartItems.value.indexOf(item), 1)
  item.isAdded = false
}

watch(
  cartItems,
  () => {
    localStorage.setItem('cart', JSON.stringify(cartItems.value))
  },
  {
    deep: true
  }
)

provide('cart', {
  cartItems,
  closeTheDrawer,
  addItemToCart,
  removeItemFromCart,
  createOrder,
  isOrderCreating,
  totalPrice,
  vatPrice
})

/* Cart Logic end*/
</script>

<template>
  <Drawer v-if="isDrawerOpen" :cart-is-empty="cartIsEmpty" :order-id="orderId" />
  <div class="max-w-7xl bg-white rounded-2xl mx-auto my-12">
    <Header @open-the-drawer="openTheDrawer" :total-price="totalPrice" />

    <div class="p-12">
      <router-view></router-view>
    </div>
  </div>
</template>
