<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'
import CardList from '@/components/CardList.vue'

const favoritesv = ref([])

onMounted(async () => {
  try {
    const { data } = await axios.get('https://test-api-xih4.onrender.com/favoritesv')

    favoritesv.value = data.map((obj) => obj.item)

  } catch (error) {
    console.log(error)
  }
})
</script>

<template>
    <h2 class="text-3xl font-bold mb-8">Избранное</h2>
  <CardList :itemsv="favoritesv" is-favorites/>
</template>
