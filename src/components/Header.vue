<template>
  <header class="header">
    <div class="header__logo">
      <img src="/logo.png" alt="" class="" />
      <div class="header__logo-text">
        <h2 class="header__logo-title">Vue Sneaker</h2>
        <p class="header__logo-subtitle">магазин лучших кроссовок</p>
      </div>
    </div>

    <ul class="header__menu">
      <li @click="() => emit('openCart')" class="header__menu-item">
        <img src="/cart.svg" alt="" class="header__menu-icon" />
        <span class="header__menu-text">{{ totalPrice }} руб</span>
      </li>
      <li class="header__menu-item">
        <img src="/heart.svg" alt="" class="header__menu-icon" />
        <span class="header__menu-text">Закладки</span>
      </li>
      <li class="header__menu-item">
        <img src="/profile.svg" alt="" class="header__menu-icon" />
        <b class="header__menu-text">Профиль</b>
      </li>
    </ul>
  </header>
</template>
<script setup>
defineProps({
  totalPrice: Number
})
const emit = defineEmits(['openCart'])
</script>
<style lang="scss" scoped>
.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 20px;
  border-bottom: 1px solid #000;
  &__logo {
    display: flex;
    align-items: center;
    gap: 10px;
    img {
      width: 40px;
      height: 40px;
    }
    &-title {
      font-size: 24px;
      font-weight: 600;
    }
    &-text {
      font-size: 14px;
    }
  }
  &__menu {
    display: flex;
    align-items: center;
    gap: 15px;
    &-item {
      display: flex;
      align-items: center;
      gap: 8px;
      cursor: pointer;
      transition: all 0.3s ease;
      &:hover {
        opacity: 0.7;
      }
    }
  }
}
</style>
