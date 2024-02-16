<script setup>
defineProps({
  title: String,
  description: String,
  imageUrl: String
})
</script>

<template>
  <div class="flex flex-col items-center text-center mx-auto w-72">
    <img class="h-16 w-16" :src="imageUrl" alt="logo image" />
    <h2 class="mt-2 text-2xl font-medium">{{ title }}</h2>
    <p class="text-gray-400">{{ description }}</p>
  </div>
</template>
