<template>
  <div class="sidebar-overlay" v-if="isOpen" @click="close"></div>
  <div class="sidebar" :class="{ open: isOpen }">
    <nav class="sidebar-nav">
      <a href="#" @click.prevent="navigate('home')" class="nav-link">Home</a>
      <a href="#" @click.prevent="navigate('menu')" class="nav-link">Menu</a>
      <a href="#" @click.prevent="navigate('cart')" class="nav-link">Cart</a>
    </nav>
  </div>
</template>

<script setup>
defineProps({
  isOpen: {
    type: Boolean,
    default: false
  }
});

const emit = defineEmits(['close', 'navigate']);

const close = () => {
  emit('close');
};

const navigate = (page) => {
  emit('navigate', page);
  close();
};
</script>

<style scoped>
.sidebar-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.5);
  z-index: 998;
  animation: fadeIn 0.3s ease;
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

.sidebar {
  position: fixed;
  top: 0;
  right: -300px;
  width: 300px;
  height: 100vh;
  background-color: #222;
  box-shadow: -2px 0 8px rgba(0, 0, 0, 0.2);
  z-index: 999;
  transition: right 0.3s ease;
}

.sidebar.open {
  right: 0;
}

.sidebar-nav {
  display: flex;
  flex-direction: column;
  padding-top: 80px;
}

.nav-link {
  padding: 16px 24px;
  color: white;
  text-decoration: none;
  border-bottom: 1px solid #444;
  transition: background-color 0.2s ease;
  font-size: 18px;
}

.nav-link:hover {
  background-color: #333;
}
</style>