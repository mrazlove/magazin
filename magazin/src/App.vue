<template>
  <div id="app">
    <header>
      <h1 class="header-title">Каталог товаров</h1>
      <button class="cart-button" @click="toggleCart">
        Корзина ({{ cartCount }})
      </button>
    </header>
    <main>
      <ProductList :products="products" @add-to-cart="addToCart" />
    </main>
    <CartModal v-if="isCartVisible" :cart="cart" @close="toggleCart" />
  </div>
</template>

<script>
import { ref, computed } from 'vue';
import ProductList from './components/ProductList.vue';
import CartModal from './components/CartModal.vue';

export default {
  components: {
    ProductList,
    CartModal,
  },
  setup() {
    const products = ref([
      { id: 1, name: 'Товар 1', price: 100 },
      { id: 2, name: 'Товар 2', price: 300 },
    ]);
    const cart = ref([]);
    const isCartVisible = ref(false);

    const addToCart = (product) => {
      const cartItem = cart.value.find(item => item.product.id === product.id);
      if (cartItem) {
        cartItem.quantity += 1;
      } else {
        cart.value.push({ product, quantity: 1 });
      }
    };

    const toggleCart = () => {
      isCartVisible.value = !isCartVisible.value;
    };

    const cartCount = computed(() => {
      return cart.value.reduce((sum, item) => sum + item.quantity, 0);
    });

    return {
      products,
      cart,
      isCartVisible,
      addToCart,
      toggleCart,
      cartCount,
    };
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
}

.header-title {
  font-size: 24px;
}

.cart-button {
  position: fixed;
  top: 10px;
  right: 10px;
  padding: 10px;
  color: black;
}
</style>
