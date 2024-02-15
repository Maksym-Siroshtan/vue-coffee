<script setup>
import { inject } from 'vue'

const { totalPrice, vatPrice } = inject('cart')
</script>

<template>
  <div class="flex flex-col gap-5">
    <div class="flex gap-2">
      <span class="text-xl">Разом:</span>
      <div class="flex-1 border-b border-dashed border-slate-200"></div>
      <b class="text-2xl">{{ totalPrice }} грн.</b>
    </div>
    <div class="flex gap-2">
      <span class="text-xl">Податок 5%:</span>
      <div class="flex-1 border-b border-dashed border-slate-200"></div>
      <b class="text-2xl">{{ vatPrice }} грн.</b>
    </div>

    <button
      class="relative bg-lime-300 text-white w-full rounded-2xl text-lg py-4 hover:bg-lime-500"
    >
      Оформити замовлення
      <img src="/arrow-next.svg" alt="Arrow-right" class="absolute top-6 right-6" />
    </button>
  </div>
</template>
