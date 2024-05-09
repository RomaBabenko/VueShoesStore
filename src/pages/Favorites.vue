<script setup>
import { onMounted, ref, inject } from 'vue'
import axios from 'axios'
import CardList from '../components/CardList.vue'
import InfoBlock from '../components/InfoBlock.vue'

const favorites = ref([])
const { cart, addToCart, removeFromCart, onClickAddPlus } = inject('cart')

onMounted(async () => {
  const localCart = localStorage.getItem('cart')
  cart.value = localCart ? JSON.parse(localCart) : []

  try {
    const { data } = await axios.get(
      'https://b1c01e0897d48f83.mokky.dev/favorites?_relations=items'
    )
    favorites.value = data.map((obj) => {
      const item = obj.item
      item.isFavorite = true
      return item
    })
  } catch (err) {
    console.log(err)
  }
})

const removeToFavorite = async (item) => {
  try {
    const index = favorites.value.findIndex((favItem) => favItem.id === item.id)
    if (index !== -1) {
      favorites.value.splice(index, 1)
    }
    const { data } = await axios.get(`https://b1c01e0897d48f83.mokky.dev/favorites`)
    const test = data.splice(index, 1)
    test.map((item) => {
      axios.delete(`https://b1c01e0897d48f83.mokky.dev/favorites/${item.id}`)
    })
  } catch (err) {
    console.error('Error removing from favorites:', err)
  }
}
</script>

<template>
  <h2 class="text-3xl font-bold mb-8">Мои закладки</h2>
  <div v-if="!favorites.length">
    <InfoBlock
        title="Закладок нет"
        description="Добавьте хотя бы одну пару кроссовок, чтобы увидеть закладки."
        imageUrl="/emoji-1.png"
      />
  </div>
  <CardList :items="favorites" @add-to-favorite="removeToFavorite" @add-to-cart="onClickAddPlus" />
</template>
