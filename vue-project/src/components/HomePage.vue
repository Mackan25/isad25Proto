<template>
  <div class="home-page">
    <div class="home-content">
      <h2>Welcome to Foodelicious</h2>
      <p>Discover our delicious selection of food</p>
      <button class="order-btn" @click="goToMenu">Order Now</button>
    </div>
  </div>
</template>

<script setup>
const emit = defineEmits(['navigate']);

const goToMenu = () => {
  emit('navigate', 'menu');
};
</script>

<style scoped>
.home-page {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 80vh;
  position: relative;
  z-index: 2;
}

.home-content {
  text-align: center;
  background-color: rgba(51, 51, 51, 0.9);
  padding: 60px 40px;
  border-radius: 16px;
  max-width: 500px;
  color: white;
}

.home-content h2 {
  font-size: 48px;
  margin: 0 0 16px 0;
}

.home-content p {
  font-size: 18px;
  margin: 0 0 32px 0;
  color: #ddd;
}

.order-btn {
  background-color: #ff6b6b;
  color: white;
  border: none;
  padding: 16px 40px;
  font-size: 18px;
  border-radius: 8px;
  cursor: pointer;
  transition: background-color 0.3s ease;
  font-weight: bold;
}

.order-btn:hover {
  background-color: #ff5252;
}
</style>