<script setup>
defineProps({
  title: String,
  price: Number,
  imageUrl: String
})

const emit = defineEmits(['removeItemFromCart'])
</script>

<template>
  <li
    class="flex items-center overflow-hidden border border-slate-200 pr-8 py-4 rounded-3xl hover:shadow-xl hover:-translate-y-1 transition-all"
  >
    <img :src="imageUrl" :alt="title" class="w-32" />
    <div class="flex flex-col flex-1">
      <p class="text-xl mb-4">{{ title }}</p>
      <div class="flex justify-between">
        <b class="text-2xl">{{ price }} грн.</b>
        <img
          @click="emit('removeItemFromCart')"
          src="/close.svg"
          alt="Close"
          class="cursor-pointer hover:scale-125 transition-all"
        />
      </div>
    </div>
  </li>
</template>
