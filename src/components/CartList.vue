<script setup>
import { inject } from 'vue'
import CartItem from './CartItem.vue'


const {cartv, onDeleteItemFromCart} = inject('cart')


</script>

<template>
  <div class="flex flex-col gap-3 flex-1" v-auto-animate>
    <CartItem
      v-for="item in cartv"
      :key="item.id"
      :imageUrl="item.imageUrl"
      :title="item.title"
      :price="item.cost"
      @on-click-remove="()=>onDeleteItemFromCart(item)"
    />
  </div>
</template>
