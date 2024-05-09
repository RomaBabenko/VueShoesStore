<script setup>
import { ref, watch, provide, computed } from 'vue'

import Header from './components/Header.vue'
import Drawer from './components/Drawer.vue'

const cart = ref([])
const drawerOpen = ref(false)

const totalPrice = computed(() => cart.value.reduce((acc, item) => acc + item.price, 0))

const closeDrawer = () => {
  drawerOpen.value = false
  document.body.style.overflow = 'auto'
}

const openDrawer = () => {
  drawerOpen.value = true
  document.body.style.overflow = 'hidden'
}

const addToCart = (item) => {
  cart.value.push(item)
  item.isAdded = true
  setTimeout(() => {
    item.isAdded = false
  }, 500)
}

const onClickAddPlus = (item) => {
  if (!item.isAdded) {
    addToCart(item)
  } else {
    removeFromCart(item)
  }
}

const removeFromCart = (item) => {
  cart.value.splice(cart.value.indexOf(item), 1)
  item.isAdded = false
}

const clearCart = () => {
  cart.value = []
}

watch(
  cart,
  () => {
    localStorage.setItem('cart', JSON.stringify(cart.value))
  },
  {
    deep: true
  }
)

provide('cart', {
  cart,
  closeDrawer,
  clearCart,
  openDrawer,
  addToCart,
  removeFromCart,
  onClickAddPlus
})
</script>

<template>
  <Drawer v-if="drawerOpen" :total-price="totalPrice"/>

  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14 overflow-hidden">
    <Header :total-price="totalPrice" @open-drawer="openDrawer" />

    <div class="p-10">
      <router-view></router-view>
    </div>
  </div>
</template>

<style scoped></style>
