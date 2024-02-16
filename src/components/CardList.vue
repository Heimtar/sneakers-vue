<script setup>

import CardItem from './CardItem.vue'

defineProps({
  itemsv: Array,
  isFavorites: Boolean
})
const emit = defineEmits(['addToFavorite','addToCart'])

</script>

<template>
  <div class="grid grid-cols-4 gap-5" v-auto-animate>
    <CardItem
      v-for="item in itemsv"
      :key="item.id"
      :id="item.id"
      :imageUrl="item.imageUrl"
      :title="item.title"
      :price="item.cost"
      :isAdded="item.isAdded"
      :isFavorite="item.isFavorite"
      :onClickAddToFavorite="isFavorites ? null : () => emit('addToFavorite', item)"
      :onClickAddToCart="isFavorites ? null : ()=>emit('addToCart', item)"
    />
  </div>
</template>
