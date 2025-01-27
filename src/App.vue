<template>
  <div id="app">
    <header v-if="currentPage !== 'cart'">
      <div class="header-content">
        <img src="./assets/Kreas.verde_logo.png" alt="Kreas Logo" class="logo" />
        <div class="icons">
          <i class="bi bi-cart3" @click="showCart">
            <span v-if="cartItems.length > 0" class="cart-notification"></span>
          </i>
          <i class="bi bi-search" @click="toggleSearch"></i>
        </div>
        <div v-if="searchActive" class="search-bar">
          <input type="text" v-model="searchQuery" placeholder="Search products..." />
        </div>
      </div>
    </header>

    <ProductList
      v-if="currentPage === 'list'"
      :search-query="searchQuery"
      @select-product="showProduct"
    />
    <ProductPage
      v-if="currentPage === 'product'"
      :product="selectedProduct"
      :search-query="searchQuery"
      @back-to-list="showList"
      @add-to-cart="addToCart"
      @select-product="showProduct"
    />
    <CartPage
      v-if="currentPage === 'cart'"
      :cart-items="cartItems"
      @update-cart="updateCart"
      @back-to-list="showList"
    />
  </div>
</template>

<script>
import { ref, onMounted } from 'vue';
import ProductList from './components/ProductList.vue';
import ProductPage from './components/ProductPage.vue';
import CartPage from './components/CartPage.vue';

export default {
  components: {
    ProductList,
    ProductPage,
    CartPage,
  },
  setup() {
    const currentPage = ref('list');
    const selectedProduct = ref(null);
    const searchActive = ref(false);
    const searchQuery = ref('');
    const cartItems = ref(JSON.parse(localStorage.getItem('cart')) || []);

    const showList = () => {
      currentPage.value = 'list';
      searchActive.value = false;
      searchQuery.value = '';
    };

    const showProduct = (product) => {
      selectedProduct.value = product;
      currentPage.value = 'product';
      searchActive.value = false;
      searchQuery.value = '';
    };

    const showCart = () => {
      currentPage.value = 'cart';
    };

    const toggleSearch = () => {
      searchActive.value = !searchActive.value;
    };

    const addToCart = (product) => {
      const existingProductIndex = cartItems.value.findIndex(
        (item) => item.name === product.name
      );

      if (existingProductIndex !== -1) {
        cartItems.value[existingProductIndex].quantity += 1;
      } else {
        cartItems.value.push({ ...product, quantity: 1 });
      }

      localStorage.setItem('cart', JSON.stringify(cartItems.value));
      showCart();
    };

    const updateCart = (updatedCart) => {
      cartItems.value = updatedCart;
      localStorage.setItem('cart', JSON.stringify(cartItems.value));
    };

    onMounted(() => {
      document.addEventListener('dblclick', (e) => {
        e.preventDefault();
      });
    });

    return {
      currentPage,
      selectedProduct,
      searchActive,
      searchQuery,
      cartItems,
      showList,
      showProduct,
      showCart,
      toggleSearch,
      addToCart,
      updateCart,
    };
  },
};
</script>

<style>
@import './assets/main.css';
</style>