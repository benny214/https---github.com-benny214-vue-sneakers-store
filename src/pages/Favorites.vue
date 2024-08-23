<template>
  <div class="favorites">
    <h2 class="favorites__title">Избранное</h2>
    <CardList :items="favorites" is-favorites />
  </div>
</template>
<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

import CardList from '../components/CardList.vue'

const favorites = ref([])

onMounted(async () => {
  try {
    const { data } = await axios.get(
      'https://c74949f531e90773.mokky.dev/favourites?_relations=items'
    )

    favorites.value = data.map((obj) => obj.item)
  } catch (err) {
    console.log(err)
  }
})
</script>
<style lang="scss" scoped>
.favorites {
  &__title {
    font-size: 24px;
    font-weight: 600;
    padding: 20px;
  }

  .card__list {
    padding-bottom: 40px;
    :deep(.card) {
      &:hover {
        transform: none;
        box-shadow: none;
        border: 1px solid #e7e7e7;
      }
    }
  }
}
</style>
