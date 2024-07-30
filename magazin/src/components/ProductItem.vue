<template>
  <div class="product">
    <span class="product-name">{{ product.name }}</span>
    <span class="product-price">{{ product.price }} руб.</span>
    <div v-if="isInCart" class="cart-controls">
      <button @click="decreaseQuantity" class="quantity-button">-</button>
      <input type="number" :value="cartItem.quantity" readonly class="quantity-input" />
      <button @click="increaseQuantity" class="quantity-button">+</button>
    </div>
    <button v-else @click="addToCart" class="add-button">Купить</button>
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
      this.$emit('add-to-cart', this.product); // Reuse the add-to-cart event to increase quantity
    },
    decreaseQuantity() {
      if (this.cartItem.quantity > 1) {
        this.$emit('decrease-quantity', this.product.id);
      } else {
        this.$emit('remove-item', this.product.id); // Optionally remove item if quantity becomes 0
      }
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

.add-button,
.quantity-button {
  padding: 5px 10px;
  cursor: pointer;
}

.cart-controls {
  display: flex;
  justify-content: center;
  align-items: center;
}

.quantity-input {
  width: 50px;
  text-align: center;
  border: none;
  background-color: transparent;
  font-size: 16px;
  margin: 0 5px;
}
</style>