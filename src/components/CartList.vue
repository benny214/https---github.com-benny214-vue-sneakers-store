<template>
  <div class="cart-list">
    <CartItem
      v-for="item in cart"
      :key="item.id"
      :title="item.title"
      :image-url="item.imageUrl"
      :price="item.price"
      @onClickRemove="() => removeFromCart(item)"
    />
  </div>
</template>
<script setup>
import { inject } from 'vue'
import CartItem from './CartItem.vue'

const { cart, removeFromCart } = inject('cartActions')
</script>
<style lang="scss" scoped>
.cart-list {
  display: flex;
  flex-direction: column;
  gap: 20px;
  overflow: auto;
}
</style>
