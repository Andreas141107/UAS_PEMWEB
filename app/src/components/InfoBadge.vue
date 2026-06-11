<script setup>
const props = defineProps({
  text: {
    type: String,
    required: true
  }
});
</script>

<template>
  <span class="info-badge">
    <svg width="12" height="12" viewBox="0 0 24 24" fill="currentColor">
      <circle cx="12" cy="12" r="6"/>
    </svg>
    {{ text }}
  </span>
</template>

<style scoped>
.info-badge {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 10px 20px;
  background: rgba(201, 162, 39, 0.15);
  border: 1px solid rgba(201, 162, 39, 0.3);
  border-radius: 9999px;
  font-family: 'Inter', sans-serif;
  font-size: 12px;
  font-weight: 500;
  letter-spacing: 0.05em;
  text-transform: uppercase;
  color: #E8D38B;
  transition: all 0.3s ease;
}

.info-badge:hover {
  background: rgba(201, 162, 39, 0.25);
  border-color: rgba(201, 162, 39, 0.5);
}

.info-badge svg {
  color: #C9A227;
}
</style>
