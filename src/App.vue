<script setup>
import { computed, provide, ref, watch } from 'vue'
import axios from 'axios'
import HeaderMain from './components/HeaderMain.vue'

import DrawerMain from './components/DrawerMain.vue'

const cartv = ref([])
const itemsv = ref([])

const isCreatingOrder = ref(false)
const cartOpenToogle = ref(false)


const totalPrice = computed(() => cartv.value.reduce((acc, item) => acc + item.cost, 0))
const salePrice = computed(() => Math.round(totalPrice.value * 0.05))
const finalCost = computed(() => Math.round(totalPrice.value - salePrice.value))

const emptyCart = computed(() => cartv.value.length === 0)
const disableCartButton = computed(() => isCreatingOrder.value || emptyCart.value)

const createOrder = async () => {
  try {
    isCreatingOrder.value = true
    const { data } = axios.post('https://test-api-xih4.onrender.com/ordersv', {
      items: cartv.value,
      totalPrice: totalPrice.value
    })
    cartv.value = []
    return data
  } catch (error) {
    alert('Не удалось сформировать заказ')
    console.log('Ошибка запроса: ' + error)
  } finally {
    isCreatingOrder.value = false
  }
}
const closeCart = () => {
  cartOpenToogle.value = false
}
const openCart = () => {
  cartOpenToogle.value = true
}
const onAddItemToCart = (item) => {
  cartv.value.push(item)
  item.isAdded = true
}
const onDeleteItemFromCart = (item) => {
  cartv.value.splice(cartv.value.indexOf(item), 1)
  item.isAdded = false
}

watch(cartv, () => {
  itemsv.value = itemsv.value.map((item) => ({
    ...item,
    isAdded: false
  }))
})
watch(
  cartv,
  () => {
    localStorage.setItem('cart', JSON.stringify(cartv.value))
  },
  { deep: true }
)

provide('cart', {
  itemsv,
  cartv,
  closeCart,
  openCart,
  onAddItemToCart,
  onDeleteItemFromCart
})
provide('price', {
  totalPrice,
  salePrice,
  finalCost
})
</script>

<template>
  <DrawerMain
    v-if="cartOpenToogle"
    @create-order="createOrder"
    :disable-button="disableCartButton"
  />
  <div class="bg-gray-100 w-4/5 m-auto rounded-2xl shadow-xl mt-10">
    <HeaderMain :total-price="totalPrice" :sale-price="salePrice" />

    <div class="p-10">
      <router-view></router-view>
    </div>
  </div>
</template>
