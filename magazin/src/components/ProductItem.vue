<template>
  <div class="product">
    <span class="product-name">{{ product.name }}</span>
    <span class="product-price">{{ product.price }} руб.</span>
    <button v-if="!isInCart" @click="addToCart">Купить</button>
    <div v-else>
      <button @click="increaseQuantity">
        кол-во {{ cartItem.quantity }}, +
      </button>
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
      return this.$root.cart.find(item => item.product.id === this.product.id);
    }
  },
  methods: {
    addToCart() {
      this.$emit('add-to-cart', this.product);
    },
    increaseQuantity() {
      this.$emit('add-to-cart', this.product);
    }
  }
};
</script>

<style>
.product {
  border: 1px solid #ccc;
  padding: 10px;
  margin: 10px;
  width: 200px;
  text-align: center;
}

.product-name, .product-price {
  display: block;
  margin: 10px 0;
}

button {
  padding: 5px 10px;
  cursor: pointer;
}
</style>
