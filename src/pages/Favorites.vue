<template>
  <CardList :items="favorites" is-favorites />
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
.card__list {
  padding-top: 40px;
  padding-bottom: 40px;
}
</style>
