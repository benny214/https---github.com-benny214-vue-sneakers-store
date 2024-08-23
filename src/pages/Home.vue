<template>
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
</template>
<script setup>
import { inject, reactive, watch, ref, onMounted } from 'vue'
import axios from 'axios'
import CardList from '../components/CardList.vue'

const items = ref([])

const { cart, addToCart, removeFromCart } = inject('cartActions')

const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})

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

const addToFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      const obj = {
        item_id: item.id
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

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get(`https://c74949f531e90773.mokky.dev/favourites`)

    items.value = items.value.map((item) => {
      const favorite = favorites.find((favorite) => favorite.item_id === item.id)

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
  const localCart = localStorage.getItem('cart')
  cart.value = localCart ? JSON.parse(localCart) : []

  await fetchItems()
  await fetchFavorites()

  items.value = items.value.map((item) => ({
    ...item,
    isAdded: cart.value.some((cartItem) => cartItem.id === item.id)
  }))
})

watch(cart, () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: false
  }))
})

watch(filters, fetchItems)
</script>
<style lang="scss" scoped>
.main {
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
