<template>
  <div class="cart-list">
    <CartItem />
  </div>
</template>
<script setup>
import CartItem from './CartItem.vue'
</script>
<style lang="scss" scoped>
.cart-list {
  display: flex;
  flex-direction: column;
  gap: 20px;
}
</style>
