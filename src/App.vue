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
import ProductList from './components/ProductList.vue';
import ProductPage from './components/ProductPage.vue';
import CartPage from './components/CartPage.vue';

document.addEventListener('dblclick', (e) => {
  e.preventDefault();
});

export default {
  components: {
    ProductList,
    ProductPage,
    CartPage,
  },
  data() {
    return {
      currentPage: 'list', // Can be 'list', 'product' or 'cart'
      selectedProduct: null,
      searchActive: false,
      searchQuery: '',
      cartItems: JSON.parse(localStorage.getItem('cart')) || [], 
    };
  },
  methods: {
    showList() {
      this.currentPage = 'list';
      this.searchActive = false;
      this.searchQuery = '';
    },
    showProduct(product) {
      this.selectedProduct = product;
      this.currentPage = 'product';
      this.searchActive = false;
      this.searchQuery = '';
    },
    showCart() {
      this.currentPage = 'cart';
    },
    toggleSearch() {
      this.searchActive = !this.searchActive;
    },
     addToCart(product) {
      const existingProductIndex = this.cartItems.findIndex(item => item.name === product.name);

      if (existingProductIndex !== -1) {
        this.cartItems[existingProductIndex].quantity += 1;
      } else {
        this.cartItems.push({ ...product, quantity: 1 });
      }
      localStorage.setItem('cart', JSON.stringify(this.cartItems));
      this.showCart();
    },
    updateCart(updatedCart) {
      this.cartItems = updatedCart;
      localStorage.setItem('cart', JSON.stringify(this.cartItems));
    },
  },
};
</script>

<style>
@import './assets/main.css';
</style>

