<script setup>
import DrawerHead from './DrawerHead.vue'
import CartItemList from './CartItemList.vue'
import DrawerFooter from './DrawerFooter.vue'
</script>

<template>
  <div class="fixed top-0 right-0 bottom-0 left-0 bg-black z-10 opacity-60"></div>
  <div class="fixed top-0 right-0 flex flex-col overflow-auto w-1/4 h-screen bg-white p-7 z-20">
    <div class="mb-7">
      <DrawerHead />
    </div>

    <div class="flex-1">
      <CartItemList />
    </div>

    <div class="mt-10">
      <DrawerFooter />
    </div>
  </div>
</template>
