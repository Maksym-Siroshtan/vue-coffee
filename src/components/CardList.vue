<script setup>
import Card from './Card.vue'

defineProps({
  items: Array
})

const emit = defineEmits(['onClickFavorite'])
</script>

<template>
  <ul class="grid grid-cols-4 gap-8">
    <Card
      v-for="item in items"
      :key="item.id"
      :id="item.id"
      :title="item.title"
      :price="item.price"
      :image-url="item.imageUrl"
      :is-favorite="item.isFavorite"
      :is-added="item.isAdded"
      @on-click-favorite="emit('onClickFavorite', item)"
    />
  </ul>
</template>
