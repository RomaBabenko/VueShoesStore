<script setup>
defineProps({
  title: String,
  description: String,
  imageUrl: String
})
</script>

<template>
  <div class="w-96 flex flex-col items-center text-center mx-auto">
    <img class="w-40" :src="imageUrl" alt="Info image" />
    <h2 class="mt-4 text-2xl font-medium">{{ title }}</h2>
    <p class="text-gray-500">{{ description }}</p>
  </div>
</template>
