<template>
  <nav>
    <div class="logo">
      <router-link to="/">
        <img src="/bb-logo.png" alt="Best Buy Logo" class="nav-logo">
      </router-link>
    </div>
    <button class="hamburger" @click="toggleNav">
      <span class="hamburger-icon"></span>
    </button>
    <ul class="nav-links" :class="{ 'nav-links--open': isNavOpen }">
      <li><router-link to="/" @click="closeNav">Tech Deals</router-link></li>
      <li><router-link to="/cart" @click="closeNav">Cart ({{ cartItemCount }})</router-link></li>
    </ul>
  </nav>
</template>

<script>
export default {
  name: 'TopNav',
  props: ['cartItemCount'],
  data() {
    return {
      isNavOpen: false
    }
  },
  methods: {
    toggleNav() {
      this.isNavOpen = !this.isNavOpen
    },
    closeNav() {
      this.isNavOpen = false
    }
  }
}
</script>

<style scoped>
nav {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #0046be; /* Best Buy Blue */
  color: #fff;
  padding: 0.75rem 1.5rem;
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 1000;
  border-bottom: 4px solid #fff200; /* Best Buy Yellow */
}

.nav-logo {
  height: 45px;
  width: auto;
  display: block;
}

.nav-links {
  display: flex;
  list-style: none;
  font-size: 1.1rem;
  font-weight: bold;
  gap: 20px;
}

.nav-links li a {
  color: white;
  text-decoration: none;
  transition: color 0.2s;
}

.nav-links li a:hover {
  color: #fff200;
}

/* Hamburger styles for mobile */
.hamburger {
  display: none;
  background: none;
  border: none;
  cursor: pointer;
}

.hamburger-icon {
  display: block;
  width: 25px;
  height: 3px;
  background-color: #fff;
  position: relative;
}

.hamburger-icon::before,
.hamburger-icon::after {
  content: '';
  display: block;
  width: 25px;
  height: 3px;
  background-color: #fff;
  position: absolute;
  left: 0;
}

.hamburger-icon::before { top: -8px; }
.hamburger-icon::after { bottom: -8px; }

@media (max-width: 768px) {
  .hamburger {
    display: block;
  }
  .nav-links {
    display: none;
    flex-direction: column;
    position: absolute;
    top: 100%;
    left: 0;
    right: 0;
    background-color: #0046be;
    padding: 1rem;
  }
  .nav-links--open {
    display: flex;
  }
}
</style>