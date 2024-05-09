<script setup>
const emit = defineEmits(['onClickRemove'])

defineProps({
  id: Number,
  imageUrl: String,
  title: String,
  price: Number
})
</script>
<template>
  <div class="relative flex items-center border border-slate-200 p-4 rounded-xl gap-4">
    <img class="w-32" :src="imageUrl" alt="Sneaker" />
    <div class="flex flex-col">
      <p class="text-xl">{{ title }}</p>

      <div class="flex justify-between mt-2">
        <b class="text-xl">{{ price }} грн.</b>
        <img
          @click="emit('onClickRemove')"
          class="absolute right-4 bottom-4 opacity-40 hover:opacity-100 cursor-pointer transition"
          src="/close.svg"
          alt=""
        />
      </div>
    </div>
  </div>
</template>
