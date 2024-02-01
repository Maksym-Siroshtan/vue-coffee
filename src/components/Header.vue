<template>
  <header class="flex justify-between items-center border-b border-slate-200 px-6 py-4">
    <div class="flex items-center gap-3">
      <img src="/logo.png" alt="Logo" class="w-28" />
      <div>
        <h2 class="text-3xl uppercase font-bold">coffee shop</h2>
        <p class="text-xl text-slate-400">Магазин найсмачнішої кави</p>
      </div>
    </div>

    <ul class="flex gap-8">
      <li class="flex items-center gap-2 cursor-pointer">
        <img src="/cart.svg" alt="Cart" />
        <b class="text-lg text-slate-500 hover:text-black">1205 грн.</b>
      </li>
      <li class="flex items-center gap-2 cursor-pointer">
        <img src="/heart.svg" alt="Heart" />
        <span class="text-lg text-slate-400 hover:text-black">Закладки</span>
      </li>
      <li class="flex items-center gap-2 cursor-pointer">
        <img src="/profile.svg" alt="Profile" />
        <span class="text-lg text-slate-400 hover:text-black">Профіль</span>
      </li>
    </ul>
  </header>
</template>
