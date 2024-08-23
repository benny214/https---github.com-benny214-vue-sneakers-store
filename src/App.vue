<template>
  <Cart
    v-if="cartOpen"
    :total-price="totalPrice"
    :vat-price="vatPrice"
    @create-order="createOrder"
    :disabledButton="disabledCartButton"
  />

  <div class="main">
    <Header :total-price="totalPrice" @open-cart="openCart" />

    <router-view></router-view>
  </div>
</template>

<script setup>
import { ref, watch, provide, computed } from 'vue'
import axios from 'axios'

import Header from './components/Header.vue'
import Cart from './components/Cart.vue'

/* Корзина (START)*/
const cart = ref([])
const isCreatingOrder = ref(false)

const cartOpen = ref(false)

const totalPrice = computed(() => cart.value.reduce((acc, item) => acc + item.price, 0))

const vatPrice = computed(() => Math.round((totalPrice.value * 5) / 100))

const carttIsEmpty = computed(() => cart.value.length === 0)

const disabledCartButton = computed(() => isCreatingOrder.value || carttIsEmpty.value)

const closeCart = () => {
  cartOpen.value = false
}

const openCart = () => {
  cartOpen.value = true
}

const addToCart = (item) => {
  cart.value.push(item)
  item.isAdded = true
}

const removeFromCart = (item) => {
  cart.value.splice(cart.value.indexOf(item), 1)
  item.isAdded = false
}

const createOrder = async () => {
  try {
    isCreatingOrder.value = true
    const { data } = await axios.post('https://c74949f531e90773.mokky.dev/orders', {
      items: cart.value,
      totalPrice: totalPrice.value
    })

    cart.value = []

    return data
  } catch (err) {
    console.log(err)
  } finally {
    isCreatingOrder.value = false
  }
}

watch(
  cart,
  () => {
    localStorage.setItem('cart', JSON.stringify(cart.value))
  },
  { deep: true }
)

provide('cartActions', {
  closeCart,
  openCart,
  cart,
  addToCart,
  removeFromCart
})

/* Корзина (END)*/
</script>

<style lang="scss" scoped>
.main {
  background-color: #fff;
  min-height: 100vh;
  width: 80%;
  margin: 20px auto;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  border-radius: 15px;
}
</style>
