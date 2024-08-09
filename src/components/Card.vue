<template>
  <div class="card">
    <img
      @click="onClickFavourite"
      :src="isFavourite ? '/like-2.svg' : '/like-1.svg'"
      alt=""
      class="card__fav"
    />
    <img :src="imageUrl" alt="" class="card__img" />
    <p class="card__title">{{ title }}</p>

    <div class="card__bottom">
      <div class="card__bottom-price">
        <span class="card__bottom-price-title">Цена</span>
        <span class="card__bottom-price-value">{{ price }} руб</span>
      </div>
      <button class="card__bottom-btn">
        <img @click="onClickAdd" :src="isAdded ? '/checked.svg' : '/plus.svg'" alt="" />
      </button>
    </div>
  </div>
</template>
<script setup>
defineProps({
  id: Number,
  imageUrl: String,
  title: String,
  price: Number,
  isFavourite: Boolean,
  isAdded: Boolean,
  onClickFavourite: Function,
  onClickAdd: Function
})
</script>
<style scoped lang="scss">
.card {
  border: 1px solid #e7e7e7;
  padding: 15px;
  border-radius: 30px;
  position: relative;
  width: 300px;
  transition: all 0.3s ease;
  &:hover {
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    border-color: transparent;
    transform: translateY(-10px);
  }
  &__fav {
    position: absolute;
    top: 15px;
    left: 15px;
    cursor: pointer;
    border-radius: 10px;
    &:hover {
      box-shadow: 0 0 5px rgba(0, 0, 0, 0.1);
    }
  }
  &__img {
    max-width: 260px;
  }
  &__title {
    font-size: 18px;
    font-weight: 600;
  }
  &__bottom {
    display: flex;
    justify-content: space-between;
    align-items: center;
    &-price {
      display: flex;
      flex-direction: column;

      &-title {
        font-size: 14px;
        font-weight: 600;
      }
      &-value {
        font-size: 18px;
        font-weight: 600;
      }
    }
  }
}
</style>
