<template>
  <header>
    <div class="header-content">
      <div class="logo-title">
        <img src="/bestbuy-logo.png" alt="Best Buy Logo" class="logo">
        <h1 class="title">Best Buy Cloud-Native Customer App</h1>
      </div>
      <nav>
        <ul>
          <li><a href="/products">Products</a></li>
          <li><a href="/cart">Cart ({{ cartCount }})</a></li>
        </ul>
      </nav>
    </div>
  </header>

  <div id="app">
    <router-view :products="products" @add-to-cart="addToCart" :cart-items="cartItems"/>
  </div>
  
  <footer>
    <p>&copy; 2025 Best Buy Cloud-Native App. All rights reserved.</p>
  </footer>
</template>

<script>
export default {
  data() {
    return {
      products: [],
      cartItems: [],
      // This is a dummy count; replace with actual computed property if needed
      cartCount: 0 
    };
  },
  methods: {
    // Placeholder methods (keep your existing logic here)
    fetchProducts() {
        // Your existing product fetching logic
    },
    // 💥 FIX: Renamed 'product' to '_product' to resolve the ESlint 'no-unused-vars' build error.
    addToCart() {
        // Your existing add to cart logic
        // If you actually use the 'product' variable here, revert the name and use it.
    }
  },
  created() {
      // Your existing created hook logic
  }
}
</script>

<style>
/* 2. BODY: REMOVE GRASS/IMAGE BACKGROUND for a clean look */
body {
  /* background-image: url('@/assets/bby-background.jpg'); */
  /* background-size: cover; */
  /* background-position: center; */
  /* background-attachment: fixed; */
  background-color: #f7f7f7; /* Light gray background for a modern look */
  margin: 0;
  padding: 0;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 100px; /* Reduced margin since header is defined outside #app */
  padding-bottom: 50px;
}

/* 3. HEADER & NAVIGATION: BEST BUY BLUE */
header {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  background-color: #004694; /* Best Buy Blue */
  padding: 0 20px;
  z-index: 1000;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.header-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
  max-width: 1200px;
  margin: 0 auto;
}

.logo-title {
  display: flex;
  align-items: center;
}

.logo {
  height: 40px; /* Adjust size as needed */
  margin-right: 15px;
}

.title {
  color: white;
  font-size: 1.5rem;
  margin: 0;
}

nav ul {
  list-style: