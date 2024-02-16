<script setup>
defineProps({
  totalPrice: Number
})

const emit = defineEmits(['openTheDrawer'])
</script>

<template>
  <header class="flex justify-between items-center border-b border-slate-200 pl-8 pr-12 py-4">
    <router-link to="/">
      <div class="flex items-center gap-3">
        <img src="/logo.png" alt="Logo" class="w-24" />
        <div>
          <h2 class="text-3xl uppercase font-bold">coffee shop</h2>
          <p class="text-xl text-slate-400">Магазин найсмачнішої кави</p>
        </div>
      </div>
    </router-link>

    <ul class="flex gap-8">
      <li @click="emit('openTheDrawer')" class="flex items-center gap-2 cursor-pointer">
        <img src="/cart.svg" alt="Cart" />
        <b class="text-lg text-slate-500 hover:text-black">{{ totalPrice }} грн.</b>
      </li>
      <router-link to="/favorites">
        <li class="flex items-center gap-2 cursor-pointer">
          <img src="/heart.svg" alt="Heart" />
          <span class="text-lg text-slate-400 hover:text-black">Обране</span>
        </li>
      </router-link>
      <li class="flex items-center gap-2 cursor-pointer">
        <img src="/profile.svg" alt="Profile" />
        <span class="text-lg text-slate-400 hover:text-black">Профіль</span>
      </li>
    </ul>
  </header>
</template>
