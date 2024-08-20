<template>
  <div class="info-block">
    <img :src="imageUrl" class="info-block__img" />
    <h2 class="info-block__title">{{ title }}</h2>
    <p class="info-block__text">{{ description }}</p>
  </div>
</template>
<script setup>
defineProps({
  imageUrl: String,
  title: String,
  description: String
})
</script>
<style lang="scss" scoped>
.info-block {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  gap: 15px;
  max-width: 250px;
  margin: auto;
  &__img {
    width: 120px;
    height: 120px;
  }
  &__title {
    font-size: 24px;
    font-weight: 600;
  }
  &__text {
    font-size: 14px;
  }
}
</style>
