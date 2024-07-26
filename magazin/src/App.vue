<template>
  <div id="app">
    <header>
      <h1 class="header-title">Каталог товаров</h1>
      <div class="search-sort-container">
        <input v-model="searchQuery" placeholder="Поиск по имени" />
        <select v-model="sortOrder" @change="sortProducts">
          <option value="asc">Сортировать от А до Я</option>
          <option value="desc">Сортировать от Я до А</option>
        </select>
      </div>
      <button class="cart-button" @click="toggleCart">
        Корзина ({{ cartCount }})
      </button>
    </header>
    <main>
      <ProductList :products="filteredProducts" @add-to-cart="addToCart" />
    </main>
    <CartModal 
      v-if="isCartVisible" 
      :cart="cart" 
      @close="toggleCart" 
      @increase-quantity="increaseQuantity"
      @decrease-quantity="decreaseQuantity"
      @remove-item="removeItem"
    />
  </div>
</template>

<script>
import { ref, computed, watch } from 'vue';
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
    const searchQuery = ref('');
    const sortOrder = ref('asc');

    const addToCart = (product) => {
      const cartItem = cart.value.find(item => item.product.id === product.id);
      if (cartItem) {
        cartItem.quantity += 1;
      } else {
        cart.value.push({ product, quantity: 1 });
      }
    };

    const increaseQuantity = (productId) => {
      const cartItem = cart.value.find(item => item.product.id === productId);
      if (cartItem) {
        cartItem.quantity += 1;
      }
    };

    const decreaseQuantity = (productId) => {
      const cartItem = cart.value.find(item => item.product.id === productId);
      if (cartItem && cartItem.quantity > 1) {
        cartItem.quantity -= 1;
      }
    };

    const removeItem = (productId) => {
      cart.value = cart.value.filter(item => item.product.id !== productId);
    };

    const toggleCart = () => {
      isCartVisible.value = !isCartVisible.value;
    };

    const cartCount = computed(() => {
      return cart.value.reduce((sum, item) => sum + item.quantity, 0);
    });

    const filteredProducts = computed(() => {
      let filtered = products.value.filter(product =>
        product.name.toLowerCase().includes(searchQuery.value.toLowerCase())
      );
      if (sortOrder.value === 'asc') {
        filtered.sort((a, b) => a.name.localeCompare(b.name));
      } else {
        filtered.sort((a, b) => b.name.localeCompare(a.name));
      }
      return filtered;
    });

    return {
      products,
      cart,
      isCartVisible,
      searchQuery,
      sortOrder,
      addToCart,
      toggleCart,
      cartCount,
      increaseQuantity,
      decreaseQuantity,
      removeItem,
      filteredProducts,
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
}

.search-sort-container {
  margin: 10px;
  margin-right: 30px;
}

.search-sort-container input,
.search-sort-container select {
  margin: 0 10px;
  padding: 5px;
  margin-right: 30px;
}
</style>