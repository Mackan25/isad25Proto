<template>
  <div class="cart-page">
    <div v-if="cart.length === 0" class="empty-cart">
      <h2>Your cart is empty</h2>
      <p>Add some delicious items to get started!</p>
      <button class="continue-btn" @click="goToMenu">Continue Shopping</button>
    </div>
    <div v-else class="cart-content">
      <h2>Shopping Cart</h2>
      <div class="cart-items">
        <div v-for="(item, index) in cart" :key="index" class="cart-item">
          <img :src="item.image" :alt="item.title" class="item-img" />
          <div class="item-info">
            <h3>{{ item.title }}</h3>
            <p>{{ item.price }}</p>
            <div v-if="item.ingredients && item.ingredients.length" class="ingredient-list">
              <small>Ingredients: {{ item.ingredients.join(', ') }}</small>
            </div>
          </div>
          <div class="item-quantity">
            <button @click="decrementItem(index)">-</button>
            <span>{{ item.quantity }}</span>
            <button @click="incrementItem(index)">+</button>
          </div>
          <div class="item-actions">
            <button class="edit-btn" @click="startEdit(index)">Edit</button>
            <button class="remove-btn" @click="removeItem(index)">Remove</button>
          </div>

          <div v-if="editingIndex === index" class="edit-panel">
            <h4>Edit Ingredients</h4>
            <div class="ingredients">
              <label v-for="(ing, i) in editedIngredients" :key="i" class="ingredient-checkbox">
                <input type="checkbox" v-model="editedIngredientsChecked[i]" />
                <span>{{ ing }}</span>
              </label>
            </div>
            <div class="edit-buttons">
              <button class="save-btn" @click="saveEdit(index)">Save</button>
              <button class="cancel-btn" @click="cancelEdit">Cancel</button>
            </div>
          </div>
        </div>
      </div>
      <div class="cart-summary">
        <h3>Total: {{ cartTotal }}</h3>
        <button class="checkout-btn" @click="handleCheckout">Checkout & Pay</button>
        <button class="continue-shopping-btn" @click="goToMenu">Continue Shopping</button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue';

const props = defineProps({
  items: {
    type: Array,
    default: () => []
  }
});

import { ref } from 'vue';

const emit = defineEmits(['update', 'navigate']);

const cart = computed(() => props.items);

const editingIndex = ref(-1);
const editedIngredients = ref([]);
const editedIngredientsChecked = ref([]);

const cartTotal = computed(() => {
  return cart.value.reduce((total, item) => {
    const price = parseFloat(item.price.replace('$', ''));
    return total + (price * item.quantity);
  }, 0).toFixed(2);
});

const incrementItem = (index) => {
  emit('update', { index, action: 'increment' });
};

const decrementItem = (index) => {
  const quantity = cart.value[index].quantity;
  if (quantity > 1) {
    emit('update', { index, action: 'decrement' });
  }
};

const removeItem = (index) => {
  emit('update', { index, action: 'remove' });
};

const goToMenu = () => {
  emit('navigate', 'menu');
};

const handleCheckout = () => {
  emit('navigate', 'checkout');
};

const startEdit = (index) => {
  const item = cart.value[index];
  editingIndex.value = index;
  editedIngredients.value = item.ingredients ? [...item.ingredients] : [];
  editedIngredientsChecked.value = editedIngredients.value.map(() => true);
};

const cancelEdit = () => {
  editingIndex.value = -1;
  editedIngredients.value = [];
  editedIngredientsChecked.value = [];
};

const saveEdit = (index) => {
  // Build new ingredients list from checked items
  const newIngredients = editedIngredients.value.filter((_, i) => editedIngredientsChecked.value[i]);
  emit('update', { index, action: 'edit-ingredients', ingredients: newIngredients });
  cancelEdit();
};
</script>

<style scoped>
.cart-page {
  min-height: 80vh;
  padding: 40px 20px;
  position: relative;
  z-index: 2;
}

.empty-cart {
  text-align: center;
  background-color: rgba(51, 51, 51, 0.9);
  padding: 60px 40px;
  border-radius: 16px;
  max-width: 500px;
  margin: 0 auto;
  color: white;
}

.empty-cart h2 {
  font-size: 32px;
  margin: 0 0 16px 0;
}

.empty-cart p {
  font-size: 16px;
  margin: 0 0 24px 0;
  color: #ddd;
}

