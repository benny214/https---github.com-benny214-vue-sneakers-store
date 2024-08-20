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

    <div class="main__title-wrap">
      <h2 class="main__title">Все кроссовки</h2>

      <select @change="onChangeSelect" name="" id="" class="main__select">
        <option value="name">По названию</option>
        <option value="price">По цене (дешевле)</option>
        <option value="-price">По цене (дороже)</option>
      </select>

      <div class="main__filter-wrap">
        <img src="/search.svg" alt="" />
        <input @input="onChangeSearchInput" class="main__filter" type="text" placeholder="Поиск" />
      </div>
    </div>

    <CardList :items="items" @add-to-favorite="addToFavorite" @add-to-cart="onClickAddToCart" />
  </div>
</template>

<script setup>
import { onMounted, ref, watch, reactive, provide, computed } from 'vue'
import axios from 'axios'
import Header from './components/Header.vue'
import CardList from './components/CardList.vue'
import Cart from './components/Cart.vue'

const items = ref([])
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

const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})

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

const onClickAddToCart = (item) => {
  if (!item.isAdded) {
    addToCart(item)
  } else {
    removeFromCart(item)
  }
}

const onChangeSelect = (event) => {
  filters.sortBy = event.target.value
}

const onChangeSearchInput = (event) => {
  filters.searchQuery = event.target.value
}

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get(`https://c74949f531e90773.mokky.dev/favourites`)

    items.value = items.value.map((item) => {
      const favorite = favorites.find((favorite) => favorite.parentId === item.id)

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
    console.log(err)
  }
}

const addToFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      const obj = {
        parentId: item.id
      }

      item.isFavorite = true

      const { data } = await axios.post(`https://c74949f531e90773.mokky.dev/favourites`, obj)

      item.favoriteId = data.id
    } else {
      item.isFavorite = false

      await axios.delete(`https://c74949f531e90773.mokky.dev/favourites/${item.favoriteId}`)

      item.favoriteId = null
    }
  } catch (err) {
    console.log(err)
  }
}

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy
    }

    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`
    }
    const { data } = await axios.get(`https://c74949f531e90773.mokky.dev/items`, {
      params
    })

    items.value = data.map((obj) => ({
      ...obj,
      isFavorite: false,
      favoriteId: null,
      isAdded: false
    }))
  } catch (err) {
    console.log(err)
  }
}

onMounted(async () => {
  await fetchItems()
  await fetchFavorites()
})

watch(filters, fetchItems)

watch(cart, () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: false
  }))
})

provide('cartActions', {
  closeCart,
  openCart,
  cart,
  addToCart,
  removeFromCart
})
</script>

<style lang="scss" scoped>
.main {
  background-color: #fff;
  min-height: 100vh;
  width: 80%;
  margin: 20px auto;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  border-radius: 15px;
  &__title {
    font-size: 24px;
    font-weight: 600;
    padding: 20px;
    &-wrap {
      display: flex;
      align-items: center;
      margin-bottom: 40px;
    }
  }
  &__select {
    border: 1px solid #e7e7e7;
    border-radius: 10px;
    padding: 10px;
    margin-left: 20px;
  }
  &__filter {
    border: 1px solid #e7e7e7;
    border-radius: 10px;
    padding: 10px 30px;
    &-wrap {
      margin-left: auto;
      position: relative;
      img {
        position: absolute;
        top: 50%;
        left: 10px;
        transform: translateY(-50%);
      }
    }
  }
}
</style>
