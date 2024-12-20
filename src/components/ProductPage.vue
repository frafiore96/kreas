<template>
      <div class="product-details">
        <i class="bi bi-arrow-left-circle arrow-left" @click="$emit('back-to-list')"></i>
        <img :src="product.image" :alt="product.name" />
        <h1>{{ product.name }}</h1>
        <p>{{ product.description }}</p>
        <p class="price">{{ product.price }} €</p>
        <button class="button-atc" @click="addToCart"><p>Add to cart</p></button>
      </div>
  </template>
  
  <script>
  export default {
    props: {
      product: Object,
    },
    methods: {
      addToCart() {
        this.$emit('add-to-cart', this.product);
      },
    },
  };
  </script>
  
<style>


.button-atc {
    cursor: pointer;
    background-color: rgb(11, 39, 18);
    border: none;
    border-radius:50px;
    border-color: white;
    box-shadow: 0px 2px 8px rgba(0, 0, 0, 0.1);
    color: white;
    padding: 0 15px 0 15px;
    width: 100%;
}

.button-atc p {
    font-family: 'Poppins', sans-serif;
    font-size: 15px;
}

.product-details {

    background-color: #fff;
    border-radius: 20px;
    box-shadow: 0px 2px 8px rgba(0, 0, 0, 0.1);
    padding: 15px;
    margin: 20px;
    text-align: center;
}

.product-details i {
    cursor: pointer;
    font-size: 30px;
}


.product-details img {
    width: 100%;
}

.product-details .price {
    font-size: 20px;
    font-weight: bold;
    
}

@media only screen and (min-width:845px) {
  .product-details {
    margin: auto;
    margin-top: 25px;
    width: 85%;
  }
}
</style>