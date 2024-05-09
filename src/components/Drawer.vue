<script setup>
import { inject, ref, computed } from 'vue'
import axios from 'axios'

import DrawerHead from './DrawerHead.vue'
import CartItemList from './CartItemList.vue'
import InfoBlock from './InfoBlock.vue'

const { cart, closeDrawer } = inject('cart')
const isCreating = ref(false)
const orderId = ref(null)

const props = defineProps({
  totalPrice: Number
})

const createOrder = async () => {
  try {
    isCreating.value = true
    const { data } = await axios.post('https://b1c01e0897d48f83.mokky.dev/orders', {
      items: cart.value,
      totalPrice: props.totalPrice.value
    })

    cart.value = []

    orderId.value = data.id
  } catch (err) {
    console.log(err)
  } finally {
    isCreating.value = false
  }
}

const cartEmpty = computed(() => cart.value.length === 0)
const buttonDisabled = computed(() => isCreating.value || cartEmpty.value)
</script>

<template>
  <div
    @click="closeDrawer"
    class="fixed top-0 left-0 w-full h-full bg-black bg-opacity-70 z-10"
  ></div>

  <div class="fixed top-0 right-0 bg-white w-2/5 h-full z-20 p-8 overflow-y-auto">
    <DrawerHead v-if="totalPrice" />

    <div v-if="!totalPrice || orderId" class="flex items-center mt-52">
      <InfoBlock
      v-if="!totalPrice && !orderId"
        title="Корзина пустая"
        description="Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ."
        imageUrl="/package-icon.png"
      />
      <InfoBlock
        v-else-if="orderId"
        title="Заказ оформлен"
        :description="`Ваш заказ #${orderId} обрабатывается. Ожидайте звонка менеджера.`"
        imageUrl="/order-success-icon.png"
      />
    </div>

    <div v-else>
      <CartItemList />

      <div v-if="totalPrice" class="mt-10 mt-10">
        <div class="flex gap-2">
          <span>Итого:</span>
          <div class="flex-1 border-b border-gray-400 border-dashed"></div>
          <b>{{ totalPrice }} ₴</b>
        </div>

        <button
          :disabled="buttonDisabled"
          @click="createOrder"
          class="mt-10 bg-lime-500 w-full rounded-xl py-3 text-white text-xl transition disabled:bg-slate-300 hover:bg-lime-400 active:bg-lime-600 cursor-pointer"
        >
          Оформить заказ
        </button>
      </div>
    </div>
  </div>
</template>
