<script setup>
defineProps({
  items: Array
})

import Card from './Card.vue'
</script>

<template>
  <ul class="grid grid-cols-4 gap-8">
    <Card
      v-for="item in items"
      :key="item.id"
      :title="item.title"
      :price="item.price"
      :image-url="item.imageUrl"
    />
  </ul>
</template>
