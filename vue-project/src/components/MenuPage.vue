<template>
  <div class="menu-page">
    <div class="cards-container">
      <FoodCard
        v-for="(food, index) in foods"
        :key="index"
        :title="food.title"
        :description="food.description"
        :image="food.image"
        :price="food.price"
        :ingredients="food.ingredients"
        @add-to-cart="addToCart"
      />
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import FoodCard from './FoodCard.vue';
import sushiImg from '../img/sushi.png';
import margheritaImg from '../img/margherita.png';
import saladImg from '../img/ceasersalad.png';
import burgerImg from '../img/hamburger.png';

const emit = defineEmits(['add-to-cart']);

const foods = ref([
  {
    title: 'Sushi Platter',
    description: 'Assorted fresh sushi with wasabi and soy sauce.',
    image: sushiImg,
    price: '$24.99',
    ingredients: ['Rice', 'Nori', 'Salmon', 'Wasabi', 'Soy sauce']
  },
  {
    title: 'Margherita Pizza',
    description: 'Classic pizza with fresh tomatoes, mozzarella, and basil.',
    image: margheritaImg,
    price: '$12.50',
    ingredients: ['Tomato', 'Mozzarella', 'Basil', 'Olive oil']
  },
  {
    title: 'Caesar Salad',
    description: 'Crisp romaine lettuce with parmesan and croutons.',
    image: saladImg,
    price: '$8.75',
    ingredients: ['Romaine', 'Parmesan', 'Croutons', 'Caesar dressing']
  },
  {
    title: 'Cheeseburger',
    description: 'Juicy beef burger with cheddar cheese and toppings.',
    image: burgerImg,
    price: '$10.99',
    ingredients: ['Beef patty', 'Cheddar', 'Lettuce', 'Tomato', 'Bun']
  }
]);

const addToCart = (item) => {
  emit('add-to-cart', item);
};
</script>

<style scoped>
.menu-page {
  position: relative;
  z-index: 2;
}

.cards-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}
</style>