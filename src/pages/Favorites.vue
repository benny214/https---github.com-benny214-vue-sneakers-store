<template>
  <div class="card__list">Favorites</div>
</template>
<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

const favorites = ref([])

onMounted(async () => {
  try {
    const { data } = await axios.get('https://c74949f531e90773.mokky.dev/favourites')

    console.log(data)
  } catch (err) {
    console.log(err)
  }
})
</script>
<style lang="scss" scoped>
.card__list {
  padding-top: 20px;
}
</style>
