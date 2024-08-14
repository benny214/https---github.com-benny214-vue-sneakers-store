<template>
  <div class="cart-item">
    <img :src="imageUrl" :alt="title" />

    <div class="cart-item__info">
      <div class="cart-item__title">{{ title }}</div>
      <div class="cart-item__price">{{ price }} руб</div>
    </div>

    <button class="cart-item__btn">
      <img @click="emit('onClickRemove')" src="/close.svg" alt="" />
    </button>
  </div>
</template>
<script setup>
const emit = defineEmits(['onClickRemove'])

defineProps({
  id: Number,
  title: String,
  imageUrl: String,
  price: Number
})
</script>
<style lang="scss" scoped>
.cart-item {
  padding: 20px;
  border: 1px solid #e7e7e7;
  border-radius: 10px;
  display: flex;
  gap: 21px;
  align-items: center;
  img {
    width: 64px;
    height: 64px;
  }
  &__price {
    font-weight: 600;
  }
  &__title {
    margin-bottom: 15px;
  }
  &__btn {
    margin-left: auto;
    opacity: 0.4;
    transition: all 0.3s ease;
    img {
      width: 32px;
      height: 32px;
    }
    &:hover {
      opacity: 1;
    }
  }
}
</style>
