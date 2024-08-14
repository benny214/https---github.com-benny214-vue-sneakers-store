<template>
  <div class="cart">
    <div class="cart__inner">
      <div class="cart__title-wrap">
        <svg
          @click="closeCart"
          width="16"
          height="14"
          viewBox="0 0 16 14"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            d="M1 7H14.7143"
            stroke="#000"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
          />
          <path
            d="M8.71436 1L14.7144 7L8.71436 13"
            stroke="#000"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
          />
        </svg>
        <h2 class="cart__title">Корзина</h2>
      </div>

      <CartList />

      <div class="cart__details">
        <div class="cart__total">
          <span class="cart__total-title">Итого:</span>
          <span class="cart__total-price">12995 руб</span>
        </div>

        <div class="cart__tax">
          <span class="cart__tax-title">Налог 5%: </span>
          <span class="cart__tax-price">1205 руб</span>
        </div>

        <button class="cart__btn">Оформить заказ</button>
      </div>
    </div>
    <div class="cart__backdrop" @click="closeCart"></div>
  </div>
</template>
<script setup>
import { inject } from 'vue'
import CartList from './CartList.vue'

const { closeCart } = inject('cartActions')
</script>
<style lang="scss" scoped>
.cart {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 10;
  &__backdrop {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
  }
  &__inner {
    background-color: #fff;
    width: 40%;
    height: 100%;
    position: fixed;
    top: 0;
    right: 0;
    z-index: 20;
    padding: 25px;
    display: flex;
    flex-direction: column;
  }
  &__title {
    font-size: 24px;
    font-weight: 600;
    &-wrap {
      display: flex;
      align-items: center;
      gap: 20px;
      margin-bottom: 30px;
      svg {
        cursor: pointer;
        opacity: 0.5;
        transform: rotate(-180deg);
        transition: all 0.3s ease;
        &:hover {
          opacity: 1;
        }
      }
    }
  }
  &__details {
    margin-top: auto;
  }
  &__total,
  &__tax {
    display: flex;
    align-items: center;
    margin-bottom: 20px;
    &-price {
      margin-left: auto;
      font-weight: 600;
    }
  }
  &__btn {
    width: 100%;
    background-color: #9dd458;
    padding: 18px 0;
    border-radius: 18px;
    color: #fff;
    font-weight: 600;
    transition: all 0.3s ease;
    &:hover {
      background-color: #698743;
    }
  }
}
</style>
