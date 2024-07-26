<template>
    <div class="product">
      <span class="product-name">{{ product.name }}</span>
      <span class="product-price">{{ product.price }} руб.</span>
      <button v-if="!isInCart" @click="addToCart">Купить</button>
      <div v-else>
        <span>Количество: {{ cartItem.quantity }}</span>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      product: Object,
    },
    computed: {
      isInCart() {
        return !!this.cartItem;
      },
      cartItem() {
        return this.$root.cart.find(item => item.id === this.product.id);
      }
    },
    methods: {
      addToCart() {
        this.$emit('add-to-cart', this.product);
      }
    }
  };
  </script>
  