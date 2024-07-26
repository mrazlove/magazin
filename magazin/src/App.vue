<template>
  <div id="app">
    <header>
      <h1 class="header_cotolog">Католог товаров</h1>
      <button
      @click="toggleCart">Корзина
    ({{ cartCount }})</button>
    </header>
    <main>
      <ProductList :products="products"
      @add-to-cart="AddtoCart"/>
    </main>
    <CartModel :cart="cart"
    v-if="isCartVisible" @click="toggleCart"/>
  </div>
</template>

<script>
import { ref } from 'vue';
import ProductList from './components/ProductList.vue';
import CartModal from './components/CartModal.vue';

export default{
  components: {
    ProductList,
    CartModal,
  },
  setup() {
    const products = ref([
      { id: 1, name: 'Товар 1', 
        price: 100 },
        { id1: 2, name: 'Товар 2', 
        price: 300 },
    ]);
    const cart = ref([]);
    const isCartVisible = ref(false);

    const addToCart = (product) => {
      const cartItem = cart.value.find(item => item.id === product.id);
      if (cartItem) {
        cartItem.quantity += 1;
      } else {
        cart.value.push({ ...product, quantity: 1 });
      }
    };

    const toggleCart = () => {
      isCartVisible.value = !isCartVisible.value;
    };

    return {
      products,
      cart,
      isCartVisible,
      addToCart,
      toggleCart,
      get cartCount() {
        return cart.value.reduce((sum, item) => sum + item.quantity, 0);
      },
    };
  },
};
</script>