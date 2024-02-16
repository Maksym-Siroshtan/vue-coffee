<script setup>
import { inject } from 'vue'

defineProps({
  title: String,
  description: String,
  imageUrl: String
})

const { closeTheDrawer } = inject('cart')
</script>

<template>
  <div class="flex flex-col items-center w-5/6 m-auto">
    <img :src="imageUrl" alt="Package-icon" class="w-44 mb-8" />
    <h2 class="text-2xl font-bold mb-4 text-center">{{ title }}</h2>
    <p class="text-xl text-slate-400 text-center mb-14">
      {{ description }}
    </p>
    <router-link to="/">
      <button
        @click="closeTheDrawer"
        class="relative bg-lime-300 text-white w-full px-14 rounded-2xl text-lg py-4 hover:bg-lime-500 disabled:bg-slate-200"
      >
        Повернутися на головну
        <img src="/arrow-next.svg" alt="Arrow-right" class="absolute left-6 top-6 rotate-180" />
      </button>
    </router-link>
  </div>
</template>
