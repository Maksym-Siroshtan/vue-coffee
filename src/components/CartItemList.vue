<script setup>
import { inject } from 'vue'
import CartItem from './CartItem.vue'

const { cartItems, removeItemFromCart } = inject('cart')
</script>

<template>
  <ul v-auto-animate class="flex flex-col gap-6">
    <CartItem
      v-for="item in cartItems"
      :key="item.id"
      :title="item.title"
      :price="item.price"
      :image-url="item.imageUrl"
      @remove-item-from-cart="removeItemFromCart(item)"
    />
  </ul>
</template>
