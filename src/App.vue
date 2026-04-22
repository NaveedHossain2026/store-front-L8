<template>
  <TopNav :cartItemCount="cartItemCount"/>
  <router-view
    :products="products"
    :cartItems="cartItems"
    @addToCart="addToCart"
    @removeFromCart="removeFromCart"
    @submitOrder="submitOrder"
  ></router-view>
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
      products: [],
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
      fetch('/products')
        .then(response => response.json())
        .then(products => {
          console.log('success getting proxy products')
          this.products = products
        })
        .catch(error => {
          console.log(error)
          alert('Error occurred while fetching products')
        })
    },
    addToCart({ productId, quantity }) {
      // check if the product is already in the cart
      const existingCartItem = this.cartItems.find(
        item => item.product.id == productId
      )
      if (existingCartItem) {
        // if it is, increment the quantity
        existingCartItem.quantity += quantity
      } else {
        // if not, find the product, and add it with quantity to the cart
        const product = this.products.find(product => product.id == productId)
        this.cartItems.push({ product, quantity })
      }
    },
    removeFromCart(index) {
      this.cartItems.splice(index, 1)
    },
    submitOrder() {
      // get the order-service URL from an environment variable
      // const orderServiceUrl = process.env.VUE_APP_ORDER_SERVICE_URL;

      // create an order object
      const order = {
        customerId: Math.floor(Math.random() * 10000000000).toString(),
        items: this.cartItems.map(item => {
          return {
            productId: item.product.id,
            quantity: item.quantity,
            price: item.product.price
          }
        })
      }

      console.log(JSON.stringify(order));

      // call the order-service using fetch
      fetch(`/order`, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(order)
      })
        .then(response => {
          console.log(response)
          if (!response.ok) {
            alert('Error occurred while submitting order')
          } else {
            this.cartItems = []
            alert('Order submitted successfully')
          }
        })
        .catch(error => {
          console.log(error)
          alert('Error occurred while submitting order')
        })
    }
  },
}
</script>

<style>
/* Best Buy Brand Identity */
:root {
  --bb-blue: #0046be;
  --bb-yellow: #fff200;
  --bb-white: #ffffff;
  --bb-black: #000000;
}

body {
  /* Remove the algonquin.jpg and use a clean, professional retail light gray */
  background-color: #f4f6f8;
  margin: 0;
  padding: 0;
}

#app {
  font-family: 'Inter', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #1d252c;
  /* Reduced margin to accommodate a standard fixed header */
  margin-top: 80px; 
}

/* Updated Header/Footer to Best Buy Blue */
header, .navbar, footer {
  background-color: var(--bb-blue) !important;
  color: var(--bb-white);
  padding: 1rem;
}

footer {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  font-size: 0.9rem;
  border-top: 4px solid var(--bb-yellow);
}

/* Button Customization - Best Buy Yellow is the "Call to Action" color */
button {
  padding: 10px 20px;
  background-color: var(--bb-yellow);
  color: var(--bb-black);
  border: none;
  border-radius: 4px;
  font-weight: bold;
  cursor: pointer;
  transition: background-color 0.2s;
}

button:hover {
  background-color: #e5d900; /* Slightly darker yellow on hover */
}

.product-card {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
  margin: 1rem;
  padding: 1.5rem;
  border: 1px solid #e0e6ef;
  border-radius: 8px;
  background-color: white;
  box-shadow: 0 2px 4px rgba(0,0,0,0.05);
}

.product-card h2 {
  font-size: 1.1rem;
  color: var(--bb-blue);
  min-height: 3rem;
}

.product-price {
  font-weight: bold;
  font-size: 1.4rem;
  color: #c70000; /* Standard retail price red */
}

.shopping-cart {
  background-color: white;
  padding: 2rem;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0,0,0,0.1);
  max-width: 1000px;
  margin: 20px auto;
}

.checkout-button {
  background-color: var(--bb-yellow);
  color: var(--bb-black);
  text-transform: uppercase;
  letter-spacing: 1px;
}
</style>