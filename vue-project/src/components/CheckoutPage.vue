<template>
  <div class="checkout-page">
    <div class="checkout-container">
      <h2>Delivery Options</h2>
      <p>How would you like to receive your order?</p>
      
      <div class="options">
        <label class="option-card" :class="{ selected: deliveryMethod === 'pickup' }">
          <input type="radio" v-model="deliveryMethod" value="pickup" />
          <div class="option-content">
            <h3>🏪 Pickup</h3>
            <p>Pick up your order at our store</p>
            <p class="time">Ready in 30 minutes</p>
          </div>
        </label>

        <label class="option-card" :class="{ selected: deliveryMethod === 'delivery' }">
          <input type="radio" v-model="deliveryMethod" value="delivery" />
          <div class="option-content">
            <h3>🚗 Delivery</h3>
            <p>Get your order delivered to your home</p>
            <p class="time">Delivery in 45 minutes</p>
          </div>
        </label>
      </div>

      <div v-if="deliveryMethod === 'delivery'" class="delivery-form">
        <h3>Delivery Address</h3>
        <div class="form-group">
          <label>Street Address</label>
          <input v-model="address" type="text" placeholder="Enter your street address" />
        </div>
        <div class="form-group">
          <label>Phone Number</label>
          <input v-model="phone" type="tel" placeholder="Enter your phone number" />
        </div>
      </div>

      <div class="form-group">
        <label>Special Instructions (Optional)</label>
        <textarea v-model="instructions" placeholder="Add any special requests..." rows="3"></textarea>
      </div>

      <div class="order-summary">
        <h3>Order Summary</h3>
        <p v-if="cartItems.length > 0">
          <strong>Items: {{ cartItems.length }}</strong><br />
          <strong>Total: ${{ cartTotal }}</strong>
        </p>
      </div>

      <div class="actions">
        <button class="confirm-btn" @click="confirmOrder">Confirm Order</button>
        <button class="back-btn" @click="goBack">Go Back</button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const props = defineProps({
  items: {
    type: Array,
    default: () => []
  }
});

const emit = defineEmits(['navigate', 'confirm']);

const deliveryMethod = ref('pickup');
const address = ref('');
const phone = ref('');
const instructions = ref('');

const cartItems = computed(() => props.items);

const cartTotal = computed(() => {
  return cartItems.value.reduce((total, item) => {
    const price = parseFloat(item.price.replace('$', ''));
    return total + (price * item.quantity);
  }, 0).toFixed(2);
});

const confirmOrder = () => {
  const orderData = {
    deliveryMethod: deliveryMethod.value,
    address: deliveryMethod.value === 'delivery' ? address.value : null,
    phone: deliveryMethod.value === 'delivery' ? phone.value : null,
    instructions: instructions.value,
    items: cartItems.value,
    total: cartTotal.value
  };
  
  emit('confirm', orderData);
  location.reload();
};

const goBack = () => {
  emit('navigate', 'cart');
};
</script>

<style scoped>
.checkout-page {
  min-height: 80vh;
  padding: 40px 20px;
  position: relative;
  z-index: 2;
}

.checkout-container {
  max-width: 600px;
  margin: 0 auto;
  background-color: rgba(51, 51, 51, 0.9);
  padding: 40px;
  border-radius: 12px;
  color: white;
}

.checkout-container h2 {
  font-size: 32px;
  margin: 0 0 8px 0;
}

.checkout-container > p {
  margin: 0 0 32px 0;
  color: #ddd;
}

.options {
  display: flex;
  flex-direction: column;
  gap: 16px;
  margin-bottom: 32px;
}

.option-card {
  display: flex;
  align-items: center;
  gap: 16px;
  padding: 16px;
  background-color: #222;
  border-radius: 8px;
  border: 2px solid #444;
  cursor: pointer;
  transition: all 0.3s ease;
}

.option-card:hover {
  border-color: #ff6b6b;
  background-color: #2a2a2a;
}

.option-card.selected {
  border-color: #ff6b6b;
  background-color: rgba(255, 107, 107, 0.1);
}

.option-card input[type="radio"] {
  width: 20px;
  height: 20px;
  cursor: pointer;
  flex-shrink: 0;
}

.option-content {
  flex: 1;
}

.option-content h3 {
  margin: 0 0 4px 0;
  font-size: 18px;
}

.option-content p {
  margin: 0 0 4px 0;
  color: #ccc;
  font-size: 14px;
}

.option-content p.time {
  color: #ff6b6b;
  font-weight: bold;
  font-size: 12px;
}

.delivery-form {
  background-color: #2a2a2a;
  padding: 20px;
  border-radius: 8px;
  margin-bottom: 24px;
}

.delivery-form h3 {
  margin: 0 0 16px 0;
  font-size: 18px;
}

.form-group {
  margin-bottom: 16px;
}

.form-group label {
  display: block;
  margin-bottom: 8px;
  color: #fff;
  font-weight: bold;
}

.form-group input,
.form-group textarea {
  width: 100%;
  padding: 12px;
  background-color: #333;
  color: white;
  border: 1px solid #555;
  border-radius: 4px;
  font-family: inherit;
  font-size: 14px;
}

.form-group input:focus,
.form-group textarea:focus {
  outline: none;
  border-color: #ff6b6b;
  background-color: #3a3a3a;
}

.form-group input::placeholder,
.form-group textarea::placeholder {
  color: #999;
}

.order-summary {
  background-color: #2a2a2a;
  padding: 16px;
  border-radius: 8px;
  margin-bottom: 24px;
}

.order-summary h3 {
  margin: 0 0 12px 0;
  font-size: 16px;
}

.order-summary p {
  margin: 0;
  color: #ff6b6b;
  font-weight: bold;
}

.actions {
  display: flex;
  gap: 12px;
  flex-direction: column;
}

.confirm-btn {
  background-color: #52c41a;
  color: white;
  border: none;
  padding: 14px 30px;
  font-size: 16px;
  border-radius: 8px;
  cursor: pointer;
  transition: background-color 0.3s ease;
  font-weight: bold;
}

.confirm-btn:hover {
  background-color: #45a017;
}

.back-btn {
  background-color: #333;
  color: white;
  border: 1px solid #555;
  padding: 14px 30px;
  font-size: 16px;
  border-radius: 8px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.back-btn:hover {
  background-color: #444;
}
</style>