<template>
  <div id="app">
    <header>
      <div class="header-content">
        <div class="logo-title">
          <img src="@/assets/bby-logo.png" alt="Best Buy Logo" class="logo" />
          <h1 class="title">Best Buy Cloud-Native Store</h1>
        </div>

        <nav>
          <ul>
            <li><a href="#">Products</a></li>
            <li><a href="#">Cart ({{ cartCount }})</a></li>
          </ul>
        </nav>
      </div>
    </header>

    <!-- Product Grid -->
    <div class="product-grid">
      <div class="product-card" v-for="p in products" :key="p.id">
        <img :src="p.image" class="product-image" alt="Product Image" />
        <h3>{{ p.name }}</h3>
        <p class="price">$ {{ p.price }}</p>

        <button @click="addToCart(p)">Add to Cart</button>
      </div>
    </div>

    <footer>
      <p>&copy; 2025 Best Buy Cloud-Native App. All rights reserved.</p>
    </footer>
  </div>
</template>

<script>
export default {
  data() {
    return {
      products: [],
      cartItems: [],
      cartCount: 0
    };
  },

  methods: {
    // -----------------------------------------
    // Fetch products from Product-Service (BestBuy version)
    // -----------------------------------------
    async fetchProducts() {
      try {
        const res = await fetch("http://product-service.default.svc.cluster.local/products");
        const data = await res.json();

        // Expecting your Product-Service to return:
        // [{ id, name, price, image }]
        this.products = data;
      } catch (err) {
        console.error("❌ Failed to fetch products:", err);
      }
    },

    // -----------------------------------------
    // Add product to cart
    // -----------------------------------------
    addToCart(product) {
      this.cartItems.push(product);
      this.cartCount = this.cartItems.length;
    }
  },

  created() {
    this.fetchProducts();
  }
};
</script>

<style>
/* Light gray background for modern UI */
body {
  background-color: #f7f7f7;
  margin: 0;
  padding: 0;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  margin-top: 100px;
  padding-bottom: 50px;
}

/* HEADER */
header {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  background-color: #004694; /* Best Buy Blue */
  padding: 0 20px;
  z-index: 1000;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.header-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
  max-width: 1200px;
  margin: auto;
}

.logo-title {
  display: flex;
  align-items: center;
}

.logo {
  height: 40px;
  margin-right: 15px;
}

.title {
  color: #fff;
  font-size: 1.5rem;
  margin: 0;
}

nav ul {
  display: flex;
  list-style: none;
}

nav li {
  margin-left: 20px;
}

nav a {
  color: white;
  font-weight: bold;
  text-decoration: none;
}

nav a:hover {
  color: #FFC600;
}

/* FOOTER */
footer {
  position: fixed;
  bottom: 0;
  width: 100%;
  background-color: #004694;
  color: white;
  padding: 0.5rem;
}

/* PRODUCT GRID */
.product-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 25px;
  padding: 40px;
  max-width: 1200px;
  margin: auto;
  margin-bottom: 100px;
}

.product-card {
  background: white;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 2px 6px rgba(0,0,0,0.1);
}

.product-image {
  width: 100%;
  height: 180px;
  object-fit: contain;
}

.price {
  font-size: 1.2rem;
  font-weight: bold;
  color: #004694;
}

/* Buttons (Best Buy Yellow) */
button {
  padding: 10px;
  background-color: #FFC600;
  color: #000;
  border: none;
  border-radius: 5px;
  font-weight: bold;
  cursor: pointer;
}

button:hover {
  background-color: #002D59;
  color: white;
}
</style>
