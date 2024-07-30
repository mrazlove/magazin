<template>
  <div class="modal" @click="closeOnBackgroundClick">
    <div class="modal-content" @click.stop>
      <span class="close" @click="$emit('close')">&times;</span>
      <h2>Корзина</h2>
      <div v-if="cart.length === 0" class="empty-cart">
        Корзина пуста
      </div>
      <ul v-else>
        <li v-for="item in cart" :key="item.product.id">
          {{ item.product.name }} - 
          {{ item.quantity }} x {{ item.product.price }} руб.
          <button @click="$emit('increase-quantity', item.product.id)">+</button>
          <button @click="$emit('decrease-quantity', item.product.id)">-</button>
          <button @click="$emit('remove-item', item.product.id)">Удалить</button>
        </li>
      </ul>
      <div v-if="cart.length > 0">
        <p>Итоговая стоимость: {{ totalCost }} руб.</p>
        <button class="clear-cart" @click="$emit('clear-cart')">Очистить корзину</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    cart: Array,
  },
  computed: {
    totalCost() {
      return this.cart.reduce((sum, item) => sum + item.quantity * item.product.price, 0);
    }
  },
  methods: {
    closeOnBackgroundClick(event) {
      if (event.target === event.currentTarget) {
        this.$emit('close');
      }
    }
  }
};
</script>

<style>
.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  color: black;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-content {
  position: relative;
  background-color: white;
  padding: 20px;
  border-radius: 5px;
  width: 300px;
}

.close {
  position: absolute;
  top: 10px;
  right: 10px;
  cursor: pointer;
}

.empty-cart {
  text-align: center;
  margin-top: 20px;
}

.clear-cart {
  margin-top: 20px;
  padding: 10px;
  background-color: #f44336;
  color: white;
  border: none;
  cursor: pointer;
  width: 100%;
}

.clear-cart:hover {
  background-color: #d32f2f;
}
</style>