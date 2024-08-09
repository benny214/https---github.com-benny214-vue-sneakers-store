<template>
  <div class="card__list">
    <Card
      v-for="item in items"
      :key="item.id"
      :id="item.id"
      :image-url="item.imageUrl"
      :title="item.title"
      :price="item.price"
      :is-favourite="item.isFavourite"
      :on-click-favourite="() => addToFavourites(item)"
    />
  </div>
</template>

<script setup>
import { inject } from 'vue'
import Card from './Card.vue'

defineProps({
  items: Array
})

const addToFavourites = inject('addToFavourites')

const onClickAdd = () => {
  console.log('added')
}
</script>
<style lang="scss" scoped>
.card__list {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  padding: 0 20px;
}
</style>
