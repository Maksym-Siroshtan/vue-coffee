<script setup>
defineProps({
  title: String,
  description: String,
  imageUrl: String
})
</script>

<template>
  <div class="flex flex-col items-center w-5/6 m-auto">
    <img :src="imageUrl" alt="Package-icon" class="w-44 mb-8" />
    <h2 class="text-2xl font-bold mb-4 text-center">{{ title }}</h2>
    <p class="text-xl text-slate-400 text-center">
      {{ description }}
    </p>
  </div>
</template>
