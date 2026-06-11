<script setup>
const props = defineProps({
  item: {
    type: Object,
    required: true
  }
});

const emit = defineEmits(['open-modal']);

const handleClick = () => {
  emit('open-modal', props.item);
};
</script>

<template>
  <div class="gallery-card" @click="handleClick">
    <img :src="item.image" :alt="item.title" class="gallery-card__image" />
    <div class="gallery-card__overlay">
      <svg class="gallery-card__icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
        <circle cx="11" cy="11" r="8"/>
        <path d="M21 21l-4.35-4.35" stroke-linecap="round" stroke-linejoin="round"/>
        <path d="M11 8v6M8 11h6" stroke-linecap="round" stroke-linejoin="round"/>
      </svg>
      <span class="gallery-card__title">{{ item.title }}</span>
      <span class="gallery-card__category">{{ item.category }}</span>
    </div>
  </div>
</template>

<style scoped>
.gallery-card {
  position: relative;
  border-radius: var(--radius-md);
  overflow: hidden;
  cursor: pointer;
  transition: transform 0.3s ease;
}

.gallery-card:hover {
  transform: scale(1.02);
}

.gallery-card__image {
  width: 100%;
  display: block;
  aspect-ratio: 4/3;
  object-fit: cover;
  transition: transform 0.5s ease;
}

.gallery-card:hover .gallery-card__image {
  transform: scale(1.1);
}

.gallery-card__overlay {
  position: absolute;
  inset: 0;
  background: rgba(0, 0, 0, 0.5);
  backdrop-filter: blur(4px);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: var(--space-sm);
  opacity: 0;
  transition: opacity 0.3s ease;
}

.gallery-card:hover .gallery-card__overlay {
  opacity: 1;
}

.gallery-card__icon {
  width: 32px;
  height: 32px;
  color: var(--white);
}

.gallery-card__title {
  font-family: 'Poppins', sans-serif;
  font-size: 16px;
  font-weight: 500;
  color: var(--white);
  text-align: center;
  padding: 0 var(--space-md);
}

.gallery-card__category {
  font-family: 'Inter', sans-serif;
  font-size: 11px;
  font-weight: 500;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: var(--gold);
}
</style>