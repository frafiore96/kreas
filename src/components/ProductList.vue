<template>
  <h1>Our products</h1>
  <div>
    <div class="product-grid">
      <div
        v-for="product in filteredProducts"
        :key="product.id"
        class="product-item"
        @click="selectProduct(product)"
      >
        <img :src="product.image" :alt="product.name" />
        <h3>{{ product.name }}</h3>
        <p>{{ product.price }} €</p>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, computed, onMounted, defineComponent } from 'vue';

export default defineComponent({
  props: {
    searchQuery: String,
  },
  emits: ['select-product'],
  setup(props, { emit }) {
    const products = ref([]);

    const filteredProducts = computed(() => {
      return products.value.filter((product) =>
        product.name.toLowerCase().includes(props.searchQuery.toLowerCase())
      );
    });

    const selectProduct = (product) => {
      emit('select-product', product);
    };

    onMounted(() => {
      fetch('https://ott-fogliata.github.io/vuejs-s2i-repository/cultured-meat.json')
        .then((response) => response.json())
        .then((data) => {
          products.value = data;
        })
        .catch((error) => console.error('Errore nel recupero dei dati:', error));
    });

    return {
      products,
      filteredProducts,
      selectProduct,
    };
  },
});
</script>

<style scoped>
h1 {
    color: white;
    font-size: 30px;
    text-align: center;
  }
  .product-grid {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 15px;
    padding: 15px;
  
  }
  
  .product-item {
    background-color: #fff;
    border-radius: 20px;
    box-shadow: 0px 2px 8px rgba(0, 0, 0, 0.1);
    padding: 15px;
    padding-bottom: 5px;
    text-align: center;
  }
  
  .product-item img {
    width: 100%; 
    height: auto;
    border-radius: 8px;
  }
  
  .product-item h3 {
    margin: 10px 0 5px;
    font-size: 16px;
  }
  
  .product-item p {
    margin-top: 5px;
    color: #666;
  }
  @media only screen and (min-width:845px)

  {

    h1 {
      font-size: 40px;
    }
    .product-grid {
      margin: auto;
      width: 85%;
      
    }
    .product-item {
    padding: 30px;
    padding-bottom: 0px;
    margin-bottom: 15px;
    text-align: center;
    border-radius: 30px;
  }
  
  .product-item img {
    width: 100%; 
    height: auto;
    border-radius: 15px;
  }
  
  .product-item h3 {
    margin: 10px 0 5px;
    font-size: 20px;
  }
  
  .product-item p {
    margin-top: 5px;
    color: #666;
    font-size: 20px;
  }

  }
</style>