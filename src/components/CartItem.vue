<script setup>
defineProps({
  imageUrl: String,
  title: String,
  price: Number,
  onClickRemove: Function
})

const emit = defineEmits(['onClickRemove'])
</script>

<template>
  <div class="flex items-center justify-between border border-slate-200 p-4 rounded-xl gap-4">
    <img class="w-16 h-16" :src="imageUrl" alt="product" />

    <div class="flex flex-1 flex-col">
      <p>{{ title }}</p>

      <div class="flex justify-between mt-2">
        <b>{{ price }} руб</b>
        <img
          @click="emit('onClickRemove')"
          class="opacity-40 hover:opacity-100 cursor-pointer transition"
          src="/close.svg"
          alt="delete"
        />
      </div>
    </div>
  </div>
</template>
