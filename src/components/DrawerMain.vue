<script setup>
import DrawerHead from './DrawerHead.vue'
import CartList from './CartList.vue'
import { inject } from 'vue'
import InfoBlock from './InfoBlock.vue'

defineProps({
  disableButton: Boolean
})

const { totalPrice, salePrice, finalCost } = inject('price')

const emit = defineEmits(['createOrder'])
</script>

<template>
  <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-40"></div>
  <div class="fixed top-0 right-0 bg-white w-1/4 h-full z-20 p-8">
    <DrawerHead />

    <div v-if="!totalPrice" class="flex items-center h-full">
      <InfoBlock
        title="Корзина пуста"
        description="Добавьте что нибудь в корзину"
        image-url="package-icon.png"
      />
    </div>

    <div v-else>
      <CartList />

      <div class="flex flex-col gap-4 mt-7">
        <div class="flex gap-2">
          <span>Общая стоимость:</span>
          <div class="flex-1 border-b border-dashed"></div>
          <b>{{ totalPrice }} руб</b>
        </div>
        <div class="flex gap-2">
          <span>Скидка 5%:</span>
          <div class="flex-1 border-b border-dashed"></div>
          <b>{{ salePrice }} руб</b>
        </div>
        <div class="flex gap-2">
          <span>Итого со скидкой:</span>
          <div class="flex-1 border-b border-dashed"></div>
          <b>{{ finalCost }} руб</b>
        </div>
        <button
          :disabled="disableButton"
          @click="() => emit('createOrder')"
          class="transition bg-lime-500 w-full rounded-xl py-3 mt-4 text-white hover:bg-lime-600 active:bg-lime-400 disabled:bg-slate-300 disabled:cursor-not-allowed cursor-pointer"
        >
          Оформить заказ
        </button>
      </div>
    </div>
  </div>
</template>