.continue-btn {
  background-color: #ff6b6b;
  color: white;
  border: none;
  padding: 12px 32px;
  font-size: 16px;
  border-radius: 8px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.continue-btn:hover {
  background-color: #ff5252;
}

.cart-content {
  max-width: 900px;
  margin: 0 auto;
}

.cart-content h2 {
  color: white;
  margin-bottom: 24px;
  font-size: 32px;
}

.cart-items {
  background-color: rgba(51, 51, 51, 0.9);
  border-radius: 12px;
  padding: 20px;
  margin-bottom: 24px;
}

.cart-item {
  display: grid;
  grid-template-columns: 80px 1fr auto auto;
  gap: 16px;
  padding: 16px;
  background-color: #222;
  border-radius: 8px;
  margin-bottom: 12px;
  color: white;
  align-items: start;
}

.cart-item > .edit-panel {
  grid-column: 1 / -1;
  margin-top: 12px;
}

.cart-item > .item-actions {
  display: flex;
  gap: 8px;
  align-items: center;
}

.item-img {
  width: 80px;
  height: 80px;
  border-radius: 8px;
  object-fit: cover;
  flex-shrink: 0;
}

.item-info {
  flex: 1;
}

.item-info h3 {
  margin: 0 0 8px 0;
  font-size: 18px;
}

.item-info p {
  margin: 0 0 8px 0;
  color: #ff6b6b;
  font-weight: bold;
}

.ingredient-list {
  margin: 8px 0 0 0;
}

.ingredient-list small {
  color: #ccc;
  display: block;
  word-break: break-word;
}

.item-quantity {
  display: flex;
  align-items: center;
  gap: 12px;
}

.item-quantity button {
  background-color: #333;
  color: white;
  border: 1px solid #555;
  width: 32px;
  height: 32px;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.2s ease;
}

.item-quantity button:hover {
  background-color: #444;
}

.item-quantity span {
  min-width: 30px;
  text-align: center;
}

.item-actions {
  display: flex;
  gap: 8px;
  margin-top: 12px;
}

.edit-btn {
  background-color: #1890ff;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.2s ease;
  font-size: 14px;
}

.edit-btn:hover {
  background-color: #0050b3;
}

.remove-btn {
  background-color: #ff6b6b;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.2s ease;
}

.remove-btn:hover {
  background-color: #ff5252;
}

.edit-panel {
  width: 100%;
  margin-top: 16px;
  padding: 16px;
  background-color: #333;
  border-radius: 8px;
  border: 1px solid #555;
}

.edit-panel h4 {
  margin: 0 0 12px 0;
  color: white;
  font-size: 16px;
}

.ingredients {
  display: flex;
  flex-direction: column;
  gap: 8px;
  margin-bottom: 12px;
}

.ingredient-checkbox {
  display: flex;
  align-items: center;
  gap: 8px;
  cursor: pointer;
  color: #ccc;
  font-size: 14px;
}

.ingredient-checkbox input[type="checkbox"] {
  cursor: pointer;
  width: 16px;
  height: 16px;
}

.edit-buttons {
  display: flex;
  gap: 8px;
}

.save-btn {
  background-color: #52c41a;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.2s ease;
  font-size: 14px;
}

.save-btn:hover {
  background-color: #45a017;
}

.cancel-btn {
  background-color: #ff6b6b;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.2s ease;
  font-size: 14px;
}

.cancel-btn:hover {
  background-color: #ff5252;
}

.cart-summary {
  background-color: rgba(51, 51, 51, 0.9);
  padding: 24px;
  border-radius: 12px;
  text-align: right;
  color: white;
}

.cart-summary h3 {
  margin: 0 0 24px 0;
  font-size: 28px;
  color: #ff6b6b;
}

.checkout-btn {
  background-color: #52c41a;
  color: white;
  border: none;
  padding: 12px 32px;
  font-size: 16px;
  border-radius: 8px;
  cursor: pointer;
  margin-right: 12px;
  transition: background-color 0.3s ease;
  font-weight: bold;
}

.checkout-btn:hover {
  background-color: #45a017;
}

.continue-shopping-btn {
  background-color: #333;
  color: white;
  border: 1px solid #555;
  padding: 12px 32px;
  font-size: 16px;
  border-radius: 8px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.continue-shopping-btn:hover {
  background-color: #444;
}
</style>