<script setup>
import axios from 'axios'
import debounce from 'lodash.debounce'
import { inject, onMounted, provide, reactive, watch } from 'vue'
import CardList from '../components/CardList.vue'

const { itemsv, cartv, onDeleteItemFromCart, onAddItemToCart } = inject('cart')



const filters = reactive({
  _sort: 'title',
  q: ''
})
const fetchItems = async () => {
  try {
    const params = {
      _sort: filters._sort
    }

    if (filters.q) {
      params.q = filters.q
    }
    const { data } = await axios.get('http://localhost:3001/itemsv', { params })
    itemsv.value = data.map((obj) => ({
      ...obj,
      isAdded: false,
      isFavorite: false,
      favoriteId: null
    }))
  } catch (err) {
    alert('Не удалось загрузить данные с сервера')
    console.log('Ошибка запроса: ' + err)
  }
}
const fetchFavorites = async () => {
  try {
    const { data: favoritesv } = await axios.get('http://localhost:3001/favoritesv')
    itemsv.value = itemsv.value.map((item) => {
      const favorite = favoritesv.find((favorite) => favorite.parentId === item.id)
      if (!favorite) {
        return item
      }

      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id
      }
    })
  } catch (err) {
    alert('Не удалось загрузить данные с сервера')
    console.log('Ошибка запроса: ' + err)
  }
}
const onChangeSelect = (event) => {
  filters._sort = event.target.value
  console.log(event.target.value)
}


const onSearchInputChange = debounce((event) => {
  filters.q = event.target.value
  console.log(event.target.value)
}, 500)
const onClickPlusAddItem = (item) => {
  if (!item.isAdded) {
    onAddItemToCart(item)
  } else {
    onDeleteItemFromCart(item)
  }
}
const addToFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      const obj = {
        parentId: item.id,
        item
      }
      item.isFavorite = true
      const { data } = await axios.post('http://localhost:3001/favoritesv', obj)

      item.favoriteId = data.id
    } else {
      item.isFavorite = false
      await axios.delete(`http://localhost:3001/favoritesv/${item.favoriteId}`)
      item.favoriteId = null
    }
  } catch (error) {
    console.log(error)
  }
}

onMounted(async () => {
  const localCart = localStorage.getItem('cart')
  cartv.value = localCart ? JSON.parse(localCart) : []

  await fetchItems()
  await fetchFavorites()

  itemsv.value = itemsv.value.map((item) => ({
    ...item,
    isAdded: cartv.value.some((cartItem) => cartItem.id === item.id)
  }))
})
watch(filters, fetchItems)

provide('addToFavorite', addToFavorite)
</script>

<template>
  <div class="flex justify-between items-center">
    <h2 class="text-3xl font-bold mb-8">Все кроссовки</h2>

    <div class="flex gap-3">
      <select @change="onChangeSelect" class="py-2 px-3 border rounded-md outline-none">
        <option value="title">По названию</option>
        <option value="cost">По цене(дешевле)</option>
        <option value="-cost">По цене(дороже)</option>
      </select>
      <div class="relative">
        <img class="absolute left-4 top-3" src="/search.svg" alt="search" />
        <input
          @input="onSearchInputChange"
          type="text"
          placeholder="Поиск"
          class="border rounded-md py-2 pl-11 pr-4 outline-none focus:border-grey-400"
        />
      </div>
    </div>
  </div>

  <div class="mt-10">
    <CardList :itemsv="itemsv" @add-to-favorite="addToFavorite" @add-to-cart="onClickPlusAddItem" />
  </div>
</template>
