<script setup>
import { computed } from 'vue'

const props = defineProps({
  id: Number,
  title: String,
  price: Number,
  imageUrl: String,
  isFavorite: Boolean,
  isAdded: Boolean
})

const emit = defineEmits(['onClickFavorite'])

const cardIsFavorite = computed(() => (props.isFavorite ? '/like-2.svg' : '/like-1.svg'))
const cardIsAdded = computed(() => (props.isAdded ? '/checked.svg' : '/plus.svg'))
</script>

<template>
  <li
    class="relative bg-white rounded-3xl border border-slate-200 p-6 hover:shadow-xl hover:-translate-y-1 transition-all"
  >
    <img
      @click="emit('onClickFavorite')"
      :src="cardIsFavorite"
      alt="Like"
      class="absolute top-6 left-4 cursor-pointer"
    />
    <img :src="imageUrl" :alt="title" class="w-72 mb-2" />
    <p class="text-2xl mb-4">{{ title }}</p>
    <div class="flex items-end justify-between">
      <div class="flex flex-col">
        <span class="text-slate-400 uppercase">ціна:</span>
        <b class="text-2xl">{{ price }} грн.</b>
      </div>
      <img :src="cardIsAdded" alt="Plus" class="cursor-pointer" />
    </div>
  </li>
</template>
