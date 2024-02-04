<script setup>
import DrawerHead from './DrawerHead.vue'
import CartItemList from './CartItemList.vue'
</script>

<template>
  <div class="fixed top-0 right-0 bottom-0 left-0 bg-black z-10 opacity-60"></div>
  <div class="fixed top-0 right-0 flex flex-col overflow-auto w-1/4 h-screen bg-white p-7 z-20">
    <DrawerHead />

    <div class="flex-1">
      <CartItemList />
    </div>

    <div class="flex flex-col gap-5 mt-10">
      <div class="flex gap-2">
        <span class="text-xl">Разом:</span>
        <div class="flex-1 border-b border-dashed border-slate-200"></div>
        <b class="text-2xl">10000 грн.</b>
      </div>
      <div class="flex gap-2">
        <span class="text-xl">Податок 5%:</span>
        <div class="flex-1 border-b border-dashed border-slate-200"></div>
        <b class="text-2xl">500 грн.</b>
      </div>

      <button
        class="relative bg-lime-300 text-white w-full rounded-2xl text-lg py-4 hover:bg-lime-500"
      >
        Оформити замовлення
        <img src="/arrow-next.svg" alt="Arrow-right" class="absolute top-6 right-6" />
      </button>
    </div>
  </div>
</template>
