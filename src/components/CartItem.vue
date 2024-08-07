<template>
  <div class="cart-item">
    <img src="/sneakers/sneakers-1.jpg" alt="" />

    <div class="cart-item__info">
      <div class="cart-item__title">Мужские Кроссовки Nike Air Max 270</div>
      <div class="cart-item__price">1205 руб</div>
    </div>

    <button class="cart-item__btn">
      <img src="/close.svg" alt="" />
    </button>
  </div>
</template>
<script setup></script>
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
    &:hover {
      opacity: 1;
    }
  }
}
</style>
