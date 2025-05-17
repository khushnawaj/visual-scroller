<template>
  <div class="modal-overlay" @click.self="$emit('close')">
    <div class="modal">
      <button class="close-btn" @click="$emit('close')">✖</button>
      <img :src="user.picture.large" alt="user photo" />
      <h2>{{ user.name.first }} {{ user.name.last }}</h2>
      <p><strong>Email:</strong> {{ user.email }}</p>
      <p><strong>Phone:</strong> {{ user.phone }}</p>
      <p><strong>Location:</strong> {{ user.location.city }}, {{ user.location.country }}</p>
      <p><strong>Registered:</strong> {{ new Date(user.registered.date).toLocaleDateString() }}</p>
    </div>
  </div>
</template>

<script setup>
defineProps({ user: Object })
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(20, 50, 20, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 999;
  backdrop-filter: blur(3px);
}

.modal {
  background: #f6fff6; /* light wellness green */
  padding: 30px;
  border-radius: 18px;
  max-width: 400px;
  width: 90%;
  text-align: center;
  border: 2px solid #22543d; /* forest green */
  box-shadow: 0 8px 28px rgba(20, 60, 20, 0.2);
  font-family: 'Segoe UI', sans-serif;
  color: #22543d;
  position: relative;
  transition: all 0.3s ease;
}

.close-btn {
  position: absolute;
  top: 10px;
  right: 10px;
  border: none;
  background: none;
  font-size: 20px;
  cursor: pointer;
  color: #22543d;
  font-weight: bold;
  transition: color 0.2s ease;
}
.close-btn:hover {
  color: #2f855a;
}

img {
  width: 100px;
  height: 100px;
  border-radius: 50%;
  border: 3px solid #c6f6d5;
  margin-bottom: 16px;
  object-fit: cover;
  box-shadow: 0 0 8px rgba(0, 80, 50, 0.2);
}

p {
  margin: 6px 0;
  font-size: 14px;
}

h2 {
  margin-bottom: 10px;
  font-size: 22px;
}
</style>
