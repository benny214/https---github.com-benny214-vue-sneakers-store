<script setup>
import { onMounted, ref, watch, reactive, provide } from 'vue'
import axios from 'axios'
import Header from './components/Header.vue'
import CardList from './components/CardList.vue'
//import Cart from './components/Cart.vue'

const items = ref([])

const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})

const onChangeSelect = (event) => {
  filters.sortBy = event.target.value
}

const onChangeSearchInput = (event) => {
  filters.searchQuery = event.target.value
}

const fetchFavorites = async () => {
  try {
    const { data: favourites } = await axios.get(`https://c74949f531e90773.mokky.dev/favourites`)

    items.value = items.value.map((item) => {
      const favourite = favourites.find((favourite) => favourite.productId === item.id)

      if (!favourite) {
        return item
      }

      return {
        ...item,
        isFavourite: true,
        favouriteId: favourite.id
      }
    })
  } catch (err) {
    console.log(err)
  }
}

const addToFavourites = async (item) => {
  item.is = true

  console.log(item)
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
      isFavourite: false,
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

provide('addToFavourites', addToFavourites)
</script>

<template>
  <!--<Cart />-->
  <div class="main">
    <Header />

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

    <CardList :items="items" />
  </div>
</template>

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
