<template>
  <div class="product">

    <div class="product-image">
      <img :src="image" alt="">
    </div>

    <div class="product-info">
      <h1>{{ title }}</h1>
      <p v-if="inStock">In Stock</p>
      <p v-show="outStock">Out of Stock</p>
      <p v-show="lowStock">Low in Stock.  Only a few left!</p>
      <!-- <p>Premium Product Alert: {{ premium }}</p> -->
      <p>Price: {{ price }}</p>
      <p>Shipping: {{ shipping }}</p>
      <Details/>

      <!-- also passing the index of each variant to use it for dynamiically updating the image -->
      <p>Available colors:</p>
      <div id="box-color"
        class="color-box"
        v-for="(variant, index) in variants"  
        :key="variant.id"
        :style="{ backgroundColor: variant.variantColor }"
        @click="toggleSelect(index)"
        >
      </div>

      <button
        @click="addToCart"
        :disabled="outStock"
        :class="{ disabledButton: outStock }">Add to Cart
      </button>
      <button
        @click="removeFrCart"
        :disabled="outStock"
        :class="{ disabledButton: outStock }">Remove from Cart
      </button>

      <Tabs class="tabs" :reviews="reviews"/>

    </div>
  </div>
</template>

<script>
import Details from './product-details.vue'
import Tabs from './product-review-tabs.vue'
import { eventBus } from '@/main.js'


export default {
  name: 'Product',
  props: {
    premium: {
      type: Boolean,
      required: true
    }
  },
  components: {
    Details,
    Tabs
    },
  data() {
    return {
      product: 'Socks',
      brand: 'Vue Mastery',
      price: '$5.99',
      // image: 'https://www.vuemastery.com/images/challenges/vmSocks-green-onWhite.jpg',
      selectedVariant: 0,
      variants: [
        {
          variantId: 2234,
          variantColor: 'green',
          variantQuantity: 9,
          variantImage: 'https://www.vuemastery.com/images/challenges/vmSocks-green-onWhite.jpg'
        },
        {
          variantId: 2235,
          variantColor: 'blue',
          variantQuantity: 2,
          variantImage: 'https://www.vuemastery.com/images/challenges/vmSocks-blue-onWhite.jpg'
        }
      ],
      reviews: []
    }
  },
  methods: {
    toggleSelect(index) {
      this.selectedVariant = index;
      const elements = document.getElementsByClassName('color-box');
      elements[this.selectedVariant].classList.toggle('border-color-box');
    },
    addToCart() {  // emits the data (we want to add)
      // ** TODO ** -- whichever selectedVariant was last clicked on, gets added to cart
      // ... whenther it has been highlighted in red or not.  May need to set the state of the 
      // .. highlighted variant as the true selection that should be added
      this.$emit('add-cart', this.variants[this.selectedVariant].variantId);
    },
    removeFrCart() {  // emits the data (that we want to remove)
      this.$emit('remove-cart', this.variants[this.selectedVariant].variantId)
    },
    updateColor(index) {
      this.selectedVariant = index;  // the index denotes whichever color box is hovered on
    }
  },
  computed: {  // function objects can be used as values to attributes as long as it returns an actual value object
    title() {
      return this.brand + ' ' + this.product;
    },
    image() {
      return this.variants[this.selectedVariant].variantImage;
    },
    inStock() {
        return this.variants[this.selectedVariant].variantQuantity > 5;
    },
    lowStock() {
        return this.variants[this.selectedVariant].variantQuantity <= 5;
    },
    outStock() {
        return this.variants[this.selectedVariant].variantQuantity === 0;
    },
    shipping() {
      return this.premium ? 'FREE' : 'Standard $1.99'
    }
  },
  mounted() {
    eventBus.$on('review-submitted', productReview => {
      this.reviews.push(productReview);
    })
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only <style scoped>-->
<style>
  .product {
    display: flex;
    flex-direction: row;
  }

  img {
    border: 1px sollid #d8d8d8;
    width: 70%;
    margin: 10px;
    box-shadow: 0px 1px 1px lightgray;
  }

  .product-image {
    flex-basis: 400px;
  }

  .product-info {
    margin-top: 10px;
    margin-right: 20px;
    flex-basis: 500px;
  }

  .color-box {
    width: 40px;
    height: 40px;
    border-radius: 10px 10px;
    margin-top: 5px;
  }

  .border-color-box {
    border: 2px solid red;
    width: 40px;
    height: 40px;
    border-radius: 10px 10px;
    margin-top: 5px;
  }

  button {
    display: flex;
    margin-top: 30px;
    padding-top: 5px;
    border: none;
    border-radius: 5px 15px;
    background-color:rgb(51, 52, 97);
    color: #1dbf81;
    height: 50px;
    width: 80px;
    font-size: 14px;
  }

  .disabledButton {
    background-color: #d8d8d8;
  }

  .tabs {
    padding-top: 50px;
  }

  .review-form {
    width: 100%;
    padding: 20px;
    border: 1px solid #d8d8d8;
  }

  input {
    width: 100%;
    height: 25px;
    margin-bottom: 20px;
  }

  textarea {
    width: 100%;
    height: 60px;
  }

  a {
    color: #42b983;
  }
</style>
