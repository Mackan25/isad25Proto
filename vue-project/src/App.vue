<script setup>
import { ref } from 'vue';
import FoodCard from './components/FoodCard.vue';
import Header from './components/Header.vue';
import Sidebar from './components/Sidebar.vue';
import HomePage from './components/HomePage.vue';
import MenuPage from './components/MenuPage.vue';
import CartPage from './components/CartPage.vue';
import CheckoutPage from './components/CheckoutPage.vue';
import sushiImg from './img/sushi.png';
import margheritaImg from './img/margherita.png';
import saladImg from './img/ceasersalad.png';
import burgerImg from './img/hamburger.png';

const sidebarOpen = ref(false);
const currentPage = ref('home');
const cartItems = ref([]);

const toggleSidebar = (isOpen) => {
  sidebarOpen.value = isOpen;
};

const closeSidebar = () => {
  sidebarOpen.value = false;
};

const navigateTo = (page) => {
  currentPage.value = page;
};

const addToCart = (item) => {
  const existingItem = cartItems.value.find(cartItem => cartItem.title === item.title);
  if (existingItem) {
    existingItem.quantity++;
  } else {
    cartItems.value.push({ ...item, quantity: 1 });
  }
};

const handleCartUpdate = ({ index, action, ingredients }) => {
  if (action === 'increment') {
    cartItems.value[index].quantity++;
  } else if (action === 'decrement') {
    cartItems.value[index].quantity--;
  } else if (action === 'remove') {
    cartItems.value.splice(index, 1);
  } else if (action === 'edit-ingredients') {
    // replace ingredients array for the item
    if (Array.isArray(ingredients)) {
      cartItems.value[index].ingredients = ingredients;
    }
  }
};

const handleCartClick = () => {
  navigateTo('cart');
};
</script>

<template>
  <div class="app">
    <Header title="Foodelicious" @toggle="toggleSidebar" @cart-click="handleCartClick" :cartCount="cartItems.length" />
    <Sidebar :isOpen="sidebarOpen" @close="closeSidebar" @navigate="navigateTo" />
    <HomePage v-if="currentPage === 'home'" @navigate="navigateTo" />
    <MenuPage v-if="currentPage === 'menu'" @add-to-cart="addToCart" />
    <CartPage v-if="currentPage === 'cart'" :items="cartItems" @update="handleCartUpdate" @navigate="navigateTo" />
    <CheckoutPage v-if="currentPage === 'checkout'" :items="cartItems" @navigate="navigateTo" />
  </div>
</template>

<style scoped>
.app {
  padding: 24px;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  background-image: url('./img/foodbackground.jpg');
  background-size: cover;
  background-position: center;
  background-attachment: fixed;
  min-height: 100vh;
  position: relative;
}

.app::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.4);
  z-index: 1;
}
</style>
