<template>
  <div id="app">
    <div class="nav-bar"></div>
    <div class="cart">
        <p>Cart({{ cart.length }})</p>
    </div>
    <Product id="product"
      :premium="premium"
      @add-cart="addCart"
      @remove-cart="editCart"
    />
  </div>
  
</template>

<script>
import Product from './components/product.vue'

export default {
  name: 'app',
  components: {
    Product
  },
  data() {
    return {
      premium: true,
      cart: []
    }
  },
  methods: {
    addCart(id) {
      this.cart.push(id);
    },
    editCart(id) {
      this.cart.pop(id);
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  margin-top: 10px;
  display: flex;
  flex-direction: column;
}

.nav-bar {
  background: linear-gradient(-90deg, #84CF6A, #16c0b0);
  height: 60px;
  width: 100%;
  margin-bottom: 15px;
}

.cart {
    align-self: flex-end;
    width: 50px;
    margin-right: 30px;
    border: 1px solid #d8d8d8;
    border-radius: 5px 15px;
    padding: 5px 20px;
}

.product {
  align-self: center;
  width: auto;
  height: 30%;
}

</style>
