<script setup>
import { inject } from 'vue'
defineProps({
  totalPrice: Number,
  salePrice: Number
})

const { openCart } = inject('cart')
</script>
<template>
  <header class="flex justify-between border-b border-slate-100 px-10 py-8">
    <router-link to="/">
      <div class="flex items-center gap-4 cursor-pointer">
        <img src="/logo.png" alt="logo" class="w-10" />
        <div>
          <h2 class="text-xl font-bold uppercase">Vue проект</h2>
          <p class="text-slate-400">Магазин игрушек</p>
        </div>
      </div>
    </router-link>

    <ul class="flex items-center gap-10">
      <li
        @click="openCart"
        class="flex items-center gap-3 text-slate-500 hover:text-black cursor-pointer"
      >
        <img src="/cart.svg" alt="cart" />
        <b>Общая стоимость:</b>
        <span>{{ totalPrice }} руб</span>
        <b>Скидка:</b>
        <span>{{ salePrice }} руб</span>
      </li>
      <router-link to="/favorites"
        ><li class="flex items-center gap-3 text-slate-500 hover:text-black cursor-pointer">
          <img src="/heart.svg" alt="favorites" />
          <span>Избранное</span>
        </li></router-link
      >

      <li class="flex items-center gap-3 text-slate-500 hover:text-black cursor-pointer">
        <img src="/profile.svg" alt="profile" />
        <span>Профиль</span>
      </li>
    </ul>
  </header>
</template>
