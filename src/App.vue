change app.vue:
 
<template>
<div id="app">
<!-- Top Navigation -->
<TopNav :cartItemCount="cartItemCount" />
 
    <!-- Main page content (pushed below fixed nav) -->
<main class="page">
<router-view

        :products="products"

        :cartItems="cartItems"

        @addToCart="addToCart"

        @removeFromCart="removeFromCart"

        @submitOrder="submitOrder"

      />
</main>
</div>
</template>
 
<script>

import TopNav from './components/TopNav.vue'
 
export default {

  name: 'App',

  components: {

    TopNav

  },

  data() {

    return {

      cartItems: [],

      products: []

    }

  },

  computed: {

    cartItemCount() {

      return this.cartItems.reduce((total, item) => {

        return total + item.quantity

      }, 0)

    }

  },

  mounted() {

    this.getProducts()

  },

  methods: {

    getProducts() {

      // ✅ IMPORTANT: fetch from API endpoint, not '/'

      fetch('/products')

        .then(response => {

          if (!response.ok) {

            throw new Error('Failed to fetch products')

          }

          return response.json()

        })

        .then(products => {

          console.log('Products loaded successfully')

          this.products = products

        })

        .catch(error => {

          console.error(error)

          alert('Error occurred while fetching products')

        })

    },
 
    addToCart({ productId, quantity }) {

      const existingCartItem = this.cartItems.find(

        item => item.product.id == productId

      )
 
      if (existingCartItem) {

        existingCartItem.quantity += quantity

      } else {

        const product = this.products.find(p => p.id == productId)

        if (product) {

          this.cartItems.push({ product, quantity })

        }

      }

    },
 
    removeFromCart(index) {

      this.cartItems.splice(index, 1)

    },
 
    submitOrder() {

      const order = {

        customerId: Math.floor(Math.random() * 10000000000).toString(),

        items: this.cartItems.map(item => ({

          productId: item.product.id,

          quantity: item.quantity,

          price: item.product.price

        }))

      }
 
      console.log('Submitting order:', order)
 
      fetch('/order', {

        method: 'POST',

        headers: {

          'Content-Type': 'application/json'

        },

        body: JSON.stringify(order)

      })

        .then(response => {

          if (!response.ok) {

            throw new Error('Order submission failed')

          }

          this.cartItems = []

          alert('Order submitted successfully')

        })

        .catch(error => {

          console.error(error)

          alert('Error occurred while submitting order')

        })

    }

  }

}
</script>
 
<style>

/* Background */

body {

  background-image: url('@/assets/bby-logo.png');

  background-size: cover;

  background-position: center;

  background-attachment: fixed;

  margin: 0;

  padding: 0;

}
 
/* App wrapper */

#app {

  font-family: Avenir, Helvetica, Arial, sans-serif;

  text-align: center;

  color: #2c3e50;

  margin: 0;

}
 
/* Push content below fixed TopNav */

.page {

  padding-top: 80px; /* match TopNav height */

}
 
/* Buttons */

button {

  padding: 10px;

  background-color: #005f8b;

  color: #fff;

  border: none;

  border-radius: 5px;

  cursor: pointer;

  height: 42px;

}
 
/* Product grid */

.product-list {

  display: grid;

  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));

}
 
.product-card {

  display: flex;

  flex-direction: column;

  align-items: center;

  justify-content: space-between;

  margin: 1rem;

  padding: 1rem;

  border: 1px solid #ccc;

  border-radius: 0.5rem;

  background-color: rgba(255, 255, 255, 0.9);

}
 
.product-card img {

  max-width: 100%;

  margin-bottom: 1rem;

}
 
.product-card h2 {

  font-weight: bold;

  margin-bottom: 0.5rem;

}
 
.product-price {

  font-weight: bold;

  font-size: 1.2rem;

}
 
/* Cart */

.shopping-cart {

  display: flex;

  flex-direction: column;

  align-items: center;

  background-color: rgba(255, 255, 255, 0.9);

}
</style>

 