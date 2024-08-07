<template>
  <div class="card__list">
    <Card
      v-for="item in items"
      :key="item.id"
      :image-url="item.imageUrl"
      :title="item.title"
      :price="item.price"
    />
  </div>
</template>

<script setup>
import Card from './Card.vue'

defineProps({
  items: Array
})
</script>
<style lang="scss" scoped>
.card__list {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  padding: 0 20px;
}
</style>
