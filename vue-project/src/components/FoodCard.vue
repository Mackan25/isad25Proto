<template>
  <div class="food-card">
    <img v-if="image" :src="image" :alt="title" class="food-img" />
    <h2>{{ title }}</h2>
    <p>{{ description }}</p>
    <p class="price" v-if="price">Price: {{ price }}</p>
    <button class="add-cart-btn" @click="addToCart">Add to Cart</button>
  </div>
</template>

<script setup>
const props = defineProps({
  title: String,
  description: String,
  image: String,
  price: String,
  ingredients: Array
});

const emit = defineEmits(['add-to-cart']);

const addToCart = () => {
  emit('add-to-cart', {
    title: props.title,
    description: props.description,
    image: props.image,
    price: props.price,
    ingredients: props.ingredients || []
  });
};
</script>

<style scoped>
.food-card {
  background-color: #333;
  color: white;
  border: 1px solid #444;
  border-radius: 8px;
  padding: 16px;
  max-width: 300px;
  text-align: center;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  margin: 8px;
  display: flex;
  flex-direction: column;
}

.food-img {
  max-width: 100%;
  border-radius: 4px;
  margin-bottom: 12px;
}

.price {
  font-weight: bold;
  color: #fff;
  margin: 8px 0;
}

.add-cart-btn {
  background-color: #ff6b6b;
  color: white;
  border: none;
  padding: 10px 16px;
  border-radius: 6px;
  cursor: pointer;
  font-size: 14px;
  font-weight: bold;
  transition: background-color 0.3s ease;
  margin-top: auto;
}

.add-cart-btn:hover {
  background-color: #ff5252;
}
</style>