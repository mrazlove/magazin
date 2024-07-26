<template>
    <div class="product-list">
      <ProductItem 
        v-for="product in products" 
        :key="product.id" 
        :product="product" 
        @add-to-cart="handleAddToCart" 
      />
    </div>
  </template>
  
  <script>
  import ProductItem from './ProductItem.vue';
  
  export default {
    components: { ProductItem },
    props: {
      products: Array,
    },
    methods: {
      handleAddToCart(product) {
        this.$emit('add-to-cart', product);
      }
    }
  };
  </script>
  