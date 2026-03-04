<template>
  <header class="header">
    <div class="header-content">
      <h1 class="title">{{ title }}</h1>
      <div class="header-buttons">
        <button class="cart-btn" @click="toggleCart" title="Shopping Cart">
          <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <circle cx="9" cy="21" r="1"></circle>
            <circle cx="20" cy="21" r="1"></circle>
            <path d="M1 1h4l2.68 13.39a2 2 0 0 0 2 1.61h9.72a2 2 0 0 0 2-1.61L23 6H6"></path>
          </svg>
          <span v-if="cartCount > 0" class="cart-count">{{ cartCount }}</span>
        </button>
        <button class="hamburger" @click="toggleMenu" :class="{ active: menuOpen }">
          <span></span>
          <span></span>
          <span></span>
        </button>
      </div>
    </div>
  </header>
</template>

<script setup>
import { ref } from 'vue';

const props = defineProps({
  title: {
    type: String,
    default: 'My App'
  },
  cartCount: {
    type: Number,
    default: 0
  }
});

const menuOpen = ref(false);

const toggleMenu = () => {
  menuOpen.value = !menuOpen.value;
  emit('toggle', menuOpen.value);
};

const toggleCart = () => {
  emit('cart-click');
};

const emit = defineEmits(['toggle', 'cart-click']);
</script>

<style scoped>
.header {
  background-color: #333;
  color: white;
  padding: 16px 0;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  position: relative;
  z-index: 10;
}

.header-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 24px;
}

.title {
  margin: 0;
  font-size: 24px;
  font-weight: bold;
}

.header-buttons {
  display: flex;
  align-items: center;
  gap: 12px;
}

.cart-btn {
  background: none;
  border: none;
  cursor: pointer;
  color: white;
  padding: 8px;
  display: flex;
  justify-content: center;
  align-items: center;
  transition: opacity 0.2s ease;
  position: relative;
}

.cart-btn:hover {
  opacity: 0.8;
}

.cart-count {
  position: absolute;
  top: -5px;
  right: -5px;
  background-color: #ff6b6b;
  color: white;
  border-radius: 50%;
  width: 20px;
  height: 20px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 12px;
  font-weight: bold;
}

.hamburger {
  background: none;
  border: none;
  cursor: pointer;
  display: flex;
  flex-direction: column;
  gap: 5px;
  padding: 8px;
}

.hamburger span {
  width: 25px;
  height: 3px;
  background-color: white;
  border-radius: 2px;
  transition: all 0.3s ease;
}

.hamburger.active span:nth-child(1) {
  transform: rotate(45deg) translate(10px, 10px);
}

.hamburger.active span:nth-child(2) {
  opacity: 0;
}

.hamburger.active span:nth-child(3) {
  transform: rotate(-45deg) translate(7px, -7px);
}
</style>