<template>
  <div>
    <h1>Cart</h1>
    <div class="back-to-list">
      <i class="bi bi-arrow-left-circle arrow-left" @click="$emit('back-to-list')"></i>
    </div>
    <p class="adv">Buy more than 3 products for a 10% discount.</p>
    <div v-for="(product, index) in cartItems" :key="index" class="cart-item">
      <img :src="product.image" :alt="product.name" class="thumbnail" />
      <h3>{{ product.name }}</h3>
      <div class="quantity-control">
        <button @click="decreaseQuantity(index)">-</button>
        <span>{{ product.quantity }}</span>
        <button @click="increaseQuantity(index)">+</button>
      </div>
      <!-- Arrotonda il prezzo totale del prodotto e lo formatta con due decimali -->
      <p class="price.each">€ {{ (product.price * product.quantity).toFixed(2) }}</p>
      <i class="bi bi-x-circle-fill" @click="removeProduct(index)"></i>
    </div>
    <div class="cart-summary">
      <!-- Totale scontato, arrotondato e formattato con due decimali -->
      <h3>Total: € {{ discountedTotal }}</h3>
      <!-- Messaggio di sconto -->
      <p v-if="totalQuantity > 3">Hai ricevuto uno sconto del 10%!</p>
    </div>
    <button class="check-out-btn" @click="checkout">Check out</button>
    
  </div>
</template>

<script>
export default {
  props: {
    cartItems: Array,
  },
  computed: {
    // Calcola il totale senza arrotondamento
    total() {
      return this.cartItems.reduce(
        (sum, product) => sum + product.price * product.quantity,
        0
      );
    },
    // Somma il numero totale di unità ordinate (quantità)
    totalQuantity() {
      return this.cartItems.reduce((sum, product) => sum + product.quantity, 0);
    },
    // Applica lo sconto del 10% se la quantità totale supera 3
    discountedTotal() {
      const totalWithDiscount = this.totalQuantity > 3 ? this.total * 0.9 : this.total;
      return totalWithDiscount.toFixed(2); // Arrotonda a 2 decimali
    },
  },
  methods: {
    increaseQuantity(index) {
      this.cartItems[index].quantity++;
      this.$emit('update-cart', this.cartItems);
    },
    decreaseQuantity(index) {
      if (this.cartItems[index].quantity > 1) {
        this.cartItems[index].quantity--;
        this.$emit('update-cart', this.cartItems);
      }
    },
    removeProduct(index) {
      this.cartItems.splice(index, 1);
      this.$emit('update-cart', this.cartItems);
    },
    checkout() {
      alert('Thank you for purchasing our products!');
      this.$emit('update-cart', []); // Svuota il carrello
    },
  },
};
</script>

<style scoped>

h1 {
  color: white;
  font-size: 30px;
  margin-left: 20px;
}

.adv {
  color: white;
  font-size: 10px;
  margin-left: 20px;
  margin-bottom: -10px;
}

.back-to-list i {
  color: white;
  margin-left: 20px;
  font-size: 30px;
  cursor: pointer;
}
.cart-item {
  background-color: white;
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-top: 15px;
  margin-bottom: 15px;
  width: 100%;
  height: auto;
  padding: 10px;
}
.cart-item img {
  width: 50px;
  height: 50px;
  object-fit: cover;
  margin-left: 10px;
  border-radius: 30px;
}

.cart-item h3 {
  font-size: 12px;
}
.quantity-control {
  display: flex;
  align-items: center;
}
.quantity-control button {
  cursor: pointer;
  margin: 0 10px;
  border: none;
}
.cart-item i {
  color: rgb(150, 9, 9);
  border: none;
  padding: 2px 10px;
  margin-right: 20px;
  cursor: pointer;
  font-family: 'Poppins', sans-serif ;
}
.cart-summary {
  display: flex;
  align-items: flex-end;
  justify-content: center;
  flex-direction: column;
  margin-right: 30px;
}

.cart-summary h3 {
  color:white;
  font-size: 20px;
  align-items:flex-start;
}

.cart-summary p {
  color: white;
  font-size: 15px;
  margin-top: -20px;
  font-style: italic;
  font-weight: 300;
}

.check-out-btn {
  border: none;
  font-family: 'Poppins', sans-serif ;
  cursor: pointer;
  width: 100%;
  margin-top: 10px;
  padding: 15px;
  font-size: 15px;
  font-weight: 500;
}

@media only screen and (min-width:845px) {

  h1 {
    font-size: 40px;
  }

  .adv {
    font-size: 15px;
  }
  .cart-item {
  margin-top: 35px;
  margin-bottom: 25px;
  padding: 15px;
}

.cart-item img {
  width: 140px;
  height: 140px;
  border-radius: 70px;
}
.cart-item h3 {
  font-size: 30px;
}
.quantity-control {
font-size: 30px;
}
.quantity-control button {
  font-size: 30px;
}
.cart-item i {
font-size: 30px;
}
p  {
  font-size: 30px;
}
.cart-summary {
  display: flex;
  align-items: flex-end;
  justify-content: center;
  flex-direction: column;
  margin-right: 30px;
}

.cart-summary h3 {
  font-size: 30px;
}

.cart-summary p {
  font-size: 20px;
}

.check-out-btn {
  margin: auto;
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 50px;
  font-size: 20px;
  width: 85%;
}
}
</style>

  